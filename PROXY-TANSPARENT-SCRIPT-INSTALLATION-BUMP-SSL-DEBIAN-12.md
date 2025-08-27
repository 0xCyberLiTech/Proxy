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
Optimisation SEO : mots-clés cybersécurité, Linux, administration système, sécurité informatique, tutoriels, guides, expertise, formation, supervision, Docker, OpenVAS, firewall, proxy, DNS, SSH, Debian, IT, réseau, cryptographie, open source, ressources techniques, étudiants, professionnels, passionnés.
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

## 🔐 Voici un script Bash complet pour Debian 12 qui :

- Installe Squid et les dépendances.
- Configure Squid en proxy transparent HTTP + HTTPS (SSL Bump).
- Crée un certificat CA autosigné.
- Configure les redirections avec iptables

A présent créer un fichier nommé install_squid_transparent.sh vers /usr/local/ puis copier le code ci-dessous dans ce fichier install_squid_transparent.sh.

```bash
nano /usr/local/install_squid_transparent.sh
```

```bash
#!/bin/bash

# === Variables à adapter ===
LAN_IF="eth1"     # Interface réseau locale
WAN_IF="eth0"     # Interface vers Internet
LOCAL_NET="192.168.1.0/24"  # Réseau local
CA_DIR="/etc/squid/ssl_cert"
SSL_DB="/var/lib/ssl_db"

echo "[+] Mise à jour du système"
apt update && apt upgrade -y

echo "[+] Installation de Squid et des outils SSL"
apt install -y squid openssl ssl-cert iptables iptables-persistent

echo "[+] Création du certificat CA"
mkdir -p $CA_DIR
cd $CA_DIR
openssl genrsa -out myCA.key 4096
openssl req -new -x509 -days 3650 -key myCA.key -out myCA.crt \
  -subj "/C=FR/ST=France/L=Paris/O=MonProxy/CN=MonProxyCA"

chown -R proxy:proxy $CA_DIR
chmod 700 $CA_DIR

echo "[+] Création de la base de certificats SSL"
mkdir -p $SSL_DB
/usr/lib/squid/security_file_certgen -c -s $SSL_DB -M 4MB
chown -R proxy:proxy $SSL_DB

echo "[+] Sauvegarde de la configuration Squid existante"
cp /etc/squid/squid.conf /etc/squid/squid.conf.bak

echo "[+] Écriture de la configuration Squid"
cat > /etc/squid/squid.conf <<EOF
http_port 3128 intercept
https_port 3129 intercept ssl-bump cert=$CA_DIR/myCA.crt key=$CA_DIR/myCA.key generate-host-certificates=on dynamic_cert_mem_cache_size=4MB

sslcrtd_program /usr/lib/squid/security_file_certgen -s $SSL_DB -M 4MB

acl step1 at_step SslBump1
ssl_bump peek step1
ssl_bump bump all

acl localnet src $LOCAL_NET
http_access allow localnet
http_access deny all
EOF

echo "[+] Activation du forwarding IP"
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p

echo "[+] Configuration des règles iptables"
iptables -F
iptables -t nat -F

# Redirection HTTP
iptables -t nat -A PREROUTING -i $LAN_IF -p tcp --dport 80 -j REDIRECT --to-port 3128
# Redirection HTTPS
iptables -t nat -A PREROUTING -i $LAN_IF -p tcp --dport 443 -j REDIRECT --to-port 3129
# NAT pour l'accès Internet
iptables -t nat -A POSTROUTING -o $WAN_IF -j MASQUERADE

echo "[+] Sauvegarde des règles iptables"
netfilter-persistent save

echo "[+] Redémarrage du service Squid"
systemctl restart squid
systemctl enable squid

echo "[✅] Installation terminée !"
echo "➡️  Pense à déployer le certificat CA sur les clients : $CA_DIR/myCA.crt"
```

Rendre exécutable ce script :

```bash
chmod +x /usr/local/install_squid_transparent.sh
```

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>
