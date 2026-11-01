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


Présentation
============

Ce document formalise la procédure de sécurisation de la configuration Nginx utilisée pour `site-exemple-secure.conf`.

Le guide contient les extraits de configuration, les explications et une checklist de déploiement prête à l'emploi.

Remarques préalables
--------------------

- Les chemins et fichiers indiqués s'appliquent à une installation Debian/Ubuntu classique.
- Les commandes de déploiement doivent être exécutées avec un compte disposant des droits `sudo`.

Contenu des fichiers et snippets
-------------------------------

1 — /etc/nginx/snippets/security-headers.conf

Contenu (coller tel quel dans `/etc/nginx/snippets/security-headers.conf`) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
> add_header X-Frame-Options            "DENY"                          always;
> add_header X-Content-Type-Options     "nosniff"                       always;
> add_header X-XSS-Protection           "1; mode=block"                 always;
> add_header Referrer-Policy            "strict-origin-when-cross-origin" always;
> add_header Permissions-Policy         "geolocation=(), microphone=(), camera=(), payment=(), usb=(), bluetooth=()" always;
> add_header Content-Security-Policy    "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; connect-src 'self'; frame-ancestors 'none'; object-src 'none'; base-uri 'self';" always;
> ```
<!-- CODE-WINDOW-END -->

Pourquoi un fichier séparé ?

- Nginx réinitialise l'héritage des `add_header` au niveau d'un `location` qui définit ses propres `add_header`. En pratique, inclure ce snippet dans chaque `location` qui définit des headers garantit l'application cohérente des en-têtes de sécurité.

2 — Ajouts dans le bloc `http {}` de `/etc/nginx/nginx.conf`

Extraits recommandés (à ajouter dans `http {}`) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> # Rate limiting
> limit_req_zone $binary_remote_addr zone=api_nvd:10m rate=10r/m;
> limit_req_zone $binary_remote_addr zone=api_vt:10m  rate=30r/m;
> limit_req_zone $binary_remote_addr zone=global:10m  rate=60r/m;
>
> # Cache proxy
> proxy_cache_path /var/cache/nginx/api levels=1:2 keys_zone=api_cache:10m max_size=100m inactive=2h use_temp_path=off;
>
> # Timeouts (protection Slowloris)
> client_header_timeout  10s;
> client_body_timeout    15s;
> send_timeout           30s;
> keepalive_timeout      65s;
> ```
<!-- CODE-WINDOW-END -->

3 — Exemple de serveur : `/etc/nginx/sites-available/exemple.com`

Le fichier de site fourni implémente :
- masquage des headers du backend (`proxy_hide_header`),
- réapplication des headers de sécurité dans chaque `location` (inclusion du snippet),
- protections rate-limiting par API (`/api/nvd/`, `/api/vt/`) avec `limit_req`,
- cache proxy pour réduire la charge sur les APIs externes (`proxy_cache`),
- gestion des préflight CORS pour chaque API,
- protection des chemins internes (`/docs` retourne `404` pour masquer l'existence).

Extraits importants (adapter les adresses, certificats et clés) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> location /api/nvd/ {
>     limit_req zone=api_nvd burst=5 nodelay;
>     limit_req_status 429;
>     limit_except GET OPTIONS { deny all; }
>
>     proxy_pass            https://services.nvd.nist.gov/rest/json/cves/2.0/;
>     proxy_ssl_server_name on;
>     proxy_ssl_name        services.nvd.nist.gov;
>     proxy_ssl_protocols   TLSv1.2 TLSv1.3;
>
>     proxy_set_header Host      services.nvd.nist.gov;
>     proxy_set_header apiKey    $nvd_api_key;
>     proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";
>
>     proxy_set_header Authorization "";
>     proxy_set_header Cookie        "";
>
>     proxy_cache           api_cache;
>     proxy_cache_valid     200 1h;
>     proxy_cache_valid     404 1m;
>     proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
>     proxy_cache_lock      on;
>     add_header            X-Cache-Status $upstream_cache_status;
>
>     include /etc/nginx/snippets/security-headers.conf;
>
>     add_header Access-Control-Allow-Origin  "https://exemple.com" always;
>     add_header Access-Control-Allow-Methods "GET, OPTIONS"              always;
>     add_header Access-Control-Allow-Headers "Content-Type"              always;
>
>     if ($request_method = OPTIONS) {
>         add_header Access-Control-Allow-Origin  "https://exemple.com";
>         add_header Access-Control-Allow-Methods "GET, OPTIONS";
>         add_header Access-Control-Allow-Headers "Content-Type";
>         add_header Access-Control-Max-Age       3600;
>         add_header Content-Type                 "text/plain; charset=utf-8";
>         return 204;
>     }
> }
> ```
<!-- CODE-WINDOW-END -->

4 — Exemple `nginx.conf` (extraits)

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> user www-data;
> worker_processes auto;
>
> http {
>     sendfile on;
>     tcp_nopush on;
>     server_tokens off;
>
>     ssl_protocols TLSv1.2 TLSv1.3;
>
>     access_log /var/log/nginx/access.log;
>     gzip on;
>
>     # Rate limiting (exemples définis ici si souhaité globalement)
>     limit_req_zone $binary_remote_addr zone=api_nvd:10m rate=10r/m;
>     limit_req_zone $binary_remote_addr zone=api_vt:10m  rate=30r/m;
>     limit_req_zone $binary_remote_addr zone=global:10m  rate=60r/m;
>
>     proxy_cache_path /var/cache/nginx/api levels=1:2 keys_zone=api_cache:10m max_size=100m inactive=2h use_temp_path=off;
>
>     client_header_timeout  10s;
>     client_body_timeout    15s;
>     send_timeout           30s;
>     keepalive_timeout      65s;
>
>     include /etc/nginx/conf.d/*.conf;
>     include /etc/nginx/sites-enabled/*;
> }
> ```
<!-- CODE-WINDOW-END -->

Checklist de déploiement
------------------------

1. Créer le dossier cache et définir la propriété :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo mkdir -p /var/cache/nginx/api
> sudo chown www-data:www-data /var/cache/nginx/api
> ```
<!-- CODE-WINDOW-END -->

2. Créer le snippet des headers de sécurité :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo mkdir -p /etc/nginx/snippets
> sudo nano /etc/nginx/snippets/security-headers.conf
> # (coller le contenu de la section 1 ci-dessus)
> ```
<!-- CODE-WINDOW-END -->

3. Ajouter les directives `http` dans `/etc/nginx/nginx.conf` (section 2).

4. Placer la configuration de site dans `/etc/nginx/sites-available/exemple.com` puis créer le lien :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo ln -s /etc/nginx/sites-available/exemple.com /etc/nginx/sites-enabled/exemple.com
> ```
<!-- CODE-WINDOW-END -->

5. Tester la configuration :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo nginx -t
> ```
<!-- CODE-WINDOW-END -->

6. Recharger Nginx sans coupure :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo nginx -s reload
> ```
<!-- CODE-WINDOW-END -->

7. Vérifier rapidement les en-têtes en production :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -sI https://exemple.com | grep -E "Strict|X-Frame|Content-Security|X-Content"
> curl -sI https://exemple.com/api/nvd/?cvssV3Severity=CRITICAL | grep -E "Strict|X-Frame|Content-Security|Cache"
> ```
<!-- CODE-WINDOW-END -->

8. Tester le rate limiting (NVD) — 429 attendu après le burst :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> for i in $(seq 1 10); do curl -so /dev/null -w "%{http_code}\n" https://exemple.com/api/nvd/; done
> ```
<!-- CODE-WINDOW-END -->

9. Vérifier que `/docs` retourne 404 (masquage de ressources internes) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -so /dev/null -w "%{http_code}\n" https://exemple.com/docs/security-audit.md
> ```
<!-- CODE-WINDOW-END -->

10. Exemple complet du fichier de conf `/etc/nginx/sites-available/exemple.com` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> server {
>     server_name exemple.com www.exemple.com;
>
>     server_tokens off;
>
>     # Charger les clés API depuis un fichier sécurisé séparé
>     include /etc/nginx/api-keys.conf;
>
>     # nginx est un pur reverse proxy — les fichiers du site sont sur 192.168.1.12
>     # root utilisé uniquement pour Certbot (acme-challenge servi localement)
>
>     # ── En-têtes de sécurité globaux ──────────────────────────────────────
>     # S'appliquent aux locations SANS leurs propres add_header (ex: /.well-known)
>     # Pour les autres locations : include snippets/security-headers.conf
>     # (héritage nginx cassé : chaque location avec add_header réapplique le snippet)
>     include /etc/nginx/snippets/security-headers.conf;
>
>     # ── Masquer les headers du backend interne ────────────────────────────
>     proxy_hide_header X-Powered-By;
>     proxy_hide_header Server;
>
>     # ── Protection Slowloris (si non défini dans nginx.conf) ──────────────
>     client_header_timeout 10s;
>     client_body_timeout   15s;
>
>     # ─────────────────────────────────────────────────────────────────────
>     # Renouvellement SSL Certbot (fichiers servis depuis la VM nginx)
>     # ─────────────────────────────────────────────────────────────────────
>     location /.well-known/acme-challenge/ {
>         root /var/www/html;
>         try_files $uri =404;
>     }
>
>     # ─────────────────────────────────────────────────────────────────────
>     # Proxy API CVE (NVD NIST)
>     # ─────────────────────────────────────────────────────────────────────
>     location /api/nvd/ {
>
>         # Rate limiting : 10 req/min par IP, burst de 5
>         limit_req zone=api_nvd burst=5 nodelay;
>         limit_req_status 429;
>
>         # Méthodes autorisées uniquement
>         limit_except GET OPTIONS { deny all; }
>
>         proxy_pass            https://services.nvd.nist.gov/rest/json/cves/2.0/;
>         proxy_ssl_server_name on;
>         proxy_ssl_name        services.nvd.nist.gov;
>         proxy_ssl_protocols   TLSv1.2 TLSv1.3;
>
>         proxy_set_header Host       services.nvd.nist.gov;
>         proxy_set_header apiKey     $nvd_api_key;
>         proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";
>
>         # Purger les headers d'auth que le client pourrait injecter
>         proxy_set_header Authorization "";
>         proxy_set_header Cookie        "";
>
>         # Masquer les headers techniques de la réponse NVD
>         proxy_hide_header X-RateLimit-Limit;
>         proxy_hide_header X-RateLimit-Remaining;
>         proxy_hide_header X-RateLimit-Reset;
>         proxy_hide_header X-Powered-By;
>         proxy_hide_header Server;
>         proxy_hide_header Via;
>         proxy_hide_header X-Cache;
>
>         # Cache (nécessite proxy_cache_path dans nginx.conf)
>         proxy_cache           api_cache;
>         proxy_cache_valid     200 1h;
>         proxy_cache_valid     404 1m;
>         proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
>         proxy_cache_lock      on;
>         add_header            X-Cache-Status $upstream_cache_status;
>
>         # ── Headers de sécurité (réapplication — héritage nginx corrigé) ──
>         include /etc/nginx/snippets/security-headers.conf;
>
>         # ── CORS ──────────────────────────────────────────────────────────
>         add_header Access-Control-Allow-Origin  "https://exemple.com" always;
>         add_header Access-Control-Allow-Methods "GET, OPTIONS"              always;
>         add_header Access-Control-Allow-Headers "Content-Type"              always;
>
>         # Preflight OPTIONS
>         if ($request_method = OPTIONS) {
>             add_header Access-Control-Allow-Origin  "https://exemple.com";
>             add_header Access-Control-Allow-Methods "GET, OPTIONS";
>             add_header Access-Control-Allow-Headers "Content-Type";
>             add_header Access-Control-Max-Age       3600;
>             add_header Content-Type                 "text/plain; charset=utf-8";
>             return 204;
>         }
>
>         proxy_connect_timeout 60s;
>         proxy_send_timeout    60s;
>         proxy_read_timeout    60s;
>     }
>
>     # ─────────────────────────────────────────────────────────────────────
>     # Proxy API VirusTotal
>     # ─────────────────────────────────────────────────────────────────────
>     location /api/vt/ {
>
>         # Rate limiting : 30 req/min par IP, burst de 10
>         limit_req zone=api_vt burst=10 nodelay;
>         limit_req_status 429;
>
>         # Limiter la taille du body (évite les uploads non contrôlés)
>         client_max_body_size 1m;
>
>         proxy_pass            https://www.virustotal.com/api/v3/;
>         proxy_ssl_server_name on;
>         proxy_ssl_name        www.virustotal.com;
>         proxy_ssl_protocols   TLSv1.2 TLSv1.3;
>
>         proxy_set_header Host       www.virustotal.com;
>         proxy_set_header x-apikey   $vt_api_key;
>         proxy_set_header Accept     "application/json";
>         proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";
>
>         # Purger les headers d'auth que le client pourrait injecter
>         proxy_set_header Authorization   "";
>         proxy_set_header Cookie          "";
>         proxy_set_header x-apikey-client "";
>
>         # Masquer les headers techniques de la réponse VT
>         proxy_hide_header x-apikey;
>         proxy_hide_header X-Cloud-Trace-Context;
>         proxy_hide_header X-GUploader-UploadID;
>         proxy_hide_header Via;
>         proxy_hide_header X-Powered-By;
>         proxy_hide_header Server;
>         proxy_hide_header X-RateLimit-Limit;
>         proxy_hide_header X-RateLimit-Remaining;
>         proxy_hide_header X-RateLimit-Reset;
>         proxy_hide_header Set-Cookie;
>
>         # Cache léger pour les analyses récentes
>         proxy_cache           api_cache;
>         proxy_cache_methods   GET HEAD;
>         proxy_cache_valid     200 10m;
>         proxy_cache_valid     404 1m;
>         proxy_cache_use_stale error timeout;
>         proxy_cache_lock      on;
>
>         # ── Headers de sécurité (réapplication — héritage nginx corrigé) ──
>         include /etc/nginx/snippets/security-headers.conf;
>
>         # ── CORS — Access-Control-Allow-Credentials supprimé (inutile) ───
>         add_header Access-Control-Allow-Origin  "https://exemple.com" always;
>         add_header Access-Control-Allow-Methods "GET, POST, OPTIONS"        always;
>         add_header Access-Control-Allow-Headers "Content-Type, Accept"      always;
>
>         # Preflight OPTIONS
>         if ($request_method = OPTIONS) {
>             add_header Access-Control-Allow-Origin  "https://exemple.com";
>             add_header Access-Control-Allow-Methods "GET, POST, OPTIONS";
>             add_header Access-Control-Allow-Headers "Content-Type, Accept";
>             add_header Access-Control-Max-Age       3600;
>             add_header Content-Type                 "text/plain; charset=utf-8";
>             return 204;
>         }
>
>         proxy_connect_timeout 30s;
>         proxy_send_timeout    30s;
>         proxy_read_timeout    30s;
>     }
>
>     # ─────────────────────────────────────────────────────────────────────
>     # Documentation interne — return 404 seul (évite le 403 qui révèle le path)
>     # ─────────────────────────────────────────────────────────────────────
>     location /docs {
>         return 404;
>     }
>
>     # ─────────────────────────────────────────────────────────────────────
>     # Site statique — proxy vers VM backend
>     # ─────────────────────────────────────────────────────────────────────
>     location / {
>         # Rate limiting global (protection scraping / DDoS applicatif)
>         limit_req zone=global burst=50 nodelay;
>
>         proxy_pass http://192.168.1.12;
>
>         proxy_set_header Host              $host;
>         # $remote_addr uniquement — évite le spoofing XFF par le client
>         proxy_set_header X-Real-IP         $remote_addr;
>         proxy_set_header X-Forwarded-For   $remote_addr;
>         proxy_set_header X-Forwarded-Proto $scheme;
>         proxy_set_header X-Forwarded-Host  $server_name;
>     }
>
>     listen 443 ssl;
>     listen [::]:443 ssl;
>     ssl_certificate     /etc/letsencrypt/live/exemple.com/fullchain.pem;
>     ssl_certificate_key /etc/letsencrypt/live/exemple.com/privkey.pem;
>     include             /etc/letsencrypt/options-ssl-nginx.conf;
>     ssl_dhparam         /etc/letsencrypt/ssl-dhparams.pem;
>
>     # Paramètres SSL gérés par nginx.conf + options-ssl-nginx.conf (Certbot)
> }
>
> # ─────────────────────────────────────────────────────────────────────────
> # Redirect HTTP → HTTPS (pattern simplifié sans if)
> # ─────────────────────────────────────────────────────────────────────────
> server {
>     listen 80;
>     listen [::]:80;
>     server_name exemple.com www.exemple.com;
>     return 301 https://$host$request_uri;
> }
> ```
<!-- CODE-WINDOW-END -->

Notes et bonnes pratiques
------------------------

- Remplacez `exemple.com`, les clés d'API et les adresses backend par vos valeurs.
- Ne publiez jamais vos clés API dans des dépôts publics ; utilisez des fichiers inclus hors dépôt et restreignez les permissions.
- Surveillez les logs Nginx et ajustez les valeurs de `limit_req` et `proxy_cache` selon le trafic réel.
- Testez les conséquences CORS si d'autres origines doivent interagir avec l'API.

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
