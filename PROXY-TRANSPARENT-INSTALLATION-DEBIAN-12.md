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


# <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="20" height="20" align="absmiddle" alt="" /> Procédure d'installation et d'exploitation — Squid + SquidGuard + GoAccess

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

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt update
> sudo apt upgrade -y
> ```
<!-- CODE-WINDOW-END -->

2. Vérifier la disponibilité des paquets et installer :


Avant d'installer, vous pouvez vérifier la disponibilité des paquets :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> apt-cache policy squid squidguard goaccess || true
> ```
<!-- CODE-WINDOW-END -->

Sur Debian 13 (Trixie) `squid`, `squidguard` et `goaccess` sont disponibles dans les dépôts officiels ; l'installation standard via `apt` suffit normalement. Si vous avez besoin d'une version plus récente de `goaccess`, la compilation depuis la source reste une option (voir section "Installation et utilisation de GoAccess").

Installer les paquets requis (commande recommandée) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt install -y squid squidguard goaccess apache2 apache2-utils nftables wget tar bzip2
> ```
<!-- CODE-WINDOW-END -->

3. Créer l'arborescence DB pour SquidGuard et appliquer les droits utilisateur correctement :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo mkdir -p /var/lib/squidguard/db
> # Détecte l'utilisateur système utilisé par Squid (proxy ou squid)
> SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
> sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard
> ```
<!-- CODE-WINDOW-END -->

Utilisation du script de récupération des bases
- Le script `scripts/fetch_blacklists.sh` télécharge des hosts lists publiques (configurable) et les convertit en fichiers `domains` compatibles SquidGuard.
- Exemple d'exécution (exécuter avec `sudo` sur Debian) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo bash scripts/setup_db_dirs.sh
> sudo bash scripts/fetch_blacklists.sh
> sudo bash scripts/update_and_reload.sh
> ```
<!-- CODE-WINDOW-END -->

Automatisation (cron)
- Ajouter une tâche system cron (exemple quotidien à 04:30) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> 30 4 * * * root /usr/bin/bash /opt/squid/scripts/fetch_blacklists.sh >> /var/log/squid/fetch_blacklists.log 2>&1
> ```
<!-- CODE-WINDOW-END -->

(Remplacez `/opt/squid/scripts/` par le chemin complet où vous avez placé les scripts.)

Integration dans Squid
- Vérifier que le binaire de squidGuard est accessible et que `squid.conf` pointe vers lui. Exemple de vérification :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> grep -E "url_rewrite_program|url_rewrite_children|http_port" /etc/squid/squid.conf || true
> ```
<!-- CODE-WINDOW-END -->

- Exemple d'option (transparent interception) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> http_port 3128 intercept
> url_rewrite_program /usr/bin/squidGuard -c /etc/squid/squidGuard.conf
> # Ajuster url_rewrite_children selon charge (ex: 5)
> ```
<!-- CODE-WINDOW-END -->

- Exemple simple de redirection `nftables` (adapter selon votre table) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo nft add table ip nat
> sudo nft add chain ip nat prerouting { type nat hook prerouting priority 0 ; }
> sudo nft add rule ip nat prerouting tcp dport 80 redirect to :3128
> ```
<!-- CODE-WINDOW-END -->

Type de proxy utilisé
- Ce tutoriel prend en charge deux modes courants :
	- **Proxy explicite** (recommandé par défaut) : les clients configurent l'adresse et le port du proxy dans leur navigateur ou via GPO. Avantages : support natif HTTPS sans interception, contrôle clair, moins de contraintes réseau.
	- **Proxy transparent (interception)** : le proxy intercepte le trafic HTTP redirigé depuis le réseau (pas de configuration client). Avantages : déploiement sans config client. Inconvénients : ne gère pas HTTPS sans interception TLS (ssl-bump) et certificats privés, risques légaux/sécurité.

- Choix recommandé pour Debian 13 : **proxy explicite** pour un premier déploiement. Passez en mode transparent seulement si vous maîtrisez les implications (certificats, ssl-bump, routes). Pour activer le mode transparent, modifiez `squid.conf` :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> # Exemple pour interception transparente
> http_port 3128 intercept
> url_rewrite_program /usr/bin/squidGuard -c /etc/squid/squidGuard.conf
> ```
<!-- CODE-WINDOW-END -->

Et ajoutez une règle `nftables` redirigeant le trafic HTTP vers le proxy (exemple déjà ci‑dessus) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo nft add table ip nat
> sudo nft add chain ip nat prerouting { type nat hook prerouting priority 0 ; }
> sudo nft add rule ip nat prerouting tcp dport 80 redirect to :3128
> ```
<!-- CODE-WINDOW-END -->

- HTTPS : si vous envisagez d'intercepter HTTPS, documentez et mettez en place `ssl-bump` dans Squid, un CA interne signant les certificats, et informez les utilisateurs. Sans cela, le proxy ne peut pas filtrer le contenu chiffré.


Remarques légales et sécurité
- N'interceptez pas le trafic HTTPS sans consentement et sans infrastructure de certificats appropriée.
- Vérifiez la provenance et la licence des listes téléchargées.

Installation et utilisation de GoAccess
- Installer depuis les dépôts (méthode recommandée) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt update
> sudo apt install -y goaccess
> ```
<!-- CODE-WINDOW-END -->

- Si vous voulez la dernière version, compiler depuis la source (exemple) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt install -y build-essential libncursesw5-dev libgeoip-dev libtokyocabinet-dev git
> git clone https://github.com/allinurl/goaccess.git
> cd goaccess
> ./autogen.sh
> ./configure --enable-utf8 --enable-geoip=legacy
> make
> sudo make install
> ```
<!-- CODE-WINDOW-END -->

- Exemple d'utilisation (génère un rapport HTML) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> # rapport HTML statique
> sudo goaccess /var/log/squid/access.log -o /var/www/html/squid_report.html --log-format=COMBINED
>
> # ou mode web interactive (via un petit serveur)
> goaccess /var/log/squid/access.log --log-format=COMBINED --real-time-html -o /var/www/html/squid_realtime.html
> ```
<!-- CODE-WINDOW-END -->

- Adaptez `--log-format` si votre format de log Squid est personnalisé. Voir `man goaccess` pour les modèles de format.

Fichiers fournis
-- Voir ../configs/ pour exemples de `squid.conf`, `squidGuard.conf` et `goaccess.conf`.

Commandes systemd utiles
- Activer et démarrer Squid :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo systemctl enable --now squid
> sudo systemctl status squid --no-pager
> ```
<!-- CODE-WINDOW-END -->

- Recharger Squid après mise à jour de la DB SquidGuard :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo systemctl reload squid
> ```
<!-- CODE-WINDOW-END -->

Vérifications rapides après installation
- Vérifier service : `sudo systemctl is-active squid`
- Vérifier droits DB : `ls -ld /var/lib/squidguard /var/lib/squidguard/db`
- Tester squidGuard manuellement : `sudo squidGuard -C all` puis `sudo systemctl reload squid`

Support
- Si vous le souhaitez, j'adapte les fichiers [configs/*](README.md) aux plages IP de votre réseau et rends les scripts exécutables (`chmod +x`).

Accès au rapport HTML
- Le script `fetch_blacklists.sh` peut générer `/var/www/html/squid_report.html` via `goaccess`.
- Pour servir le rapport via HTTP : installez et activez Apache, puis fixez la propriété :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo apt update
> sudo apt install -y apache2
> sudo systemctl enable --now apache2
> sudo chown www-data:www-data /var/www/html/squid_report.html || true
> sudo chmod 644 /var/www/html/squid_report.html || true
> ```
<!-- CODE-WINDOW-END -->

- Accès depuis un navigateur : http://IP_DU_SERVEUR/squid_report.html
- Sécurité : limiter l'accès si le rapport contient des informations internes (firewall, VirtualHost restreint, ou auth HTTP).

**Scripts fournis et contenu (intégral)**

1) `scripts/setup_db_dirs.sh`

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> #!/bin/bash
> set -euo pipefail
> # Crée l'arborescence pour SquidGuard et applique les droits adaptés
> SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
> DB_DIR=/var/lib/squidguard/db
> sudo mkdir -p "$DB_DIR"
> sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard
> sudo chmod -R 750 /var/lib/squidguard
> echo "Created $DB_DIR and set owner to ${SQUID_USER}"
>
> ```
<!-- CODE-WINDOW-END -->

2) `scripts/fetch_blacklists.sh`

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> #!/bin/bash
> set -euo pipefail
> # Récupère plusieurs hosts-lists publiques, convertit en fichier domains pour SquidGuard
> # Configurez SOURCES selon vos préférences/licences
> BASE_DIR=/var/lib/squidguard/db/blacklists
> TMPDIR=$(mktemp -d)
> SOURCES=(
> 	"https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts"
> 	"https://someone.invalid/example-hosts.txt"
> )
>
> sudo mkdir -p "$BASE_DIR"
> sudo chown -R $(getent passwd proxy >/dev/null && echo proxy || echo squid):$(getent passwd proxy >/dev/null && echo proxy || echo squid) /var/lib/squidguard || true
>
> for url in "${SOURCES[@]}"; do
> 	fname="$TMPDIR/$(basename "$url")"
> 	echo "Fetching $url"
> 	if ! wget -q -O "$fname" "$url"; then
> 		echo "Warning: failed to download $url"
> 		continue
> 	fi
> 	# Extraire domaines depuis lines '0.0.0.0 host' ou '127.0.0.1 host' ou 'hostname' formats
> 	awk '{ if ($1 ~ /^(0\.0\.0\.0|127\.0\.0\.1)$/) print $2; else if ($1 ~ /^[0-9A-Za-z._-]+$/ && NF==1) print $1 }' "$fname" \
> 		| sed 's/^www\.//I' >> "$TMPDIR/all_domains.txt" || true
> done
>
> mkdir -p "$BASE_DIR"
> sort -u "$TMPDIR/all_domains.txt" 2>/dev/null || true
> sort -u "$TMPDIR/all_domains.txt" 2>/dev/null > "$TMPDIR/domains.sorted" || true
> sudo install -m 0644 -D "$TMPDIR/domains.sorted" "$BASE_DIR/domains"
>
> echo "Rebuild SquidGuard DB and reload squid"
> sudo squidGuard -C all || echo "squidGuard -C all returned non-zero"
> sudo systemctl reload squid || sudo systemctl reload squid || true
>
> # Génère un rapport HTML avec goaccess si disponible
> if command -v goaccess >/dev/null 2>&1; then
> 	HTML_OUT=/var/www/html/squid_report.html
> 	sudo mkdir -p $(dirname "$HTML_OUT")
> 	echo "Generating GoAccess HTML report to $HTML_OUT"
> 	# Utilise le format COMBINED par défaut ; adaptez si votre log Squid est différent
> 	sudo goaccess /var/log/squid/access.log -o "$HTML_OUT" --log-format=COMBINED || echo "goaccess failed to generate report"
> 	# Assurer que le fichier est lisible par le serveur web
> 	if [ -f "$HTML_OUT" ]; then
> 		sudo chown www-data:www-data "$HTML_OUT" || true
> 		sudo chmod 644 "$HTML_OUT" || true
> 	fi
> fi
>
> rm -rf "$TMPDIR"
> echo "fetch_blacklists: done. Domains written to $BASE_DIR/domains"
>
> ```
<!-- CODE-WINDOW-END -->

3) `scripts/update_and_reload.sh`

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> #!/bin/bash
> set -euo pipefail
> # Rebuild SquidGuard DB and reload squid
> sudo squidGuard -C all || { echo "squidGuard -C all failed"; exit 1; }
> sudo systemctl reload squid || sudo systemctl reload squid || true
> echo "update_and_reload: done"
> ```
<!-- CODE-WINDOW-END -->

4) `scripts/install_squid.sh`

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> #!/bin/bash
> set -euo pipefail
> # Script d'installation minimale pour Squid, SquidGuard et dépendances utiles
> sudo apt update
> sudo apt install -y squid squidguard goaccess apache2-utils nftables wget tar bzip2
>
> # Activer et démarrer services de base
> sudo systemctl enable --now squid || true
> sudo systemctl enable --now apache2 || true
>
> # Créer arborescence DB si nécessaire
> sudo mkdir -p /var/lib/squidguard/db
> SQUID_USER=$(getent passwd proxy >/dev/null && echo proxy || echo squid)
> sudo chown -R ${SQUID_USER}:${SQUID_USER} /var/lib/squidguard || true
>
> echo "install_squid: terminé. Vérifiez /etc/squid/squid.conf et /etc/squid/squidGuard.conf"
> ```
<!-- CODE-WINDOW-END -->

Usage rapide des scripts
- Rendre exécutables :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo chmod +x docs/scripts/*.sh
> ```
<!-- CODE-WINDOW-END -->

- Exécution (ordre recommandé) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo bash docs/scripts/install_squid.sh
> sudo bash docs/scripts/setup_db_dirs.sh
> sudo bash docs/scripts/fetch_blacklists.sh
> ```
<!-- CODE-WINDOW-END -->

- Pour automatiser via cron (exemple quotidien 04:30) :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> 30 4 * * * root /usr/bin/bash /path/to/your/docs/scripts/fetch_blacklists.sh >> /var/log/squid/fetch_blacklists.log 2>&1
> ```
<!-- CODE-WINDOW-END -->

Remplacez `/path/to/your/` par le chemin réel où vous avez placé les scripts.

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
