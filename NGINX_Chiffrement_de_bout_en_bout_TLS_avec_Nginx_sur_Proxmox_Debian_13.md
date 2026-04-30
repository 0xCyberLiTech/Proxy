<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3EPROXY_" alt="Titre dynamique PROXY" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT.</h2>

  <p align="center">
    <a href="https://0xcyberlitech.github.io/">
      <img src="https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square" alt="🌐 Portfolio" />
    </a>
    <a href="https://github.com/0xCyberLiTech">
      <img src="https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square" alt="🔗 Profil GitHub" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Proxy/releases/latest">
      <img src="https://img.shields.io/github/v/release/0xCyberLiTech/Proxy?label=version&style=flat-square&color=blue" alt="📦 Dernière version" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Proxy/blob/main/CHANGELOG.md">
      <img src="https://img.shields.io/badge/📄%20Changelog-Proxy-blue?style=flat-square" alt="📄 CHANGELOG Proxy" />
    </a>
    <a href="https://github.com/0xCyberLiTech?tab=repositories">
      <img src="https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square" alt="📂 Dépôts publics" />
    </a>
    <a href="https://github.com/0xCyberLiTech/Proxy/graphs/contributors">
      <img src="https://img.shields.io/badge/👥%20Contributeurs-cliquez%20ici-007ec6?style=flat-square" alt="👥 Contributeurs Proxy" />
    </a>
  </p>

</div>

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

<div align="center">
  
## À propos & Objectifs.

</div>

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

# Chiffrement de bout en bout (TLS) avec Nginx sur Proxmox (Debian 13)

But : créer un guide autonome pour mettre en place un chiffrement TLS de bout en bout pour deux sites hébergés sur deux VM backend (`site-01`, `site-02`) derrière un reverse-proxy `proxy`. Toutes les VMs sous Debian 13.

Résumé réseau / DNS

- VMs : `proxy`, `site-01`, `site-02`.
- IP privées (ex.) : `10.10.10.11` (site-01), `10.10.10.12` (site-02).
- Domaines publics pointant vers le reverse-proxy : `site-01.example.com`, `site-02.example.com`.

Scope

- TLS client → proxy : certificats publics gérés par Certbot / Let's Encrypt sur `proxy`.
- TLS proxy → backend : deux options couvertes :
  - Option A : certificats Let's Encrypt sur chaque backend (si backends ont nom public ou DNS challenge possible).
  - Option B (recommandée pour backends internes) : CA interne + certificats signés pour les backends ; la CA est installée sur `proxy` pour vérification.

Prérequis

- Accès `sudo` sur `proxy`, `site-01`, `site-02`.
- DNS publics configurés pour `site-01.example.com` et `site-02.example.com` pointant vers l'IP publique du `proxy` (pour Option A). Pour Option B, pas besoin d'exposition publique des backends.
- Ports réseau : 80/443 publics sur `proxy`; 443 inter-VM ou accessible depuis `proxy` vers backends.

Vue d'ensemble des étapes

1. Installer les paquets nécessaires sur toutes les VMs.
2. Choisir Option A ou B pour les certificats backend.
3A. (Option A) Obtenir et installer Let's Encrypt sur chaque backend.
3B. (Option B) Créer une CA interne, signer les certificats backend et distribuer la CA au `proxy`.
4. Configurer Nginx HTTPS sur chaque backend.
5. Configurer Nginx sur `proxy` pour terminer TLS côté client et pour faire `proxy_pass https://<IP-backend>:443` en vérifiant le certificat backend.
6. Ouvrir ports / config pare-feu et tester.

1) Installation de base (toutes les VMs)

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y nginx ufw openssl curl
```

2) Choix de la méthode de certificats

- Option A — pratique si chaque backend possède un domaine public ou si un challenge DNS est possible.
- Option B — recommandée pour backends uniquement accessibles en interne : créez une CA interne et signez les certificats des backends.

3A) Option A — Let's Encrypt sur les backends

- Sur `site-01` (répétez pour `site-02`) :

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d site-01.example.com
```

Certbot configurera Nginx et installera les certificats sous `/etc/letsencrypt/live/site-01.example.com/`.

Remarque : pour valider, le domaine doit pointer vers l'IP publique de la VM ou utiliser un challenge DNS.

3B) Option B — CA interne et certificats pour les backends (recommandé si backends internes)

Sur une machine dédiée CA (peut être `proxy` ou une machine admin sécurisée) :

```bash
mkdir -p ~/myCA && cd ~/myCA
openssl genpkey -algorithm RSA -out ca.key -pkeyopt rsa_keygen_bits:4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt -subj "/CN=My Internal CA"
```

Pour chaque backend (ex. `site-01`) :

```bash
# Générer clé et CSR
openssl genpkey -algorithm RSA -out site01.key -pkeyopt rsa_keygen_bits:2048
openssl req -new -key site01.key -out site01.csr -subj "/CN=site-01.example.com"

# Fichier SAN (important)
cat > san.cnf <<EOF
subjectAltName = DNS:site-01.example.com,IP:10.10.10.11
EOF

# Signer la CSR avec la CA
openssl x509 -req -in site01.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out site01.crt -days 825 -sha256 -extfile san.cnf
```

Copiez `site01.crt` et `site01.key` vers `/etc/ssl/certs/` et `/etc/ssl/private/` sur `site-01` (permissions root). Copiez `ca.crt` vers `proxy` (voir section configuration proxy).

4) Configuration Nginx HTTPS sur les backends

Exemple de fichier `/etc/nginx/sites-available/site-01` (backend `site-01`):

```nginx
server {
    listen 80;
    server_name site-01.example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name site-01.example.com;

    ssl_certificate /etc/ssl/certs/site01.crt;
    ssl_certificate_key /etc/ssl/private/site01.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    location / {
        proxy_pass http://localhost:8080; # ou socket unix
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Activer le site et recharger Nginx :

```bash
sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01
sudo nginx -t
sudo systemctl reload nginx
```

5) Configuration du reverse-proxy `proxy`

Sur `proxy`, installer Certbot pour gérer les certificats clients → `proxy` :

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d site-01.example.com -d site-02.example.com
```

Si vous utilisez Option B (CA interne), copiez `ca.crt` sur `proxy` et placez-le en lieu sûr :

```bash
sudo mkdir -p /etc/ssl/private/internalCA
sudo mv ca.crt /etc/ssl/private/internalCA/ca.crt
sudo chmod 644 /etc/ssl/private/internalCA/ca.crt
```

Exemple de configuration Nginx sur `proxy` pour `site-01` (`/etc/nginx/sites-available/site-01`) :

```nginx
server {
    listen 80;
    server_name site-01.example.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name site-01.example.com;

    ssl_certificate /etc/letsencrypt/live/site-01.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/site-01.example.com/privkey.pem;

    location / {
        proxy_pass https://10.10.10.11:443;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        proxy_ssl_server_name on;               # envoie SNI
        proxy_ssl_protocols TLSv1.2 TLSv1.3;

        # Pour Option B (CA interne) : vérifier le certificat backend
        proxy_ssl_trusted_certificate /etc/ssl/private/internalCA/ca.crt;
        proxy_ssl_verify on;
        proxy_ssl_verify_depth 2;
    }
}
```

Notes importantes :

- `proxy_ssl_trusted_certificate` doit contenir la CA qui a signé le certificat backend (Option B).
- `proxy_ssl_server_name on;` permet d'envoyer SNI au backend si nécessaire.
- Assurez-vous que le CN/SAN du certificat backend correspond au `server_name` attendu (ou inclut l'IP si vous vérifiez par IP/SAN).

6) Pare-feu et ouverture de ports

Sur `proxy` (public) :

```bash
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

Sur les backends (`site-01`, `site-02`) (autoriser uniquement depuis le réseau interne / proxy) :

```bash
sudo ufw allow from 10.10.10.0/24 to any port 443 proto tcp
sudo ufw allow 80/tcp  # optionnel
sudo ufw enable
```

7) Tests

- Depuis l'extérieur (client) :

```bash
curl -I https://site-01.example.com
```

- Depuis `proxy`, tester la connexion TLS vers le backend (Option B : préciser `-CAfile`):

```bash
openssl s_client -connect 10.10.10.11:443 -servername site-01.example.com -CAfile /etc/ssl/private/internalCA/ca.crt
```

Recherchez `Verify return code: 0 (ok)` ou `verify return:1` selon la sortie (OpenSSL varie). Si la vérification est OK, la chaîne est valide.

8) Renouvellement

- Option A (Let's Encrypt) : Certbot installe un timer systemd. Tester :

```bash
sudo certbot renew --dry-run
```

- Option B (CA interne) : gérez vous‑même les durées ; renouvelez/sign ez les CSR et redéployez les fichiers sur les backends.

9) Bonnes pratiques

- Inclure les SAN corrects (DNS et IP interne si nécessaire) dans les certificats backend.
- Protéger la clé de la CA interne ; limiter l'accès à la machine CA.
- Surveiller l'expiration des certificats et automatiser la distribution si possible.
- Ne pas accepter des certificats non vérifiés côté `proxy` (éviter `proxy_ssl_verify off` en production).

Annexes — commandes utiles

- Copier config depuis poste local vers `proxy` (ex. PowerShell/OpenSSH) :

```bash
scp C:/Users/me/site-01.conf user@proxy:/tmp/site-01.conf
ssh user@proxy 'sudo mv /tmp/site-01.conf /etc/nginx/sites-available/site-01 && sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01 && sudo nginx -t && sudo systemctl reload nginx'
```

- Vérifier l'état Nginx : `sudo systemctl status nginx`
- Visualiser logs : `sudo tail -f /var/log/nginx/error.log`

Dépannage rapide

- Erreur de vérification TLS côté `proxy` : vérifier que le certificat backend contient le SAN attendu et que `proxy` a bien la CA racine (Option B).
- Si Certbot échoue : vérifier que les enregistrements DNS sont corrects et que les ports 80/443 sont accessibles pour la validation.

---

<div align="center">

<table>
<tr>
<td align="center"><b>🖥️ Infrastructure &amp; Sécurité</b></td>
<td align="center"><b>💻 Développement &amp; Web</b></td>
<td align="center"><b>🤖 Intelligence Artificielle</b></td>
</tr>
<tr>
<td align="center">
  <a href="https://www.kernel.org/"><img src="https://skillicons.dev/icons?i=linux" width="48" title="Linux" /></a>
  <a href="https://www.debian.org"><img src="https://skillicons.dev/icons?i=debian" width="48" title="Debian" /></a>
  <a href="https://www.gnu.org/software/bash/"><img src="https://skillicons.dev/icons?i=bash" width="48" title="Bash" /></a>
  <br/>
  <a href="https://nginx.org"><img src="https://skillicons.dev/icons?i=nginx" width="48" title="Nginx" /></a>
  <a href="https://www.docker.com"><img src="https://skillicons.dev/icons?i=docker" width="48" title="Docker" /></a>
  <a href="https://git-scm.com"><img src="https://skillicons.dev/icons?i=git" width="48" title="Git" /></a>
</td>
<td align="center">
  <a href="https://www.python.org"><img src="https://skillicons.dev/icons?i=python" width="48" title="Python" /></a>
  <a href="https://flask.palletsprojects.com"><img src="https://skillicons.dev/icons?i=flask" width="48" title="Flask" /></a>
  <a href="https://developer.mozilla.org/docs/Web/HTML"><img src="https://skillicons.dev/icons?i=html" width="48" title="HTML5" /></a>
  <br/>
  <a href="https://developer.mozilla.org/docs/Web/CSS"><img src="https://skillicons.dev/icons?i=css" width="48" title="CSS3" /></a>
  <a href="https://developer.mozilla.org/docs/Web/JavaScript"><img src="https://skillicons.dev/icons?i=js" width="48" title="JavaScript" /></a>
  <a href="https://code.visualstudio.com"><img src="https://skillicons.dev/icons?i=vscode" width="48" title="VS Code" /></a>
</td>
<td align="center">
  <a href="https://pytorch.org"><img src="https://skillicons.dev/icons?i=pytorch" width="48" title="PyTorch" /></a>
  <a href="https://www.tensorflow.org"><img src="https://skillicons.dev/icons?i=tensorflow" width="48" title="TensorFlow" /></a>
  <a href="https://www.raspberrypi.com"><img src="https://skillicons.dev/icons?i=raspberrypi" width="48" title="Raspberry Pi" /></a>
  <br/><br/>
  <a href="https://ollama.com"><img src="https://img.shields.io/badge/Ollama-000000?style=for-the-badge&logo=ollama&logoColor=white" alt="Ollama" /></a>
  &nbsp;
  <a href="https://anthropic.com"><img src="https://img.shields.io/badge/Anthropic-D97757?style=for-the-badge&logo=anthropic&logoColor=white" alt="Anthropic" /></a>
</td>
</tr>
</table>

<br/>

<b>🔒 Un projet proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Développé en collaboration avec <a href="https://claude.ai">Claude AI</a> (Anthropic) 🔒</b>

</div>


