<div align="center">

<a href="https://github.com/0xCyberLiTech/Proxy/blob/main/PROXY-TANSPARENT-SCRIPT-INSTALLATION-BUMP-SSL-DEBIAN-12.md">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=D14A4A&center=true&vCenter=true&width=750&lines=Installation+Proxy+Transparent;Avec+Bump+SSL+sur+Debian+12;Script+Automatisé+Open+Source;Filtrage+HTTPS+et+HTTP;Sécurisation+et+Inspection+du+Traffic" alt="Typing SVG" />
</a>

<p align="center">
  <em>Un dépôt pédagogique autour du proxy & reverse-proxy.</em><br>
  <b>📘 Apprentissage – 🔐 Sécurité – 🧠 Compréhension</b>
</p>

</div>

---

## 👨‍💻 **À propos de moi.**

> Ce dépôt constitue mon laboratoire numérique où je consigne rigoureusement mes apprentissages et expérimentations. Passionné par l'écosystème Linux > et la cybersécurité, je
> documente mon parcours et mes projets sur mon GitHub. Vous y trouverez des guides pratiques sur la supervision (Zabbix,
> Nagios), la conteneurisation (Docker), la cryptographie les algorithmes de chiffrement symétrique (AES, ChaCha20) et asymétrique (RSA, ECC).  et la
> sécurisation de serveurs Debian. Mon objectif : partager mes connaissances de manière claire et pédagogique. N'hésitez pas à y jeter un œil : https://github.com/0xcyberlitech

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,grafana,prometheus,git,vim" />
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
  🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessible à tous.
</p>

