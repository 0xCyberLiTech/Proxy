<div align="center">

  <a href="https://github.com/0xCyberLiTech/Proxy/blob/main/PROXY-TANSPARENT-INSTALLATION-DEBIAN-12.md">
    <img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=32&pause=1000&color=D14A4A&center=true&vCenter=true&width=800&lines=Tutoriel+Proxy+Squid+Transparent;Installation+sur+Debian+12;Configuration+Détaillée+Pas+à+Pas;Redirection+du+Traffic+HTTP;Proxy+Sans+Interception+SSL" alt="Typing SVG" />
  </a>

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

### 👨‍💻 **À propos de moi.**

> Bienvenue dans mon **laboratoire numérique personnel** dédié à l’apprentissage et à la vulgarisation de la cybersécurité.  
> Passionné par **Linux**, la **cryptographie** et les **systèmes sécurisés**, je partage ici mes notes, expérimentations et fiches pratiques.  
>  
> 🎯 **Objectif :** proposer un contenu clair, structuré et accessible pour étudiants, curieux et professionnels de l’IT.  
> 🔗 [Mon GitHub principal](https://github.com/0xCyberLiTech)

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim" alt="Skills" />
  </a>
</p>

---

## 🎯 **Objectif de ce dépôt.**

> Ce dépôt a pour vocation de centraliser un ensemble de notions clés autour des proxy et reverse proxy. Il s'adresse aux passionnés, étudiants et professionnels souhaitant mieux comprendre les rôles stratégiques de ces technologies dans la
> gestion du trafic réseau, la protection des systèmes, et l’optimisation des performances.
> On y explore la mise en place de configurations adaptées à différents scénarios, ainsi que les concepts et bonnes pratiques pour garantir la sécurité, la scalabilité et la fiabilité des infrastructures web modernes.

---

## 🔐 **Installer & configurer un serveur proxy Squid transparent, sur Debian 12**. 

Ce type de proxy intercepte automatiquement le trafic HTTP sans que les clients aient besoin de configurer manuellement leur navigateur.

## 🧰 Prérequis :
    • Un serveur Debian 12 (avec accès root ou sudo).
    • Deux interfaces réseau (idéalement) :
        ◦ eth0 connectée à Internet.
        ◦ eth1 connectée au réseau local.

## 🛠 Étapes d’installation et configuration :

### 1. Mise à jour du système.

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Installation de Squid.

```bash
sudo apt install squid -y
```

### 3. Sauvegarde de la configuration par défaut.

```bash
sudo cp /etc/squid/squid.conf /etc/squid/squid.conf.bak
```

### 4. Configuration de Squid en mode transparent.

Éditez le fichier de configuration :

```bash
sudo nano /etc/squid/squid.conf
```

Ajoutez ou modifiez les lignes suivantes :

Configuration :

```bash
# Port d'écoute en mode transparent
http_port 3128 intercept

# Autoriser l'accès au réseau local (à adapter selon votre plage réseau)
acl localnet src 192.168.1.0/24
http_access allow localnet
http_access deny all
```
Important : Remplacez 192.168.1.0/24 par votre plage réseau locale.

### 5. Configuration de l’IP forwarding.

Activez le routage IP :

```bash
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
```

```bash
sudo sysctl -p
```

### 6. Configuration d’iptables pour la redirection.

Redirigez le trafic HTTP entrant vers Squid :

- Videz les règles existantes (facultatif).

```bash
sudo iptables -F
```

- Redirection du port 80 vers 3128.
  
```bash
sudo iptables -t nat -A PREROUTING -i eth1 -p tcp --dport 80 -j REDIRECT --to-port 3128
```

- Autoriser le trafic NAT.

```bash
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```

- (Optionnel) sauvegarder les règles.

```bash
sudo apt install iptables-persistent
```
```bash
sudo netfilter-persistent save
```

### 7. Redémarrer Squid.

```bash
sudo systemctl restart squid
```

```bash
sudo systemctl enable squid
```

### 8. Tester le proxy transparent.

Depuis un poste client connecté à eth1, accédez à un site Web (http://example.com). Vous pouvez vérifier les journaux de Squid :

```bash
sudo tail -f /var/log/squid/access.log
```

## 🔐 Bonus : filtrage des sites Web.

Ajoutez un contrôle de contenu simple :

- Dans squid.conf.

```bash
acl interdits dstdomain .facebook.com .youtube.com
http_access deny interdits
```

## 🧪 Astuce de test.

Pour vérifier que le proxy fonctionne bien en mode transparent, utilisez curl depuis un client :

```bash
curl -I http://example.com
```

Vous devriez voir des logs dans /var/log/squid/access.log.

## 🧹 En cas de problème
    • Vérifiez que le trafic passe bien par l'interface eth1.
    • Vérifiez les règles iptables avec :

```bash
sudo iptables -t nat -L -n -v
 ```
   
    • Consultez les logs de Squid (/var/log/squid/cache.log).
    
---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>
