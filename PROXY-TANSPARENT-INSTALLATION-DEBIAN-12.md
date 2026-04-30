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

# Procédure d'installation et d'exploitation — Squid + SquidGuard + GoAccess

But : déployer un proxy Squid sur Debian 13, ajouter le filtrage SquidGuard et générer des rapports via GoAccess. Inclut scripts pour récupération automatique des bases de filtrage.

Prérequis
- Debian 13 (amd64), accès root ou sudo.
- Connexion internet pour `apt`.
- Sauvegarder les fichiers de config existants avant modification.

- Récapitulatif des scripts fournis (dossier `scripts/`):
- `install_squid.sh` : installe Squid, SquidGuard, (GoAccess recommandé) et dépendances.
- `setup_db_dirs.sh` : crée l'arborescence `/var/lib/squidguard/db`.
- `fetch_blacklists.sh` : récupère des hosts lists publiques, les convertit en fichiers `domains` pour SquidGuard puis lance `squidGuard -C all` et recharge Squid.
- `update_and_reload.sh` : wrapper pour rebuild DB et reload Squid.

Installation rapide (manuel)
1. Mettre à jour le système :

```bash
sudo apt update
sudo apt upgrade -y
```

2. Vérifier la disponibilité des paquets et installer :


Avant d'installer, vous pouvez vérifier la disponibilité des paquets :

```bash
apt-cache policy squid squidguard goaccess || true
```

Sur Debian 13 (Trixie) `squid`, `squidguard` et `goaccess` sont disponibles dans les dépôts officiels ; l'installation standard via `apt` suffit normalement. Si vous avez besoin d'une version plus récente de `goaccess`, la compilation depuis la source reste une option (voir section "Installation et utilisation de GoAccess").

Installer les paquets requis (commande recommandée) :

```bash
sudo apt install -y squid squidguard goaccess apache2 apache2-utils nftables wget tar bzip2
```

3. Créer l'arborescence DB pour SquidGuard et appliquer les droits utilisateur correctement :

```bash
sudo mkdir -p /var/lib/squidguard/db
# Détecte l'utilisateur système utilisé par Squid (proxy ou squid)
SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard
```

Utilisation du script de récupération des bases
- Le script `scripts/fetch_blacklists.sh` télécharge des hosts lists publiques (configurable) et les convertit en fichiers `domains` compatibles SquidGuard.
- Exemple d'exécution (exécuter avec `sudo` sur Debian) :

```bash
sudo bash scripts/setup_db_dirs.sh
sudo bash scripts/fetch_blacklists.sh
sudo bash scripts/update_and_reload.sh
```

Automatisation (cron)
- Ajouter une tâche system cron (exemple quotidien à 04:30) :

```
30 4 * * * root /usr/bin/bash /opt/squid/scripts/fetch_blacklists.sh >> /var/log/squid/fetch_blacklists.log 2>&1
```

(Remplacez `/opt/squid/scripts/` par le chemin complet où vous avez placé les scripts.)

Integration dans Squid
- Vérifier que le binaire de squidGuard est accessible et que `squid.conf` pointe vers lui. Exemple de vérification :

```bash
grep -E "url_rewrite_program|url_rewrite_children|http_port" /etc/squid/squid.conf || true
```

- Exemple d'option (transparent interception) :

```
http_port 3128 intercept
url_rewrite_program /usr/bin/squidGuard -c /etc/squid/squidGuard.conf
# Ajuster url_rewrite_children selon charge (ex: 5)
```

- Exemple simple de redirection `nftables` (adapter selon votre table) :

```bash
sudo nft add table ip nat
sudo nft add chain ip nat prerouting { type nat hook prerouting priority 0 ; }
sudo nft add rule ip nat prerouting tcp dport 80 redirect to :3128
```

Type de proxy utilisé
- Ce tutoriel prend en charge deux modes courants :
	- **Proxy explicite** (recommandé par défaut) : les clients configurent l'adresse et le port du proxy dans leur navigateur ou via GPO. Avantages : support natif HTTPS sans interception, contrôle clair, moins de contraintes réseau.
	- **Proxy transparent (interception)** : le proxy intercepte le trafic HTTP redirigé depuis le réseau (pas de configuration client). Avantages : déploiement sans config client. Inconvénients : ne gère pas HTTPS sans interception TLS (ssl-bump) et certificats privés, risques légaux/sécurité.

- Choix recommandé pour Debian 13 : **proxy explicite** pour un premier déploiement. Passez en mode transparent seulement si vous maîtrisez les implications (certificats, ssl-bump, routes). Pour activer le mode transparent, modifiez `squid.conf` :

```
# Exemple pour interception transparente
http_port 3128 intercept
url_rewrite_program /usr/bin/squidGuard -c /etc/squid/squidGuard.conf
```

Et ajoutez une règle `nftables` redirigeant le trafic HTTP vers le proxy (exemple déjà ci‑dessus) :

```bash
sudo nft add table ip nat
sudo nft add chain ip nat prerouting { type nat hook prerouting priority 0 ; }
sudo nft add rule ip nat prerouting tcp dport 80 redirect to :3128
```

- HTTPS : si vous envisagez d'intercepter HTTPS, documentez et mettez en place `ssl-bump` dans Squid, un CA interne signant les certificats, et informez les utilisateurs. Sans cela, le proxy ne peut pas filtrer le contenu chiffré.


Remarques légales et sécurité
- N'interceptez pas le trafic HTTPS sans consentement et sans infrastructure de certificats appropriée.
- Vérifiez la provenance et la licence des listes téléchargées.

Installation et utilisation de GoAccess
- Installer depuis les dépôts (méthode recommandée) :

```bash
sudo apt update
sudo apt install -y goaccess
```

- Si vous voulez la dernière version, compiler depuis la source (exemple) :

```bash
sudo apt install -y build-essential libncursesw5-dev libgeoip-dev libtokyocabinet-dev git
git clone https://github.com/allinurl/goaccess.git
cd goaccess
./autogen.sh
./configure --enable-utf8 --enable-geoip=legacy
make
sudo make install
```

- Exemple d'utilisation (génère un rapport HTML) :

```bash
# rapport HTML statique
sudo goaccess /var/log/squid/access.log -o /var/www/html/squid_report.html --log-format=COMBINED

# ou mode web interactive (via un petit serveur)
goaccess /var/log/squid/access.log --log-format=COMBINED --real-time-html -o /var/www/html/squid_realtime.html
```

- Adaptez `--log-format` si votre format de log Squid est personnalisé. Voir `man goaccess` pour les modèles de format.

Fichiers fournis
-- Voir ../configs/ pour exemples de `squid.conf`, `squidGuard.conf` et `goaccess.conf`.

Commandes systemd utiles
- Activer et démarrer Squid :

```bash
sudo systemctl enable --now squid
sudo systemctl status squid --no-pager
```

- Recharger Squid après mise à jour de la DB SquidGuard :

```bash
sudo systemctl reload squid
```

Vérifications rapides après installation
- Vérifier service : `sudo systemctl is-active squid`
- Vérifier droits DB : `ls -ld /var/lib/squidguard /var/lib/squidguard/db`
- Tester squidGuard manuellement : `sudo squidGuard -C all` puis `sudo systemctl reload squid`

Support
- Si vous le souhaitez, j'adapte les fichiers [configs/*](configs/) aux plages IP de votre réseau et rends les scripts exécutables (`chmod +x`).

Accès au rapport HTML
- Le script `fetch_blacklists.sh` peut générer `/var/www/html/squid_report.html` via `goaccess`.
- Pour servir le rapport via HTTP : installez et activez Apache, puis fixez la propriété :

```bash
sudo apt update
sudo apt install -y apache2
sudo systemctl enable --now apache2
sudo chown www-data:www-data /var/www/html/squid_report.html || true
sudo chmod 644 /var/www/html/squid_report.html || true
```

- Accès depuis un navigateur : http://IP_DU_SERVEUR/squid_report.html
- Sécurité : limiter l'accès si le rapport contient des informations internes (firewall, VirtualHost restreint, ou auth HTTP).

**Scripts fournis et contenu (intégral)**

1) `scripts/setup_db_dirs.sh`

```bash
#!/bin/bash
set -euo pipefail
# Crée l'arborescence pour SquidGuard et applique les droits adaptés
SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
DB_DIR=/var/lib/squidguard/db
sudo mkdir -p "$DB_DIR"
sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard
sudo chmod -R 750 /var/lib/squidguard
echo "Created $DB_DIR and set owner to ${SQUID_USER}"

```

2) `scripts/fetch_blacklists.sh`

```bash
#!/bin/bash
set -euo pipefail
# Récupère plusieurs hosts-lists publiques, convertit en fichier domains pour SquidGuard
# Configurez SOURCES selon vos préférences/licences
BASE_DIR=/var/lib/squidguard/db/blacklists
TMPDIR=$(mktemp -d)
SOURCES=(
	"https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts"
	"https://someone.invalid/example-hosts.txt"
)

sudo mkdir -p "$BASE_DIR"
sudo chown -R $(getent passwd proxy >/dev/null && echo proxy || echo squid):$(getent passwd proxy >/dev/null && echo proxy || echo squid) /var/lib/squidguard || true

for url in "${SOURCES[@]}"; do
	fname="$TMPDIR/$(basename "$url")"
	echo "Fetching $url"
	if ! wget -q -O "$fname" "$url"; then
		echo "Warning: failed to download $url"
		continue
	fi
	# Extraire domaines depuis lines '0.0.0.0 host' ou '127.0.0.1 host' ou 'hostname' formats
	awk '{ if ($1 ~ /^(0\.0\.0\.0|127\.0\.0\.1)$/) print $2; else if ($1 ~ /^[0-9A-Za-z._-]+$/ && NF==1) print $1 }' "$fname" \
		| sed 's/^www\.//I' >> "$TMPDIR/all_domains.txt" || true
done

mkdir -p "$BASE_DIR"
sort -u "$TMPDIR/all_domains.txt" 2>/dev/null || true
sort -u "$TMPDIR/all_domains.txt" 2>/dev/null > "$TMPDIR/domains.sorted" || true
sudo install -m 0644 -D "$TMPDIR/domains.sorted" "$BASE_DIR/domains"

echo "Rebuild SquidGuard DB and reload squid"
sudo squidGuard -C all || echo "squidGuard -C all returned non-zero"
sudo systemctl reload squid || sudo service squid reload || true

# Génère un rapport HTML avec goaccess si disponible
if command -v goaccess >/dev/null 2>&1; then
	HTML_OUT=/var/www/html/squid_report.html
	sudo mkdir -p $(dirname "$HTML_OUT")
	echo "Generating GoAccess HTML report to $HTML_OUT"
	# Utilise le format COMBINED par défaut ; adaptez si votre log Squid est différent
	sudo goaccess /var/log/squid/access.log -o "$HTML_OUT" --log-format=COMBINED || echo "goaccess failed to generate report"
	# Assurer que le fichier est lisible par le serveur web
	if [ -f "$HTML_OUT" ]; then
		sudo chown www-data:www-data "$HTML_OUT" || true
		sudo chmod 644 "$HTML_OUT" || true
	fi
fi

rm -rf "$TMPDIR"
echo "fetch_blacklists: done. Domains written to $BASE_DIR/domains"

```

3) `scripts/update_and_reload.sh`

```bash
#!/bin/bash
set -euo pipefail
# Rebuild SquidGuard DB and reload squid
sudo squidGuard -C all || { echo "squidGuard -C all failed"; exit 1; }
sudo systemctl reload squid || sudo service squid reload || true
echo "update_and_reload: done"
```

4) `scripts/install_squid.sh`

```bash
#!/bin/bash
set -euo pipefail
# Script d'installation minimale pour Squid, SquidGuard et dépendances utiles
sudo apt update
sudo apt install -y squid squidguard goaccess apache2-utils nftables wget tar bzip2

# Activer et démarrer services de base
sudo systemctl enable --now squid || true
sudo systemctl enable --now apache2 || true

# Créer arborescence DB si nécessaire
sudo mkdir -p /var/lib/squidguard/db
SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard || true

echo "install_squid: terminé. Vérifiez /etc/squid/squid.conf et /etc/squid/squidGuard.conf"
```

Usage rapide des scripts
- Rendre exécutables :

```bash
sudo chmod +x docs/scripts/*.sh
```

- Exécution (ordre recommandé) :

```bash
sudo bash docs/scripts/install_squid.sh
sudo bash docs/scripts/setup_db_dirs.sh
sudo bash docs/scripts/fetch_blacklists.sh
```

- Pour automatiser via cron (exemple quotidien 04:30) :

```
30 4 * * * root /usr/bin/bash /path/to/your/docs/scripts/fetch_blacklists.sh >> /var/log/squid/fetch_blacklists.log 2>&1
```

Remplacez `/path/to/your/` par le chemin réel où vous avez placé les scripts.

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

