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

# Procédure d'installation et d'exploitation — Squid + SquidGuard + SquidAnalyzer

But : déployer un proxy Squid sur Debian 13, ajouter le filtrage SquidGuard et générer des rapports SquidAnalyzer. Inclut scripts pour récupération automatique des bases de filtrage.

Prérequis
- Debian 13 (amd64), accès root ou sudo.
- Connexion internet pour `apt`.
- Sauvegarder les fichiers de config existants avant modification.

Récapitulatif des scripts fournis (dossier `scripts/`):
- `install_squid.sh` : installe Squid, SquidGuard, (SquidAnalyzer si disponible) et dépendances.
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

Avant d'installer, vérifiez que les paquets sont présents dans les dépôts :

```bash
apt-cache policy squid squidguard squidanalyzer || true
```

Si `squidanalyzer` n'est pas disponible via `apt`, installez-le depuis le paquet du projet ou suivez l'installation depuis la source (voir section "Installation alternative").

Installer les paquets disponibles :

```bash
sudo apt install -y squid squidguard apache2-utils nftables wget tar bzip2
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

Remarques légales et sécurité
- N'interceptez pas le trafic HTTPS sans consentement et sans infrastructure de certificats appropriée.
- Vérifiez la provenance et la licence des listes téléchargées.

Installation alternative de SquidAnalyzer (si absent dans les dépôts)
- Si `squidanalyzer` n'est pas disponible via `apt`, téléchargez le tarball officiel ou le dépôt du projet, puis installez selon la doc du projet. Généralement :

```bash
wget <url-du-tarball>
tar xvf squidanalyzer-*.tar.gz
cd squidanalyzer-*
sudo ./install.sh   # ou suivre les instructions du README
```

- Après installation, vérifiez que les scripts d'analyse peuvent lire vos logs Squid et que les chemins dans `squidanalyzer.conf` correspondent à `/var/log/squid/access.log`.

Fichiers fournis
- Voir ../configs/ pour exemples de `squid.conf`, `squidGuard.conf` et `squidanalyzer.conf`.

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

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

