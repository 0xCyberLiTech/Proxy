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

```nginx
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
add_header X-Frame-Options            "DENY"                          always;
add_header X-Content-Type-Options     "nosniff"                       always;
add_header X-XSS-Protection           "1; mode=block"                 always;
add_header Referrer-Policy            "strict-origin-when-cross-origin" always;
add_header Permissions-Policy         "geolocation=(), microphone=(), camera=(), payment=(), usb=(), bluetooth=()" always;
add_header Content-Security-Policy    "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:; font-src 'self'; connect-src 'self'; frame-ancestors 'none'; object-src 'none'; base-uri 'self';" always;
```

Pourquoi un fichier séparé ?

- Nginx réinitialise l'héritage des `add_header` au niveau d'un `location` qui définit ses propres `add_header`. En pratique, inclure ce snippet dans chaque `location` qui définit des headers garantit l'application cohérente des en-têtes de sécurité.

2 — Ajouts dans le bloc `http {}` de `/etc/nginx/nginx.conf`

Extraits recommandés (à ajouter dans `http {}`) :

```nginx
# Rate limiting
limit_req_zone $binary_remote_addr zone=api_nvd:10m rate=10r/m;
limit_req_zone $binary_remote_addr zone=api_vt:10m  rate=30r/m;
limit_req_zone $binary_remote_addr zone=global:10m  rate=60r/m;

# Cache proxy
proxy_cache_path /var/cache/nginx/api levels=1:2 keys_zone=api_cache:10m max_size=100m inactive=2h use_temp_path=off;

# Timeouts (protection Slowloris)
client_header_timeout  10s;
client_body_timeout    15s;
send_timeout           30s;
keepalive_timeout      65s;
```

3 — Exemple de serveur : `/etc/nginx/sites-available/exemple.com`

Le fichier de site fourni implémente :
- masquage des headers du backend (`proxy_hide_header`),
- réapplication des headers de sécurité dans chaque `location` (inclusion du snippet),
- protections rate-limiting par API (`/api/nvd/`, `/api/vt/`) avec `limit_req`,
- cache proxy pour réduire la charge sur les APIs externes (`proxy_cache`),
- gestion des préflight CORS pour chaque API,
- protection des chemins internes (`/docs` retourne `404` pour masquer l'existence).

Extraits importants (adapter les adresses, certificats et clés) :

```nginx
location /api/nvd/ {
    limit_req zone=api_nvd burst=5 nodelay;
    limit_req_status 429;
    limit_except GET OPTIONS { deny all; }

    proxy_pass            https://services.nvd.nist.gov/rest/json/cves/2.0/;
    proxy_ssl_server_name on;
    proxy_ssl_name        services.nvd.nist.gov;
    proxy_ssl_protocols   TLSv1.2 TLSv1.3;

    proxy_set_header Host      services.nvd.nist.gov;
    proxy_set_header apiKey    $nvd_api_key;
    proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";

    proxy_set_header Authorization "";
    proxy_set_header Cookie        "";

    proxy_cache           api_cache;
    proxy_cache_valid     200 1h;
    proxy_cache_valid     404 1m;
    proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
    proxy_cache_lock      on;
    add_header            X-Cache-Status $upstream_cache_status;

    include /etc/nginx/snippets/security-headers.conf;

    add_header Access-Control-Allow-Origin  "https://exemple.com" always;
    add_header Access-Control-Allow-Methods "GET, OPTIONS"              always;
    add_header Access-Control-Allow-Headers "Content-Type"              always;

    if ($request_method = OPTIONS) {
        add_header Access-Control-Allow-Origin  "https://exemple.com";
        add_header Access-Control-Allow-Methods "GET, OPTIONS";
        add_header Access-Control-Allow-Headers "Content-Type";
        add_header Access-Control-Max-Age       3600;
        add_header Content-Type                 "text/plain; charset=utf-8";
        return 204;
    }
}
```

4 — Exemple `nginx.conf` (extraits)

```nginx
user www-data;
worker_processes auto;

http {
    sendfile on;
    tcp_nopush on;
    server_tokens off;

    ssl_protocols TLSv1.2 TLSv1.3;

    access_log /var/log/nginx/access.log;
    gzip on;

    # Rate limiting (exemples définis ici si souhaité globalement)
    limit_req_zone $binary_remote_addr zone=api_nvd:10m rate=10r/m;
    limit_req_zone $binary_remote_addr zone=api_vt:10m  rate=30r/m;
    limit_req_zone $binary_remote_addr zone=global:10m  rate=60r/m;

    proxy_cache_path /var/cache/nginx/api levels=1:2 keys_zone=api_cache:10m max_size=100m inactive=2h use_temp_path=off;

    client_header_timeout  10s;
    client_body_timeout    15s;
    send_timeout           30s;
    keepalive_timeout      65s;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

Checklist de déploiement
------------------------

1. Créer le dossier cache et définir la propriété :

```bash
sudo mkdir -p /var/cache/nginx/api
sudo chown www-data:www-data /var/cache/nginx/api
```

2. Créer le snippet des headers de sécurité :

```bash
sudo mkdir -p /etc/nginx/snippets
sudo nano /etc/nginx/snippets/security-headers.conf
# (coller le contenu de la section 1 ci-dessus)
```

3. Ajouter les directives `http` dans `/etc/nginx/nginx.conf` (section 2).

4. Placer la configuration de site dans `/etc/nginx/sites-available/exemple.com` puis créer le lien :

```bash
sudo ln -s /etc/nginx/sites-available/exemple.com /etc/nginx/sites-enabled/exemple.com
```

5. Tester la configuration :

```bash
sudo nginx -t
```

6. Recharger Nginx sans coupure :

```bash
sudo nginx -s reload
```

7. Vérifier rapidement les en-têtes en production :

```bash
curl -sI https://exemple.com | grep -E "Strict|X-Frame|Content-Security|X-Content"
curl -sI https://exemple.com/api/nvd/?cvssV3Severity=CRITICAL | grep -E "Strict|X-Frame|Content-Security|Cache"
```

8. Tester le rate limiting (NVD) — 429 attendu après le burst :

```bash
for i in $(seq 1 10); do curl -so /dev/null -w "%{http_code}\n" https://exemple.com/api/nvd/; done
```

9. Vérifier que `/docs` retourne 404 (masquage de ressources internes) :

```bash
curl -so /dev/null -w "%{http_code}\n" https://exemple.com/docs/security-audit.md
```

10. Exemple complet du fichier de conf `/etc/nginx/sites-available/exemple.com` :

```nginx
server {
    server_name exemple.com www.exemple.com;

    server_tokens off;

    # Charger les clés API depuis un fichier sécurisé séparé
    include /etc/nginx/api-keys.conf;

    # nginx est un pur reverse proxy — les fichiers du site sont sur 192.168.1.12
    # root utilisé uniquement pour Certbot (acme-challenge servi localement)

    # ── En-têtes de sécurité globaux ──────────────────────────────────────
    # S'appliquent aux locations SANS leurs propres add_header (ex: /.well-known)
    # Pour les autres locations : include snippets/security-headers.conf
    # (héritage nginx cassé : chaque location avec add_header réapplique le snippet)
    include /etc/nginx/snippets/security-headers.conf;

    # ── Masquer les headers du backend interne ────────────────────────────
    proxy_hide_header X-Powered-By;
    proxy_hide_header Server;

    # ── Protection Slowloris (si non défini dans nginx.conf) ──────────────
    client_header_timeout 10s;
    client_body_timeout   15s;

    # ─────────────────────────────────────────────────────────────────────
    # Renouvellement SSL Certbot (fichiers servis depuis la VM nginx)
    # ─────────────────────────────────────────────────────────────────────
    location /.well-known/acme-challenge/ {
        root /var/www/html;
        try_files $uri =404;
    }

    # ─────────────────────────────────────────────────────────────────────
    # Proxy API CVE (NVD NIST)
    # ─────────────────────────────────────────────────────────────────────
    location /api/nvd/ {

        # Rate limiting : 10 req/min par IP, burst de 5
        limit_req zone=api_nvd burst=5 nodelay;
        limit_req_status 429;

        # Méthodes autorisées uniquement
        limit_except GET OPTIONS { deny all; }

        proxy_pass            https://services.nvd.nist.gov/rest/json/cves/2.0/;
        proxy_ssl_server_name on;
        proxy_ssl_name        services.nvd.nist.gov;
        proxy_ssl_protocols   TLSv1.2 TLSv1.3;

        proxy_set_header Host       services.nvd.nist.gov;
        proxy_set_header apiKey     $nvd_api_key;
        proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";

        # Purger les headers d'auth que le client pourrait injecter
        proxy_set_header Authorization "";
        proxy_set_header Cookie        "";

        # Masquer les headers techniques de la réponse NVD
        proxy_hide_header X-RateLimit-Limit;
        proxy_hide_header X-RateLimit-Remaining;
        proxy_hide_header X-RateLimit-Reset;
        proxy_hide_header X-Powered-By;
        proxy_hide_header Server;
        proxy_hide_header Via;
        proxy_hide_header X-Cache;

        # Cache (nécessite proxy_cache_path dans nginx.conf)
        proxy_cache           api_cache;
        proxy_cache_valid     200 1h;
        proxy_cache_valid     404 1m;
        proxy_cache_use_stale error timeout updating http_500 http_502 http_503 http_504;
        proxy_cache_lock      on;
        add_header            X-Cache-Status $upstream_cache_status;

        # ── Headers de sécurité (réapplication — héritage nginx corrigé) ──
        include /etc/nginx/snippets/security-headers.conf;

        # ── CORS ──────────────────────────────────────────────────────────
        add_header Access-Control-Allow-Origin  "https://exemple.com" always;
        add_header Access-Control-Allow-Methods "GET, OPTIONS"              always;
        add_header Access-Control-Allow-Headers "Content-Type"              always;

        # Preflight OPTIONS
        if ($request_method = OPTIONS) {
            add_header Access-Control-Allow-Origin  "https://exemple.com";
            add_header Access-Control-Allow-Methods "GET, OPTIONS";
            add_header Access-Control-Allow-Headers "Content-Type";
            add_header Access-Control-Max-Age       3600;
            add_header Content-Type                 "text/plain; charset=utf-8";
            return 204;
        }

        proxy_connect_timeout 60s;
        proxy_send_timeout    60s;
        proxy_read_timeout    60s;
    }

    # ─────────────────────────────────────────────────────────────────────
    # Proxy API VirusTotal
    # ─────────────────────────────────────────────────────────────────────
    location /api/vt/ {

        # Rate limiting : 30 req/min par IP, burst de 10
        limit_req zone=api_vt burst=10 nodelay;
        limit_req_status 429;

        # Limiter la taille du body (évite les uploads non contrôlés)
        client_max_body_size 1m;

        proxy_pass            https://www.virustotal.com/api/v3/;
        proxy_ssl_server_name on;
        proxy_ssl_name        www.virustotal.com;
        proxy_ssl_protocols   TLSv1.2 TLSv1.3;

        proxy_set_header Host       www.virustotal.com;
        proxy_set_header x-apikey   $vt_api_key;
        proxy_set_header Accept     "application/json";
        proxy_set_header User-Agent "0xCyberLiTech-Proxy/1.0";

        # Purger les headers d'auth que le client pourrait injecter
        proxy_set_header Authorization   "";
        proxy_set_header Cookie          "";
        proxy_set_header x-apikey-client "";

        # Masquer les headers techniques de la réponse VT
        proxy_hide_header x-apikey;
        proxy_hide_header X-Cloud-Trace-Context;
        proxy_hide_header X-GUploader-UploadID;
        proxy_hide_header Via;
        proxy_hide_header X-Powered-By;
        proxy_hide_header Server;
        proxy_hide_header X-RateLimit-Limit;
        proxy_hide_header X-RateLimit-Remaining;
        proxy_hide_header X-RateLimit-Reset;
        proxy_hide_header Set-Cookie;

        # Cache léger pour les analyses récentes
        proxy_cache           api_cache;
        proxy_cache_methods   GET HEAD;
        proxy_cache_valid     200 10m;
        proxy_cache_valid     404 1m;
        proxy_cache_use_stale error timeout;
        proxy_cache_lock      on;

        # ── Headers de sécurité (réapplication — héritage nginx corrigé) ──
        include /etc/nginx/snippets/security-headers.conf;

        # ── CORS — Access-Control-Allow-Credentials supprimé (inutile) ───
        add_header Access-Control-Allow-Origin  "https://exemple.com" always;
        add_header Access-Control-Allow-Methods "GET, POST, OPTIONS"        always;
        add_header Access-Control-Allow-Headers "Content-Type, Accept"      always;

        # Preflight OPTIONS
        if ($request_method = OPTIONS) {
            add_header Access-Control-Allow-Origin  "https://exemple.com";
            add_header Access-Control-Allow-Methods "GET, POST, OPTIONS";
            add_header Access-Control-Allow-Headers "Content-Type, Accept";
            add_header Access-Control-Max-Age       3600;
            add_header Content-Type                 "text/plain; charset=utf-8";
            return 204;
        }

        proxy_connect_timeout 30s;
        proxy_send_timeout    30s;
        proxy_read_timeout    30s;
    }

    # ─────────────────────────────────────────────────────────────────────
    # Documentation interne — return 404 seul (évite le 403 qui révèle le path)
    # ─────────────────────────────────────────────────────────────────────
    location /docs {
        return 404;
    }

    # ─────────────────────────────────────────────────────────────────────
    # Site statique — proxy vers VM backend
    # ─────────────────────────────────────────────────────────────────────
    location / {
        # Rate limiting global (protection scraping / DDoS applicatif)
        limit_req zone=global burst=50 nodelay;

        proxy_pass http://192.168.1.12;

        proxy_set_header Host              $host;
        # $remote_addr uniquement — évite le spoofing XFF par le client
        proxy_set_header X-Real-IP         $remote_addr;
        proxy_set_header X-Forwarded-For   $remote_addr;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Forwarded-Host  $server_name;
    }

    listen 443 ssl;
    listen [::]:443 ssl;
    ssl_certificate     /etc/letsencrypt/live/exemple.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/exemple.com/privkey.pem;
    include             /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam         /etc/letsencrypt/ssl-dhparams.pem;

    # Paramètres SSL gérés par nginx.conf + options-ssl-nginx.conf (Certbot)
}

# ─────────────────────────────────────────────────────────────────────────
# Redirect HTTP → HTTPS (pattern simplifié sans if)
# ─────────────────────────────────────────────────────────────────────────
server {
    listen 80;
    listen [::]:80;
    server_name exemple.com www.exemple.com;
    return 301 https://$host$request_uri;
}
```

Notes et bonnes pratiques
------------------------

- Remplacez `exemple.com`, les clés d'API et les adresses backend par vos valeurs.
- Ne publiez jamais vos clés API dans des dépôts publics ; utilisez des fichiers inclus hors dépôt et restreignez les permissions.
- Surveillez les logs Nginx et ajustez les valeurs de `limit_req` et `proxy_cache` selon le trafic réel.
- Testez les conséquences CORS si d'autres origines doivent interagir avec l'API.

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
