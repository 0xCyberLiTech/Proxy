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


# <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="20" height="20" align="absmiddle" alt="" /> Restriction géographique NGINX — Autoriser uniquement la France (FR)

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 01 // Objectif
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Bloquer tout le trafic entrant sur NGINX sauf les IP géolocalisées en France (FR). Les routes Certbot restent accessibles depuis n'importe quelle IP (serveurs Let's Encrypt hors France).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 02 // Architecture cible
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Internet (toutes IPs)
        │
        ▼
   nginx (VM srv-nginx)
        │
        ├── IP hors France  →  403 Forbidden
        │
        ├── IP France       →  proxy → 10.10.10.12
        │
        └── `/.well-known/` →  pas de restriction (Certbot)

---

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 03 // Étape 1 — Installer le module nginx GeoIP2
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> apt install libnginx-mod-http-geoip2 -y
> ```
<!-- CODE-WINDOW-END -->

Notes:
- Le module est disponible dans les dépôts Debian Trixie.
- Il est chargé automatiquement via `/etc/nginx/modules-enabled/`.
- **Ne pas** ajouter `load_module` manuellement dans `nginx.conf` — cela provoquerait une erreur.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 04 // Étape 2 — Installer `geoipupdate`
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Le paquet `geoipupdate` n'est **pas** dans les dépôts apt de Debian Trixie. Utiliser le binaire `.deb` officiel depuis GitHub.

- Récupérer la version :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> VER=$(curl -s https://api.github.com/repos/maxmind/geoipupdate/releases/latest \
>   | grep -oP '"tag_name": "v\K[^"]+')
> echo "Version : $VER"
> ```
<!-- CODE-WINDOW-END -->

- Télécharger le paquet `.deb` (le nom inclut la version) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -LO "https://github.com/maxmind/geoipupdate/releases/download/v${VER}/geoipupdate_${VER}_linux_amd64.deb"
> ```
<!-- CODE-WINDOW-END -->

- Installer et nettoyer :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> dpkg -i geoipupdate_${VER}_linux_amd64.deb
> rm geoipupdate_${VER}_linux_amd64.deb
> ```
<!-- CODE-WINDOW-END -->

- Vérifier :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> geoipupdate --version
> # → geoipupdate 7.1.1 (exemple)
> ```
<!-- CODE-WINDOW-END -->

Erreurs courantes :
- `curl -LO .../geoipupdate_linux_amd64.deb` qui télécharge un fichier de 9 bytes → le nom du fichier requiert le numéro de version `$VER`.
- `dpkg: numéro magique de version` → fichier `.deb` corrompu (re-télécharger).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 05 // Étape 3 — Créer un compte MaxMind et configurer `/etc/GeoIP.conf`
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

1. Créer un compte gratuit sur https://dev.maxmind.com
2. Générer une licence : *My Account → Manage License Keys → Generate new license key*
3. Éditer `/etc/GeoIP.conf` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> AccountID  TON_ACCOUNT_ID
> LicenseKey TA_LICENSE_KEY
> EditionIDs GeoLite2-Country
> ```
<!-- CODE-WINDOW-END -->

Conseils :
- Ne pas ajouter `GeoLite2-City` (inutile ici et volumineux).
- `GeoLite2-Country` (~9 Mo) suffit pour la détection du pays.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 06 // Étape 4 — Télécharger la base de données
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> geoipupdate -v
> ```
<!-- CODE-WINDOW-END -->

Sortie attendue :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> Downloading GeoLite2-Country.mmdb
> Database GeoLite2-Country successfully updated: <hash>
> Lock file /usr/share/GeoIP/.geoipupdate.lock successfully released
> ```
<!-- CODE-WINDOW-END -->

Vérifier l'emplacement :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> ls -lh /usr/share/GeoIP/
> # → GeoLite2-Country.mmdb
> ```
<!-- CODE-WINDOW-END -->

Important : sur Debian Trixie avec ce binaire GitHub, la base s'installe dans `/usr/share/GeoIP/` (pas `/var/lib/GeoIP/`). Utiliser ce chemin dans `nginx.conf`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 07 // Étape 5 — Configurer `/etc/nginx/nginx.conf`
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 5a — Format de log enrichi avec le pays

Dans le bloc `http {}`, section *Logging Settings*, remplacer la directive d'accès par :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sed -i 's|access_log /var/log/nginx/access.log;|log_format main '"'$remote_addr [$time_local] "$request" $status $body_bytes_sent "$http_user_agent" country=$geoip2_data_country_code'"';\
> \
>         access_log /var/log/nginx/access.log main;|' /etc/nginx/nginx.conf
> ```
<!-- CODE-WINDOW-END -->

Vérifier (ex. lignes 37-46) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sed -n '37,46p' /etc/nginx/nginx.conf
> ```
<!-- CODE-WINDOW-END -->

Extrait attendu :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> ##
> # Logging Settings
> ##
>
> log_format main '$remote_addr [$time_local] "$request" $status $body_bytes_sent "$http_user_agent" country=$geoip2_data_country_code';
>
> access_log /var/log/nginx/access.log main;
> ```
<!-- CODE-WINDOW-END -->

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> 5b — Bloc GeoIP2 + whitelist + map

Ajouter à la fin du bloc `http {}`, après les `include` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
>         # ── GeoIP2 — Restriction géographique France uniquement ───────────────
>         geoip2 /usr/share/GeoIP/GeoLite2-Country.mmdb {
>                 auto_reload 1d;
>                 $geoip2_data_country_code country iso_code;
>         }
>
>         # ── Whitelist IPs fixes — bypass restriction géographique ─────────────
>         geo $geo_whitelist {
>                 default          0;
>                 82.82.82.82/32   1;   # IP fixe publique admin
>                 10.10.10.0/24    1;   # réseau local (hairpin NAT)
>         }
>
>         # ── Détection pays ────────────────────────────────────────────────────
>         map $geoip2_data_country_code $geo_country_blocked {
>                 default   1;
>                 FR        0;
>         }
>
>         # ── Résultat final : whitelisté OU France = autorisé ─────────────────
>         map "$geo_whitelist:$geo_country_blocked" $geo_blocked {
>                 default  1;     # non-whitelisté + non-FR = bloqué
>                 "~^1:"   0;     # IP whitelistée = toujours autorisé
>                 "0:0"    0;     # France = autorisé
>         }
> ```
<!-- CODE-WINDOW-END -->

Notes et erreurs courantes :
- Ne pas dupliquer le chemin dans la config (éviter `MMDB_open(".../GeoLite2-Country.mmdb/GeoLite2-Country.mmdb")`).
- Vérifier que le chemin pointe bien sur `/usr/share/GeoIP/GeoLite2-Country.mmdb`.

Pourquoi trois blocs `map` ?
- Un seul `map $geoip2_data_country_code $geo_blocked` ne peut pas tenir compte des IPs locales. La combinaison de `geo` + `map` permet d'overrider pour la whitelist.

Ajouter une IP à la whitelist :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> geo $geo_whitelist {
>     default          0;
>     82.82.82.82/32   1;   # IP fixe publique admin
>     10.10.10.0/24    1;   # réseau local (hairpin NAT)
>     X.X.X.X/32       1;   # autre IP à ajouter
> }
>
> nginx -t && systemctl reload nginx
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 08 // Étape 6 — Appliquer la restriction dans `/etc/nginx/sites-available/clt`
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Ajouter `if ($geo_blocked) { return 403; }` dans les trois `location` concernées, juste après `limit_req_status` :

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> location /api/nvd/

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
>         limit_req zone=api_nvd burst=5 nodelay;
>         limit_req_status 429;
>
>         # ── Restriction géographique — France uniquement ──────────────────
>         if ($geo_blocked) { return 403; }
> ```
<!-- CODE-WINDOW-END -->

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> location /api/vt/

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
>         limit_req zone=api_vt burst=10 nodelay;
>         limit_req_status 429;
>
>         # ── Restriction géographique — France uniquement ──────────────────
>         if ($geo_blocked) { return 403; }
> ```
<!-- CODE-WINDOW-END -->

### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-target.svg" width="18" height="18" align="absmiddle" alt="" /> location /

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
>         limit_req zone=global burst=50 nodelay;
>
>         # ── Restriction géographique — France uniquement ──────────────────
>         if ($geo_blocked) { return 403; }
> ```
<!-- CODE-WINDOW-END -->

Important : **Ne pas** ajouter la restriction sur `/.well-known/acme-challenge/` (Let’s Encrypt est hors France).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 09 // Étape 7 — Tester et recharger
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> nginx -t
> systemctl reload nginx
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 10 // Étape 8 — Vérifier que la restriction et la base GeoIP fonctionnent
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Voir le contenu de la section "Tester via une API publique" et "Tester la base MaxMind en local" dans le tutoriel original pour les commandes `curl` et `mmdblookup`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 11 // Étape 9 — Commandes de surveillance des logs
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Voir les commandes `tail`, `grep`, `awk` fournies dans le tutoriel pour surveiller les 403 et les connexions hors France.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 12 // Étape 10 — Cron de mise à jour automatique de la base
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> echo "0 3 * * 3 root /usr/bin/geoipupdate && systemctl reload nginx" \
>   > /etc/cron.d/geoipupdate
>
> cat /etc/cron.d/geoipupdate
> ```
<!-- CODE-WINDOW-END -->

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
