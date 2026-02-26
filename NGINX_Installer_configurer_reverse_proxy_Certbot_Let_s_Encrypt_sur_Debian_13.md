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

# Reverse-proxy Nginx + Certbot (Debian 13)

Ce tutoriel explique comment installer et configurer un reverse-proxy `nginx` sur une VM Debian 13 (dans Proxmox) et obtenir des certificats SSL avec `certbot` pour deux backends : `site-01` et `site-02` (Debian 13).

> Remplacez `site-01.example.com`, `site-02.example.com`, et les adresses IP par vos valeurs réelles.

## Résumé

- Objectif : reverse-proxy sécurisé (TLS) sur une VM `proxy` qui route vers `site-01` et `site-02`.
- Hypothèse : les enregistrements DNS A pour `site-01.example.com` et `site-02.example.com` pointent vers l'IP publique de la VM `proxy`.

## Clarification : terminaison TLS (important)

- Le trafic public HTTPS est chiffré entre le client et la VM `proxy` (Certbot/Let's Encrypt configure TLS).
- Par défaut, la connexion entre la VM `proxy` et les backends `site-01`/`site-02` reste en HTTP (non chiffré) sur le réseau interne.

## Pourquoi cette approche ?

- Simplicité : gestion centralisée des certificats et renouvellement sur une seule machine (`proxy`).
- Acceptable si le trafic proxy→backend circule dans un réseau de confiance (réseau interne Proxmox, VLAN privé).

## Note importante — chiffrement de bout en bout

Ce tutoriel se concentre uniquement sur la terminaison TLS au niveau du reverse‑proxy `proxy` (Certbot/Let's Encrypt). Le chiffrement de bout en bout (TLS entre `proxy` et les backends) n'est pas abordé ici et fera l'objet d'un tutoriel séparé si nécessaire.

---

## Prérequis

- Proxmox opérationnel et VMs créées : `proxy`, `site-01`, `site-02` (toutes Debian 13).
- Accès `root` ou `sudo` sur chaque VM.
- IP privées des backends (ex. `10.10.10.11` pour `site-01`, `10.10.10.12` pour `site-02`).
- DNS configuré pour pointer les domaines vers l'IP publique de la VM `proxy`.

## Réseau Proxmox (rapide)

- Utilisez un bridge (ex. `vmbr0`) pour exposer la VM `proxy` sur le réseau public.
- Les backends peuvent être sur le même bridge ou sur un réseau interne ; assurez-vous que la VM `proxy` peut joindre leurs IP privées.

---

## Étapes sur la VM `proxy` (Debian 13)

### 1) Mise à jour et installation des paquets

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install -y nginx ufw curl software-properties-common
sudo apt install -y certbot python3-certbot-nginx
```

### 2) Configuration basique `nginx` (fichiers séparés)

Placez un fichier par site dans `/etc/nginx/sites-available/` puis activez-les via un lien symbolique vers `/etc/nginx/sites-enabled/`.

Exemple `/etc/nginx/sites-available/site-01` :

```nginx
server {
    listen 80;
    server_name site-01.example.com;

    location / {
        proxy_pass http://10.10.10.11:80; # backend site-01
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 90;
    }
}
```

Exemple `/etc/nginx/sites-available/site-02` :

```nginx
server {
    listen 80;
    server_name site-02.example.com;

    location / {
        proxy_pass http://10.10.10.12:80; # backend site-02
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_read_timeout 90;
    }
}
```

Commandes pour copier/activer/tester (exécuter depuis votre poste ou depuis la VM `proxy`) :

Depuis votre poste (ex. PowerShell avec scp) :

```powershell
scp C:/Users/mmalet/Documents/Work/NGIX/site-01.conf user@proxy:/tmp/site-01.conf
scp C:/Users/mmalet/Documents/Work/NGIX/site-02.conf user@proxy:/tmp/site-02.conf
```

Sur la VM `proxy` :

```bash
sudo mv /tmp/site-01.conf /etc/nginx/sites-available/site-01
sudo mv /tmp/site-02.conf /etc/nginx/sites-available/site-02
sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01
sudo ln -s /etc/nginx/sites-available/site-02 /etc/nginx/sites-enabled/site-02
sudo nginx -t
sudo systemctl reload nginx
```

> Après activation et rechargement, les sites sont prêts mais aucun certificat n'a encore été émis. Lancez l'étape 4 pour que Certbot contacte Let's Encrypt et crée les certificats.

### 3) Ouvrir les ports (UFW)

```bash
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

### 4) Obtenir les certificats avec Certbot (plugin `nginx`)

Après avoir activé les sites et rechargé `nginx`, et ouvert les ports, vérifiez les enregistrements DNS A pointant vers l'IP publique du `proxy`.

Lancer Certbot une fois par domaine :

```bash
sudo certbot --nginx -d site-01.example.com
sudo certbot --nginx -d site-02.example.com
```

Options :

- Option A (recommandée) — commandes séparées :

```bash
sudo certbot --nginx -d site-01.example.com
sudo certbot --nginx -d site-02.example.com
```

- Option B — certificat SAN couvrant les deux domaines :

```bash
sudo certbot --nginx -d site-01.example.com -d site-02.example.com
```

Remarques :

- `--nginx` détecte les blocs `server` et mettra à jour la config pour HTTPS et (optionnellement) rediriger HTTP → HTTPS.
- Si vous préférez obtenir un certificat sans modification automatique, utilisez `certonly` ou `--manual`.

### 5) Vérifier renouvellement

```bash
sudo systemctl status certbot.timer
sudo certbot renew --dry-run
```

## Recommandations TLS & sécurité

- Conserver les directives `proxy_set_header` dans chaque bloc `location`.
- Renforcer TLS si souhaité (Ciphers, versions TLS, HSTS). Certbot applique une configuration sûre par défaut.

## Schéma simplifié (flux)

```
                          Client (navigateur)
                                  |
                                  v
             DNS: site-01.example.com / site-02.example.com
                                  |
                                  v
                              Internet
                                  |
                                  v
                     +--------------------------------+
                     | VM `proxy` (Debian)            |
                     | Nginx reverse-proxy + Certbot  |
                     | écoute : 80, 443               |
                     | Public IP : <IP_publique>      |
                     +---------------+----------------+
                                     |
                    +----------------+----------------+
                    |                                 |
                    v                                 v
            site-01 backend                   site-02 backend
            10.10.10.11:80                    10.10.10.12:80
            (HTTP interne)                    (HTTP interne)

          flux : Client -> HTTPS -> proxy -> HTTP -> backend
```

## Côté backends (`site-01`, `site-02`)

- Les backends servent en HTTP interne ; TLS géré par le proxy.
- Restreindre l'accès direct aux ports 80/443 des backends depuis Internet. Autoriser uniquement la VM `proxy` si possible.

## Tests rapides

Depuis une machine externe :

```bash
curl -I https://site-01.example.com
curl -I https://site-02.example.com
```

Depuis la VM `proxy` :

```bash
curl -I http://10.10.10.11
curl -I http://10.10.10.12
```

## Maintenance & dépannage

- Logs nginx : `/var/log/nginx/error.log` et `/var/log/nginx/access.log`.
- Tester config nginx : `sudo nginx -t` puis `sudo systemctl reload nginx`.
- Renouvellement manuel : `sudo certbot renew`.

## Bonnes pratiques

- Garder le système et paquets à jour.
- Restreindre SSH (authentification par clé, désactiver root si possible).
- Ne pas exposer directement les backends au public.
- Surveiller l'expiration des certificats (Certbot automatise le renouvellement).

---

Fichier généré par l'utilisateur (conversion en Markdown).

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

