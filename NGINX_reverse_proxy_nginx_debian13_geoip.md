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

# Restriction géographique NGINX — Autoriser uniquement la France (FR)

## Objectif

Bloquer tout le trafic entrant sur NGINX sauf les IP géolocalisées en France (FR). Les routes Certbot restent accessibles depuis n'importe quelle IP (serveurs Let's Encrypt hors France).

## Architecture cible

Internet (toutes IPs)
        │
        ▼
   nginx (VM srv-ngix)
        │
        ├── IP hors France  →  403 Forbidden
        │
        ├── IP France       →  proxy → 10.10.10.12
        │
        └── `/.well-known/` →  pas de restriction (Certbot)

---

## Étape 1 — Installer le module nginx GeoIP2

```sh
apt install libnginx-mod-http-geoip2 -y
```

Notes:
- Le module est disponible dans les dépôts Debian Trixie.
- Il est chargé automatiquement via `/etc/nginx/modules-enabled/`.
- **Ne pas** ajouter `load_module` manuellement dans `nginx.conf` — cela provoquerait une erreur.

## Étape 2 — Installer `geoipupdate`

Le paquet `geoipupdate` n'est **pas** dans les dépôts apt de Debian Trixie. Utiliser le binaire `.deb` officiel depuis GitHub.

- Récupérer la version :

```sh
VER=$(curl -s https://api.github.com/repos/maxmind/geoipupdate/releases/latest \
  | grep -oP '"tag_name": "v\K[^"]+')
echo "Version : $VER"
```

- Télécharger le paquet `.deb` (le nom inclut la version) :

```sh
curl -LO "https://github.com/maxmind/geoipupdate/releases/download/v${VER}/geoipupdate_${VER}_linux_amd64.deb"
```

- Installer et nettoyer :

```sh
dpkg -i geoipupdate_${VER}_linux_amd64.deb
rm geoipupdate_${VER}_linux_amd64.deb
```

- Vérifier :

```sh
geoipupdate --version
# → geoipupdate 7.1.1 (exemple)
```

Erreurs courantes :
- `curl -LO .../geoipupdate_linux_amd64.deb` qui télécharge un fichier de 9 bytes → le nom du fichier requiert le numéro de version `$VER`.
- `dpkg: numéro magique de version` → fichier `.deb` corrompu (re-télécharger).

## Étape 3 — Créer un compte MaxMind et configurer `/etc/GeoIP.conf`

1. Créer un compte gratuit sur https://dev.maxmind.com
2. Générer une licence : *My Account → Manage License Keys → Generate new license key*
3. Éditer `/etc/GeoIP.conf` :

```text
AccountID  TON_ACCOUNT_ID
LicenseKey TA_LICENSE_KEY
EditionIDs GeoLite2-Country
```

Conseils :
- Ne pas ajouter `GeoLite2-City` (inutile ici et volumineux).
- `GeoLite2-Country` (~9 Mo) suffit pour la détection du pays.

## Étape 4 — Télécharger la base de données

```sh
geoipupdate -v
```

Sortie attendue :

```
Downloading GeoLite2-Country.mmdb
Database GeoLite2-Country successfully updated: <hash>
Lock file /usr/share/GeoIP/.geoipupdate.lock successfully released
```

Vérifier l'emplacement :

```sh
ls -lh /usr/share/GeoIP/
# → GeoLite2-Country.mmdb
```

Important : sur Debian Trixie avec ce binaire GitHub, la base s'installe dans `/usr/share/GeoIP/` (pas `/var/lib/GeoIP/`). Utiliser ce chemin dans `nginx.conf`.

## Étape 5 — Configurer `/etc/nginx/nginx.conf`

### 5a — Format de log enrichi avec le pays

Dans le bloc `http {}`, section *Logging Settings*, remplacer la directive d'accès par :

```sh
sed -i 's|access_log /var/log/nginx/access.log;|log_format main '"'$remote_addr [$time_local] "$request" $status $body_bytes_sent "$http_user_agent" country=$geoip2_data_country_code'"';\
\
        access_log /var/log/nginx/access.log main;|' /etc/nginx/nginx.conf
```

Vérifier (ex. lignes 37-46) :

```sh
sed -n '37,46p' /etc/nginx/nginx.conf
```

Extrait attendu :

```
##
# Logging Settings
##

log_format main '$remote_addr [$time_local] "$request" $status $body_bytes_sent "$http_user_agent" country=$geoip2_data_country_code';

access_log /var/log/nginx/access.log main;
```

### 5b — Bloc GeoIP2 + whitelist + map

Ajouter à la fin du bloc `http {}`, après les `include` :

```nginx
        # ── GeoIP2 — Restriction géographique France uniquement ───────────────
        geoip2 /usr/share/GeoIP/GeoLite2-Country.mmdb {
                auto_reload 1d;
                $geoip2_data_country_code country iso_code;
        }

        # ── Whitelist IPs fixes — bypass restriction géographique ─────────────
        geo $geo_whitelist {
                default          0;
                82.82.82.82/32   1;   # IP fixe publique admin
                10.10.10.0/24    1;   # réseau local (hairpin NAT)
        }

        # ── Détection pays ────────────────────────────────────────────────────
        map $geoip2_data_country_code $geo_country_blocked {
                default   1;
                FR        0;
        }

        # ── Résultat final : whitelisté OU France = autorisé ─────────────────
        map "$geo_whitelist:$geo_country_blocked" $geo_blocked {
                default  1;     # non-whitelisté + non-FR = bloqué
                "~^1:"   0;     # IP whitelistée = toujours autorisé
                "0:0"    0;     # France = autorisé
        }
```

Notes et erreurs courantes :
- Ne pas dupliquer le chemin dans la config (éviter `MMDB_open(".../GeoLite2-Country.mmdb/GeoLite2-Country.mmdb")`).
- Vérifier que le chemin pointe bien sur `/usr/share/GeoIP/GeoLite2-Country.mmdb`.

Pourquoi trois blocs `map` ?
- Un seul `map $geoip2_data_country_code $geo_blocked` ne peut pas tenir compte des IPs locales. La combinaison de `geo` + `map` permet d'overrider pour la whitelist.

Ajouter une IP à la whitelist :

```nginx
geo $geo_whitelist {
    default          0;
    82.82.82.82/32   1;   # IP fixe publique admin
    10.10.10.0/24    1;   # réseau local (hairpin NAT)
    X.X.X.X/32       1;   # autre IP à ajouter
}

nginx -t && systemctl reload nginx
```

## Étape 6 — Appliquer la restriction dans `/etc/nginx/sites-available/clt`

Ajouter `if ($geo_blocked) { return 403; }` dans les trois `location` concernées, juste après `limit_req_status` :

### location /api/nvd/

```nginx
        limit_req zone=api_nvd burst=5 nodelay;
        limit_req_status 429;

        # ── Restriction géographique — France uniquement ──────────────────
        if ($geo_blocked) { return 403; }
```

### location /api/vt/

```nginx
        limit_req zone=api_vt burst=10 nodelay;
        limit_req_status 429;

        # ── Restriction géographique — France uniquement ──────────────────
        if ($geo_blocked) { return 403; }
```

### location /

```nginx
        limit_req zone=global burst=50 nodelay;

        # ── Restriction géographique — France uniquement ──────────────────
        if ($geo_blocked) { return 403; }
```

Important : **Ne pas** ajouter la restriction sur `/.well-known/acme-challenge/` (Let’s Encrypt est hors France).

## Étape 7 — Tester et recharger

```sh
nginx -t
systemctl reload nginx
```

## Étape 8 — Vérifier que la restriction et la base GeoIP fonctionnent

Voir le contenu de la section "Tester via une API publique" et "Tester la base MaxMind en local" dans le tutoriel original pour les commandes `curl` et `mmdblookup`.

## Étape 9 — Commandes de surveillance des logs

Voir les commandes `tail`, `grep`, `awk` fournies dans le tutoriel pour surveiller les 403 et les connexions hors France.

## Étape 10 — Cron de mise à jour automatique de la base

```sh
echo "0 3 * * 3 root /usr/bin/geoipupdate && systemctl reload nginx" \
  > /etc/cron.d/geoipupdate

cat /etc/cron.d/geoipupdate
```

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
