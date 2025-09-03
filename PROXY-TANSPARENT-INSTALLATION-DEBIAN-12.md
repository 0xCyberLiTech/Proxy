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

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>
