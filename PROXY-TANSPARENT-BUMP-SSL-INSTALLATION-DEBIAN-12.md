<div align="center">

<a href="https://github.com/0xCyberLiTech/Proxy/blob/main/PROXY-TANSPARENT-BUMP-SSL-INSTALLATION-DEBIAN-12.md">
  <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=D14A4A&center=true&vCenter=true&width=850&lines=Proxy+Squid+Transparent+avec+SSL+Bump;Inspection+HTTPS+et+Filtrage+Avancé;Installation+Automatisée+Debian+12;Redirection+du+Traffic+avec+iptables;Sécurité+et+Contrôle+du+Flux+Web" alt="Typing SVG" />
</a>

<p align="center">
  <em>Un dépôt pédagogique autour du proxy & reverse-proxy.</em><br>
  <b>📘 Apprentissage – 🔐 Sécurité – 🧠 Compréhension</b>
</p>

</div>

---

### 👨‍💻 **À propos de moi.**

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

### 🎯 **Objectif de ce dépôt.**

> Ce dépôt a pour vocation de centraliser un ensemble de notions clés autour des proxy et reverse proxy. Il s'adresse aux passionnés, étudiants et professionnels souhaitant mieux comprendre les rôles stratégiques de ces technologies dans la
> gestion du trafic réseau, la protection des systèmes, et l’optimisation des performances.
> On y explore la mise en place de configurations adaptées à différents scénarios, ainsi que les concepts et bonnes pratiques pour garantir la sécurité, la scalabilité et la fiabilité des infrastructures web modernes.

---

## 🔐 Installation serveur proxy bump (HTTPS / SSL) transparents, sur Debian 12.

Parfait, tu veux donc aussi intercepter le trafic HTTPS via SSL Bump. C’est plus complexe que pour le HTTP car il faut :

    • Générer un certificat CA local.
    • Configurer Squid pour faire un man-in-the-middle contrôlé.
    • Installer le certificat CA sur les clients (navigateurs ou systèmes d’exploitation).

⚠️ AVERTISSEMENT IMPORTANT :

Intercepter le HTTPS est intrusif. Cela casse la confidentialité des connexions SSL. Ne fais cela que sur un réseau que tu contrôles totalement, à des fins légitimes (ex : filtrage parental, entreprise, lab tests).

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

<p align="center">
  🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessible à tous.
</p>

