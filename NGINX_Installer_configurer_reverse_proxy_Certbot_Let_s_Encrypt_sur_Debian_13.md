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

</details>
<!-- CIEL-PEDAGOGY-END -->


# <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="20" height="20" align="absmiddle" alt="" /> Reverse-proxy Nginx + Certbot (Debian 13)

Ce tutoriel explique comment installer et configurer un reverse-proxy `nginx` sur une VM Debian 13 (dans Proxmox) et obtenir des certificats SSL avec `certbot` pour deux backends : `site-01` et `site-02` (Debian 13).

> Remplacez `site-01.example.com`, `site-02.example.com`, et les adresses IP par vos valeurs réelles.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 01 // Résumé
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Objectif : reverse-proxy sécurisé (TLS) sur une VM `proxy` qui route vers `site-01` et `site-02`.
- Hypothèse : les enregistrements DNS A pour `site-01.example.com` et `site-02.example.com` pointent vers l'IP publique de la VM `proxy`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 02 // Clarification : terminaison TLS (important)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Le trafic public HTTPS est chiffré entre le client et la VM `proxy` (Certbot/Let's Encrypt configure TLS).
- Par défaut, la connexion entre la VM `proxy` et les backends `site-01`/`site-02` reste en HTTP (non chiffré) sur le réseau interne.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 03 // Pourquoi cette approche ?
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Simplicité : gestion centralisée des certificats et renouvellement sur une seule machine (`proxy`).
- Acceptable si le trafic proxy→backend circule dans un réseau de confiance (réseau interne Proxmox, VLAN privé).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 04 // Note importante — chiffrement de bout en bout
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Ce tutoriel se concentre uniquement sur la terminaison TLS au niveau du reverse‑proxy `proxy` (Certbot/Let's Encrypt). Le chiffrement de bout en bout (TLS entre `proxy` et les backends) n'est pas abordé ici et fera l'objet d'un tutoriel séparé si nécessaire.

---

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 05 // Prérequis
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Proxmox opérationnel et VMs créées : `proxy`, `site-01`, `site-02` (toutes Debian 13).
- Accès `root` ou `sudo` sur chaque VM.
- IP privées des backends (ex. `10.10.10.11` pour `site-01`, `10.10.10.12` pour `site-02`).
- DNS configuré pour pointer les domaines vers l'IP publique de la VM `proxy`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 06 // Réseau Proxmox (rapide)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Utilisez un bridge (ex. `vmbr0`) pour exposer la VM `proxy` sur le réseau public.
- Les backends peuvent être sur le même bridge ou sur un réseau interne ; assurez-vous que la VM `proxy` peut joindre leurs IP privées.

---

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 07 // Étapes sur la VM `proxy` (Debian 13)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 1) Mise à jour et installation des paquets

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt update
> sudo apt upgrade -y
> sudo apt install -y nginx ufw curl software-properties-common
> sudo apt install -y certbot python3-certbot-nginx
> ```
<!-- CODE-WINDOW-END -->

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 2) Configuration basique `nginx` (fichiers séparés)

Placez un fichier par site dans `/etc/nginx/sites-available/` puis activez-les via un lien symbolique vers `/etc/nginx/sites-enabled/`.

Exemple `/etc/nginx/sites-available/site-01` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> server {
>     listen 80;
>     server_name site-01.example.com;
>
>     location / {
>         proxy_pass http://10.10.10.11:80; # backend site-01
>         proxy_http_version 1.1;
>         proxy_set_header Host $host;
>         proxy_set_header X-Real-IP $remote_addr;
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>         proxy_set_header X-Forwarded-Proto $scheme;
>         proxy_set_header Upgrade $http_upgrade;
>         proxy_set_header Connection "upgrade";
>         proxy_read_timeout 90;
>     }
> }
> ```
<!-- CODE-WINDOW-END -->

Exemple `/etc/nginx/sites-available/site-02` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> server {
>     listen 80;
>     server_name site-02.example.com;
>
>     location / {
>         proxy_pass http://10.10.10.12:80; # backend site-02
>         proxy_http_version 1.1;
>         proxy_set_header Host $host;
>         proxy_set_header X-Real-IP $remote_addr;
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
>         proxy_set_header X-Forwarded-Proto $scheme;
>         proxy_set_header Upgrade $http_upgrade;
>         proxy_set_header Connection "upgrade";
>         proxy_read_timeout 90;
>     }
> }
> ```
<!-- CODE-WINDOW-END -->

Commandes pour copier/activer/tester (exécuter depuis votre poste ou depuis la VM `proxy`) :

Depuis votre poste (ex. PowerShell avec scp) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```powershell
> scp C:/Users/me/site-01.conf user@proxy:/tmp/site-01.conf
> scp C:/Users/me/site-02.conf user@proxy:/tmp/site-02.conf
> ```
<!-- CODE-WINDOW-END -->

Sur la VM `proxy` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo mv /tmp/site-01.conf /etc/nginx/sites-available/site-01
> sudo mv /tmp/site-02.conf /etc/nginx/sites-available/site-02
> sudo ln -s /etc/nginx/sites-available/site-01 /etc/nginx/sites-enabled/site-01
> sudo ln -s /etc/nginx/sites-available/site-02 /etc/nginx/sites-enabled/site-02
> sudo nginx -t
> sudo systemctl reload nginx
> ```
<!-- CODE-WINDOW-END -->

> Après activation et rechargement, les sites sont prêts mais aucun certificat n'a encore été émis. Lancez l'étape 4 pour que Certbot contacte Let's Encrypt et crée les certificats.

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 3) Ouvrir les ports (UFW)

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

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 4) Obtenir les certificats avec Certbot (plugin `nginx`)

Après avoir activé les sites et rechargé `nginx`, et ouvert les ports, vérifiez les enregistrements DNS A pointant vers l'IP publique du `proxy`.

Lancer Certbot une fois par domaine :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo certbot --nginx -d site-01.example.com
> sudo certbot --nginx -d site-02.example.com
> ```
<!-- CODE-WINDOW-END -->

Options :

- Option A (recommandée) — commandes séparées :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo certbot --nginx -d site-01.example.com
> sudo certbot --nginx -d site-02.example.com
> ```
<!-- CODE-WINDOW-END -->

- Option B — certificat SAN couvrant les deux domaines :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo certbot --nginx -d site-01.example.com -d site-02.example.com
> ```
<!-- CODE-WINDOW-END -->

Remarques :

- `--nginx` détecte les blocs `server` et mettra à jour la config pour HTTPS et (optionnellement) rediriger HTTP → HTTPS.
- Si vous préférez obtenir un certificat sans modification automatique, utilisez `certonly` ou `--manual`.

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 5) Vérifier renouvellement

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo systemctl status certbot.timer
> sudo certbot renew --dry-run
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 08 // Recommandations TLS & sécurité
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Conserver les directives `proxy_set_header` dans chaque bloc `location`.
- Renforcer TLS si souhaité (Ciphers, versions TLS, HSTS). Certbot applique une configuration sûre par défaut.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 09 // Schéma simplifié (flux)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
>                           Client (navigateur)
>                                   |
>                                   v
>              DNS: site-01.example.com / site-02.example.com
>                                   |
>                                   v
>                               Internet
>                                   |
>                                   v
>                      +--------------------------------+
>                      | VM `proxy` (Debian)            |
>                      | Nginx reverse-proxy + Certbot  |
>                      | écoute : 80, 443               |
>                      | Public IP : <IP_publique>      |
>                      +---------------+----------------+
>                                      |
>                     +----------------+----------------+
>                     |                                 |
>                     v                                 v
>             site-01 backend                   site-02 backend
>             10.10.10.11:80                    10.10.10.12:80
>             (HTTP interne)                    (HTTP interne)
>
>           flux : Client -> HTTPS -> proxy -> HTTP -> backend
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 10 // Côté backends (`site-01`, `site-02`)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Les backends servent en HTTP interne ; TLS géré par le proxy.
- Restreindre l'accès direct aux ports 80/443 des backends depuis Internet. Autoriser uniquement la VM `proxy` si possible.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 11 // Tests rapides
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Depuis une machine externe :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -I https://site-01.example.com
> curl -I https://site-02.example.com
> ```
<!-- CODE-WINDOW-END -->

Depuis la VM `proxy` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -I http://10.10.10.11
> curl -I http://10.10.10.12
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 12 // Maintenance & dépannage
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Logs nginx : `/var/log/nginx/error.log` et `/var/log/nginx/access.log`.
- Tester config nginx : `sudo nginx -t` puis `sudo systemctl reload nginx`.
- Renouvellement manuel : `sudo certbot renew`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 13 // Bonnes pratiques
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- Garder le système et paquets à jour.
- Restreindre SSH (authentification par clé, désactiver root si possible).
- Ne pas exposer directement les backends au public.
- Surveiller l'expiration des certificats (Certbot automatise le renouvellement).

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
