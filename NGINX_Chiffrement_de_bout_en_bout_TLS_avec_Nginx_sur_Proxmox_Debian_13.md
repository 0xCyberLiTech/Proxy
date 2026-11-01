<!-- MASTER-HEADER-START -->
<div align="center">

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/headers/header-proxy.svg" alt="ATELIER PROXY // FILTRAGE APPLICATIF" width="100%" />
</a>

<p align="center">
  <a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-portal.svg" alt="Portail 0xCyberLiTech.com" width="24%" /></a>
  <a href="https://0xcyberlitech.com/soc-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-soc-rd.svg" alt="Livre Blanc SOC Cockpit" width="24%" /></a>
  <a href="https://0xcyberlitech.com/jarvis-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-jarvis-rd.svg" alt="Livre Blanc JARVIS IA" width="24%" /></a>
  <a href="https://github.com/0xCyberLiTech?tab=repositories"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-github.svg" alt="Catalogue des 23 Dépôts" width="24%" /></a>
</p>
<p align="center">
  <a href="README.md"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-back-readme.svg" alt="Retour au sommaire du dépôt" width="340" /></a>
</p>

<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />
</div>
<!-- MASTER-HEADER-END -->

<!-- DEBIAN-TRUTH-START -->
> [!IMPORTANT]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="18" height="18" align="absmiddle" alt="" /> ** Matrice Officielle Debian — `Debian 12 (Bookworm)` · `Debian 13 (Trixie)` · `Debian 14 (Forky)`**
> - **Sources Officielles Debian.org (`squid`) :** `Bookworm (12)` : **`5.7-2+deb12u5`** &nbsp;·&nbsp; `Trixie (13 — Stable)` : **`6.13-2+deb13u3`** &nbsp;·&nbsp; `Forky (14 — Next-Gen)` : **`7.7-1`**
> - **Références Officielles :** [Paquet Debian Trixie (`squid`)](https://packages.debian.org/trixie/squid) &nbsp;·&nbsp; [Paquet Debian Forky (`squid`)](https://packages.debian.org/forky/squid) &nbsp;·&nbsp; [Documentation Officielle Debian](https://packages.debian.org/trixie/squid)
> - **Note d'Ingénierie (Divergence 13 vs 14) :** Sous Debian 13 (Trixie) et Debian 14 (Forky), Squid 6+ privilégie l'inclusion modulaire dans **`/etc/squid/conf.d/`** plutôt que la modification directe du fichier maître `/etc/squid/squid.conf`.

<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-doc.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Afficher les Encarts Ligne de Commande : Debian 13 (Trixie — Stable) vs Debian 14 (Forky — Next-Gen)</b></summary>

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-ok.svg" width="42" height="16" align="absmiddle" alt="[OK]" /> Profil Production Stable — Commandes pour `Debian 13 (Trixie)`
```bash
# Debian 13 (Trixie — Squid 6.x) : Verification de la configuration & rechargement sans coupure
sudo squid -k parse && sudo systemctl reload squid
sudo tail -f /var/log/squid/access.log
```

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-rd.svg" width="46" height="16" align="absmiddle" alt="[R&D]" /> Profil Anticipation Next-Gen — Commandes pour `Debian 14 (Forky)`
```bash
# Debian 14 (Forky — Squid 6.x/7.x) : Configuration modulaire dans /etc/squid/conf.d/*.conf
sudo journalctl -u squid.service -n 50 --no-hostname
```

</details>
<!-- DEBIAN-TRUTH-END -->

<!-- CIEL-PEDAGOGY-START -->
<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-puzzle.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Synthèse d'Architecture, Standards Officiels &amp; Diagnostic (Niveau 1 : Fondamentaux · Niveau 2 : Repères · Niveau 3 : Diagnostic)</b></summary>

- **Niveau 1 — Fondamentaux &amp; Principe d'Architecture :** Un **Serveur Proxy** agit comme passerelle intermédiaire de sécurité et de performance : en **Proxy Explicite / Sortant (Squid)**, il contrôle, journalise et met en cache les accès Web des postes internes ; en **Reverse Proxy (Nginx / HAProxy)**, il protège les applications internes derrière un point d'entrée unique (TLS, WAF, mTLS).
- **Niveau 2 — Repères Officiels (Ports IANA, Fichiers Debian &amp; Commandes de Validation) :** **Ports standards :** `TCP 3128` (Squid Forward Proxy) · `TCP 80 / 443` (Reverse Proxy HTTP/HTTPS) &nbsp;|&nbsp; **Fichiers Debian :** `/etc/squid/squid.conf`, `/etc/nginx/nginx.conf` &nbsp;|&nbsp; **Validation officielle :** `sudo squid -k parse` / `sudo nginx -t`.
- **Niveau 3 — Diagnostic d'Incidents &amp; Ingénierie Avancée :** **Diagnostic & Architecture :** Dans Squid, les directives `http_access` sont évaluées séquentiellement selon le principe du **premier match gagnant** : placez impérativement vos règles d'autorisation (`http_access allow <acl_lan>`) au-dessus de la règle de fermeture `http_access deny all`.
- **Sources de Vérité Officielles Éditeurs &amp; Standards :** [Documentation Officielle Squid Cache](https://www.squid-cache.org/Doc/config/) &nbsp;·&nbsp; [Documentation Officielle Nginx](https://nginx.org/en/docs/)
- **Livres Blancs &amp; 67 Podcasts Audio R&amp;D :** [Portail 0xCyberLiTech](https://0xcyberlitech.com/) &nbsp;·&nbsp; [Livre Blanc SOC Cockpit v5.5](https://0xcyberlitech.com/soc-rd.html) &nbsp;·&nbsp; [Livre Blanc JARVIS Mark-VII](https://0xcyberlitech.com/jarvis-rd.html) &nbsp;·&nbsp; [Blueprint Logiciel &amp; Matériel RTX 5080](https://0xcyberlitech.com/blueprint-jarvis.html)

</details>
<!-- CIEL-PEDAGOGY-END -->


# <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="20" height="20" align="absmiddle" alt="" /> Chiffrement de bout en bout (TLS) avec Nginx sur Proxmox (Debian 13)

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

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt update
> sudo apt upgrade -y
> sudo apt install -y nginx ufw openssl curl
> ```
<!-- CODE-WINDOW-END -->

2) Choix de la méthode de certificats

- Option A — pratique si chaque backend possède un domaine public ou si un challenge DNS est possible.
- Option B — recommandée pour backends uniquement accessibles en interne : créez une CA interne et signez les certificats des backends.

3A) Option A — Let's Encrypt sur les backends

- Sur `site-01` (répétez pour `site-02`) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt install -y certbot python3-certbot-nginx
> sudo certbot --nginx -d site-01.example.com
> ```
<!-- CODE-WINDOW-END -->

Certbot configurera Nginx et installera les certificats sous `/etc/letsencrypt/live/site-01.example.com/`.

Remarque : pour valider, le domaine doit pointer vers l'IP publique de la VM ou utiliser un challenge DNS.

3B) Option B — CA interne et certificats pour les backends (recommandé si backends internes)

Sur une machine dédiée CA (peut être `proxy` ou une machine admin sécurisée) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> mkdir -p ~/myCA && cd ~/myCA
> openssl genpkey -algorithm RSA -out ca.key -pkeyopt rsa_keygen_bits:4096
> openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt -subj "/CN=My Internal CA"
> ```
<!-- CODE-WINDOW-END -->

Pour chaque backend (ex. `site-01`) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> # Générer clé et CSR
> openssl genpkey -algorithm RSA -out site01.key -pkeyopt rsa_keygen_bits:2048
> openssl req -new -key site01.key -out site01.csr -subj "/CN=site-01.example.com"
>
> # Fichier SAN (important)
> cat > san.cnf <<EOF
> subjectAltName = DNS:site-01.example.com,IP:10.10.10.11
> EOF
>
> # Signer la CSR avec la CA
> openssl x509 -req -in site01.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out site01.crt -days 825 -sha256 -extfile san.cnf
> ```
<!-- CODE-WINDOW-END -->

Copiez `site01.crt` et `site01.key` vers `/etc/ssl/certs/` et `/etc/ssl/private/` sur `site-01` (permissions root). Copiez `ca.crt` vers `proxy` (voir section configuration proxy).

4) Configuration Nginx HTTPS sur les backends

Exemple de fichier `/etc/nginx/sites-available/site-01` (backend `site-01`):

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> server {
>     listen 80;
>     server_name site-01.example.com;
>     return 301 https://$host$request_uri;
> }
>
> server {
>     listen 443 ssl;
>     server_name site-01.example.com;
>
>     ssl_certificate /etc/ssl/certs/site01.crt;
>     ssl_certificate_key /etc/ssl/private/site01.key;
>     ssl_protocols TLSv1.2 TLSv1.3;
>     ssl_prefer_server_ciphers on;
>
>     location / {
>         proxy_pass http://localhost:8080; # ou socket unix
>         proxy_set_header Host $host;
>         proxy_set_header X-Real-IP $remote_addr;
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>         proxy_set_header X-Forwarded-Proto $scheme;
>     }
> }
> ```
<!-- CODE-WINDOW-END -->

Activer le site et recharger Nginx :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01
> sudo nginx -t
> sudo systemctl reload nginx
> ```
<!-- CODE-WINDOW-END -->

5) Configuration du reverse-proxy `proxy`

Sur `proxy`, installer Certbot pour gérer les certificats clients → `proxy` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt install -y certbot python3-certbot-nginx
> sudo certbot --nginx -d site-01.example.com -d site-02.example.com
> ```
<!-- CODE-WINDOW-END -->

Si vous utilisez Option B (CA interne), copiez `ca.crt` sur `proxy` et placez-le en lieu sûr :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo mkdir -p /etc/ssl/private/internalCA
> sudo mv ca.crt /etc/ssl/private/internalCA/ca.crt
> sudo chmod 644 /etc/ssl/private/internalCA/ca.crt
> ```
<!-- CODE-WINDOW-END -->

Exemple de configuration Nginx sur `proxy` pour `site-01` (`/etc/nginx/sites-available/site-01`) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> server {
>     listen 80;
>     server_name site-01.example.com;
>     return 301 https://$host$request_uri;
> }
>
> server {
>     listen 443 ssl;
>     server_name site-01.example.com;
>
>     ssl_certificate /etc/letsencrypt/live/site-01.example.com/fullchain.pem;
>     ssl_certificate_key /etc/letsencrypt/live/site-01.example.com/privkey.pem;
>
>     location / {
>         proxy_pass https://10.10.10.11:443;
>         proxy_http_version 1.1;
>         proxy_set_header Host $host;
>         proxy_set_header X-Real-IP $remote_addr;
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>         proxy_set_header X-Forwarded-Proto $scheme;
>
>         proxy_ssl_server_name on;               # envoie SNI
>         proxy_ssl_protocols TLSv1.2 TLSv1.3;
>
>         # Pour Option B (CA interne) : vérifier le certificat backend
>         proxy_ssl_trusted_certificate /etc/ssl/private/internalCA/ca.crt;
>         proxy_ssl_verify on;
>         proxy_ssl_verify_depth 2;
>     }
> }
> ```
<!-- CODE-WINDOW-END -->

Notes importantes :

- `proxy_ssl_trusted_certificate` doit contenir la CA qui a signé le certificat backend (Option B).
- `proxy_ssl_server_name on;` permet d'envoyer SNI au backend si nécessaire.
- Assurez-vous que le CN/SAN du certificat backend correspond au `server_name` attendu (ou inclut l'IP si vous vérifiez par IP/SAN).

6) Pare-feu et ouverture de ports

Sur `proxy` (public) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo ufw allow OpenSSH
> sudo ufw allow 80/tcp
> sudo ufw allow 443/tcp
> sudo ufw enable
> ```
<!-- CODE-WINDOW-END -->

Sur les backends (`site-01`, `site-02`) (autoriser uniquement depuis le réseau interne / proxy) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo ufw allow from 10.10.10.0/24 to any port 443 proto tcp
> sudo ufw allow 80/tcp  # optionnel
> sudo ufw enable
> ```
<!-- CODE-WINDOW-END -->

7) Tests

- Depuis l'extérieur (client) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -I https://site-01.example.com
> ```
<!-- CODE-WINDOW-END -->

- Depuis `proxy`, tester la connexion TLS vers le backend (Option B : préciser `-CAfile`):

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> openssl s_client -connect 10.10.10.11:443 -servername site-01.example.com -CAfile /etc/ssl/private/internalCA/ca.crt
> ```
<!-- CODE-WINDOW-END -->

Recherchez `Verify return code: 0 (ok)` ou `verify return:1` selon la sortie (OpenSSL varie). Si la vérification est OK, la chaîne est valide.

8) Renouvellement

- Option A (Let's Encrypt) : Certbot installe un timer systemd. Tester :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo certbot renew --dry-run
> ```
<!-- CODE-WINDOW-END -->

- Option B (CA interne) : gérez vous‑même les durées ; renouvelez/sign ez les CSR et redéployez les fichiers sur les backends.

9) Bonnes pratiques

- Inclure les SAN corrects (DNS et IP interne si nécessaire) dans les certificats backend.
- Protéger la clé de la CA interne ; limiter l'accès à la machine CA.
- Surveiller l'expiration des certificats et automatiser la distribution si possible.
- Ne pas accepter des certificats non vérifiés côté `proxy` (éviter `proxy_ssl_verify off` en production).

Annexes — commandes utiles

- Copier config depuis poste local vers `proxy` (ex. PowerShell/OpenSSH) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> scp C:/Users/me/site-01.conf user@proxy:/tmp/site-01.conf
> ssh user@proxy 'sudo mv /tmp/site-01.conf /etc/nginx/sites-available/site-01 && sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01 && sudo nginx -t && sudo systemctl reload nginx'
> ```
<!-- CODE-WINDOW-END -->

- Vérifier l'état Nginx : `sudo systemctl status nginx`
- Visualiser logs : `sudo tail -f /var/log/nginx/error.log`

Dépannage rapide

- Erreur de vérification TLS côté `proxy` : vérifier que le certificat backend contient le SAN attendu et que `proxy` a bien la CA racine (Option B).
- Si Certbot échoue : vérifier que les enregistrements DNS sont corrects et que les ports 80/443 sont accessibles pour la validation.

<!-- MASTER-FOOTER-START -->
<div align="center">
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/footer-master.svg" alt="0xCyberLiTech — Architecture Cyberdéfense, Ingénierie Déterministe & IA Souveraine" width="100%" />
</a>

<p align="center"><a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/badge-antigravity.svg" alt="Architecturé par Marc & Antigravity (Google DeepMind)" width="520" /></a></p>

<sub><b>Architecturé &amp; Forgé par Marc &amp; Antigravity (Google DeepMind)</b> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/"><b>0xcyberlitech.com</b></a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/soc-rd.html">Architecture SOC Cockpit v5.5</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/jarvis-rd.html">Architecture JARVIS Mark-VII</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/blueprint-jarvis.html">Blueprint Matériel RTX 5080</a></sub>

</div>
<!-- MASTER-FOOTER-END -->
