<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3EPROXY_" alt="Titre dynamique PROXY" />
  </a>
  
  <br></br>

  <h2>Laboratoire numérique pour la cybersécurité, Linux & IT</h2>

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

<!--
Optimisation SEO : mots-clés Proxy, 0xCyberLiTech, cybersécurité, Linux, sécurité informatique, tutoriels, guides, administration système, scripts Bash, Debian, proxy, reverse proxy, log, ressources techniques, étudiants, professionnels, formation, réseau, IT, bonnes pratiques, passionnés.
-->

<div align="center">
  <img src="https://img.icons8.com/fluency/96/000000/cyber-security.png" alt="CyberSec" width="80"/>
</div>

<div align="center">
  <p>
    <strong>Cybersécurité</strong> <img src="https://img.icons8.com/color/24/000000/lock--v1.png"/> • <strong>Linux Debian</strong> <img src="https://img.icons8.com/color/24/000000/linux.png"/> • <strong>Sécurité informatique</strong> <img src="https://img.icons8.com/color/24/000000/shield-security.png"/>
  </p>
</div>

---

## 🚀 À propos & Objectifs

Ce projet propose des solutions innovantes et accessibles en cybersécurité, avec une approche centrée sur la simplicité d’utilisation et l’efficacité. Il vise à accompagner les utilisateurs dans la protection de leurs données et systèmes, tout en favorisant l’apprentissage et le partage des connaissances.

Le contenu est structuré, accessible et optimisé SEO pour répondre aux besoins de :
- 🎓 Étudiants : approfondir les connaissances
- 👨‍💻 Professionnels IT : outils et pratiques
- 🖥️ Administrateurs système : sécuriser l’infrastructure
- 🛡️ Experts cybersécurité : ressources techniques
- 🚀 Passionnés du numérique : explorer les bonnes pratiques

---

## 🔐 Installer & configurer un serveur proxy Squid transparent + BUMP, sur Debian 12.

Parfait, tu veux donc aussi intercepter le trafic HTTPS via SSL Bump.

C’est plus complexe que pour le HTTP car il faut :

    • Générer un certificat CA local.
    • Configurer Squid pour faire un man-in-the-middle contrôlé.
    • Installer le certificat CA sur les clients (navigateurs ou systèmes d’exploitation).

## ⚠️ - AVERTISSEMENT IMPORTANT - ⚠️

Intercepter le HTTPS est intrusif. Cela casse la confidentialité des connexions SSL. Ne fais cela que sur un réseau que tu contrôles totalement, à des fins légitimes (ex : filtrage parental, entreprise, lab tests).

## ⚠️ - AVERTISSEMENT IMPORTANT - ⚠️

🛠 Étapes supplémentaires pour activer SSL Bump (HTTPS transparent)

## 1. Installer les paquets nécessaires

```bash
sudo apt install openssl ssl-cert
```

## 2. Générer un certificat d’autorité (CA)

```bash
sudo mkdir -p /etc/squid/ssl_cert
cd /etc/squid/ssl_cert
```

- Générer une clé privée

```bash
sudo openssl genrsa -out myCA.key 4096
```

- Générer un certificat autosigné (valide 10 ans)

```bash
sudo openssl req -new -x509 -days 3650 -key myCA.key -out myCA.crt \
    -subj "/C=FR/ST=France/L=Paris/O=MonProxy/CN=MonProxyCA"
```

## 3. Créer une base de certificats pour Squid

```bash
sudo chown -R proxy:proxy /etc/squid/ssl_cert
```
```bash
sudo chmod 700 /etc/squid/ssl_cert
```
- Créer un certificat dérivé pour chaque site intercepté

```bash
sudo /usr/lib/squid/security_file_certgen -c -s /var/lib/ssl_db -M 4MB
```

```bash
sudo chown -R proxy:proxy /var/lib/ssl_db
```
## 4. Modifier squid.conf pour le HTTPS.

Ajoute les lignes suivantes dans /etc/squid/squid.conf :

Configuration :

```bash
# HTTPS intercepté (port 3129)
https_port 3129 intercept ssl-bump cert=/etc/squid/ssl_cert/myCA.crt key=/etc/squid/ssl_cert/myCA.key generate-host-certificates=on dynamic_cert_mem_cache_size=4MB

# Active SSL Bump
sslcrtd_program /usr/lib/squid/security_file_certgen -s /var/lib/ssl_db -M 4MB

acl step1 at_step SslBump1
ssl_bump peek step1
ssl_bump bump all

# Autoriser le réseau local
acl localnet src 192.168.1.0/24
http_access allow localnet
http_access deny all
```

## 5. Rediriger le trafic HTTPS via iptables.

- Redirection du port 443 vers Squid (port 3129).
  
```bash
sudo iptables -t nat -A PREROUTING -i eth1 -p tcp --dport 443 -j REDIRECT --to-port 3129
```

## 6. Redémarrer Squid

```bash
sudo systemctl restart squid
```

## 7. Installer le certificat CA sur les clients.

### 💻 Pour Firefox / Chrome :
    • Exporte /etc/squid/ssl_cert/myCA.crt vers le poste client.
    • Installe-le comme autorité de certification de confiance.

Sur Firefox :

Paramètres → Vie privée et sécurité → Certificats → Voir les certificats → Autorités → Importer

### ✅ Test :

Sur un poste client :
    • Navigue vers https://example.com
    • Squid devrait intercepter le trafic, et tu devrais voir les logs dans :

```bash
sudo tail -f /var/log/squid/access.log
```

## 🔐 Bonus : filtrage HTTPS.

Tu peux filtrer les sites même s’ils sont en HTTPS :

Configuration :

```bash
acl interdits dstdomain .facebook.com .youtube.com
http_access deny interdits
```

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>
