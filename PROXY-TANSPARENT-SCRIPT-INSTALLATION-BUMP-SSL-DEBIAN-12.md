<div align="center">

  <br></br>
  
  <a href="https://github.com/0xCyberLiTech">
    <img src="https://readme-typing-svg.herokuapp.com?font=JetBrains+Mono&size=50&duration=6000&pause=1000000000&color=FF0048&center=true&vCenter=true&width=1100&lines=%3EPROXY_" alt="Titre dynamique PROXY" />
  </a>
  
  <br></br>

  <p align="center">
    <em>Un dépôt pédagogique autour du proxy & reverse-proxy.</em><br>
    <b>📘 Apprentissage – 🔐 Sécurité – 🧠 Compréhension</b>
  </p>

  [![🔗 Profil GitHub](https://img.shields.io/badge/Profil-GitHub-181717?logo=github&style=flat-square)](https://github.com/0xCyberLiTech)
  [![📦 Dernière version](https://img.shields.io/github/v/release/0xCyberLiTech/Proxy?label=version&style=flat-square&color=blue)](https://github.com/0xCyberLiTech/Proxy/releases/latest)
  [![📄 CHANGELOG](https://img.shields.io/badge/📄%20Changelog-Proxy-blue?style=flat-square)](https://github.com/0xCyberLiTech/Proxy/blob/main/CHANGELOG.md)
  [![📂 Dépôts publics](https://img.shields.io/badge/Dépôts-publics-blue?style=flat-square)](https://github.com/0xCyberLiTech?tab=repositories)
  [![👥 Contributeurs](https://img.shields.io/badge/👥%20Contributeurs-cliquez%20ici-007ec6?style=flat-square)](https://github.com/0xCyberLiTech/Proxy/graphs/contributors)

</div>

---

### 👨‍💻 **À propos de moi**

> Bienvenue sur le dépôt <strong>0xCyberLiTech</strong>, votre laboratoire numérique pour l'<strong>apprentissage</strong> et la <strong>vulgarisation</strong> de la <strong>cybersécurité</strong>, de l'<strong>administration Linux Debian</strong> et de la <strong>sécurité informatique</strong>.
> Passionné par <strong>Linux</strong>, la <strong>cryptographie</strong>, la <strong>supervision réseau</strong> et les <strong>systèmes sécurisés</strong>, je partage ici des <strong>tutoriels</strong>, <strong>guides pratiques</strong>, <strong>fiches techniques</strong> et <strong>retours d'expérience</strong> pour renforcer vos compétences IT.
>
> 🎯 <strong>Objectif :</strong> Offrir un contenu structuré, accessible et optimisé pour le référencement naturel, destiné aux étudiants, professionnels, administrateurs système, experts en sécurité et curieux du monde numérique.

<p align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="420">
  </a>
</p>

---

## 🎯 **Objectif de ce dépôt.**

> Ce dépôt a pour vocation de centraliser un ensemble de notions clés autour des proxy et reverse proxy. Il s'adresse aux passionnés, étudiants et professionnels souhaitant mieux comprendre les rôles stratégiques de ces technologies dans la
> gestion du trafic réseau, la protection des systèmes, et l’optimisation des performances.
> On y explore la mise en place de configurations adaptées à différents scénarios, ainsi que les concepts et bonnes pratiques pour garantir la sécurité, la scalabilité et la fiabilité des infrastructures web modernes.

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
