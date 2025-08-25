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

  [![🌐 Portfolio](https://img.shields.io/badge/Portfolio-0xCyberLiTech-181717?logo=github&style=flat-square)](https://0xcyberlitech.github.io/)
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

### 🎯 **Objectif de ce dépôt.**

> Ce dépôt a pour vocation de centraliser un ensemble de notions clés autour des proxy et reverse proxy. Il s'adresse aux passionnés, étudiants et professionnels souhaitant mieux comprendre les rôles stratégiques de ces technologies dans la
> gestion du trafic réseau, la protection des systèmes, et l’optimisation des performances.
> On y explore la mise en place de configurations adaptées à différents scénarios, ainsi que les concepts et bonnes pratiques pour garantir la sécurité, la scalabilité et la fiabilité des infrastructures web modernes.

---

### 🗂️ **Catégories des projets :**


| 🗂️ **Catégorie**     | 📄 **Description**                           | 🔗 **Accès rapide**                                                                                                                         |
|-----------------------|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| - Tuto proxy transparent.     | Installation & configuration   | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](PROXY-TANSPARENT-INSTALLATION-DEBIAN-12.md) |
| - Tuto proxy transparent + BUMP (SSL).     | Installation & configuration   | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](PROXY-TANSPARENT-BUMP-SSL-INSTALLATION-DEBIAN-12.md) |
| - Script proxy transparent + BUMP (SSL).     | Installation & configuration   | [<img src="https://img.shields.io/badge/EXPLORER-brightgreen?style=for-the-badge&logo=github&logoColor=white">](PROXY-TANSPARENT-SCRIPT-INSTALLATION-BUMP-SSL-DEBIAN-12.md) |

---

# 🧠 Les différents types de serveurs proxy

Un **serveur proxy** agit comme un **intermédiaire** entre un client (navigateur, application) et Internet. Il permet de **filtrer, sécuriser, anonymiser ou contrôler** le trafic réseau. Voici les principaux types de proxy existants, avec leurs usages et caractéristiques.

---

## 🔹 1. Proxy direct ou standard (**Forward Proxy**)

➡️ Placé entre le client et Internet.  
📌 Nécessite une configuration sur le client.  
✅ Utilisations :
- Contourner les restrictions géographiques ou réseau
- Anonymiser l’adresse IP du client
- Mettre en cache des contenus pour améliorer les performances

**Exemple :** un proxy HTTP configuré dans un navigateur pour accéder à un site bloqué.

---

## 🔹 2. Proxy transparent (**Transparent Proxy**)

➡️ Intercepte le trafic **sans configuration** sur le client.  
📌 Utilisé dans les réseaux d’entreprises, écoles ou hôtels.  
✅ Avantages :
- Invisible pour l’utilisateur
- Permet de filtrer ou rediriger le trafic HTTP automatiquement  
⚠️ Ne protège pas la vie privée (l’IP réelle reste visible).

---

## 🔹 3. Proxy inverse (**Reverse Proxy**)

➡️ Placé **devant les serveurs web**.  
📌 Sert d’intermédiaire entre les utilisateurs et les serveurs internes.  
✅ Utilisations :
- Répartition de charge (load balancing)
- Sécurisation des services web (WAF)
- Caching et optimisation de contenu statique

**Exemple :** NGINX ou HAProxy utilisé comme reverse proxy pour un site web.

---

## 🔹 4. Proxy SOCKS (**SOCKS4 / SOCKS5**)

➡️ Fonctionne au **niveau transport**, contrairement au proxy HTTP.  
📌 Peut transporter tout type de trafic (HTTP, FTP, SMTP, etc.).  
✅ SOCKS5 permet :
- Authentification
- Connexions UDP (streaming, jeux)

**Exemple :** Le réseau Tor utilise des proxys SOCKS5 pour anonymiser le trafic.

---

## 🔹 5. Proxy SSL / Bump SSL (**HTTPS Intercepting Proxy**)

➡️ Intercepte et inspecte le trafic **HTTPS** (chiffré).  
📌 Nécessite un **certificat racine installé** sur les clients.  
✅ Utilisé pour :
- Contrôle parental ou d’entreprise
- Analyse antivirus ou sécurité réseau (DLP)

⚠️ Peut compromettre la vie privée s’il est mal utilisé.

---

## 🔹 6. Niveaux d’anonymat des proxys

| Type de proxy     | Cache l’IP client ? | Modifie les en-têtes ? | Description |
|-------------------|---------------------|------------------------|-------------|
| **Transparent**   | ❌ Non              | ✅ Oui                 | L’IP client est visible |
| **Anonyme**       | ✅ Oui              | ✅ Oui                 | L’IP est cachée, mais détectable comme proxy |
| **Elite (Haut Anonymat)** | ✅ Oui      | ❌ Non                | Indétectable comme proxy |

---

## 📊 Tableau récapitulatif

| Type de Proxy        | Fonction principale              | Position           | Config. côté client |
|----------------------|----------------------------------|--------------------|----------------------|
| **Forward**          | Navigation via un intermédiaire | Avant Internet     | ✅ Oui               |
| **Transparent**      | Filtrage sans config client     | Avant Internet     | ❌ Non              |
| **Reverse**          | Protéger les serveurs web       | Devant les serveurs| ❌ Non              |
| **SOCKS**            | Acheminer tout type de trafic   | Avant Internet     | ✅ Oui               |
| **SSL Bump**         | Inspection du trafic HTTPS      | Avant Internet     | ✅ Oui (certificat)  |

---

## ✅ Conclusion

Les serveurs proxy sont des outils puissants pour :
- **Contrôler** et **filtrer** le trafic réseau
- **Protéger** la vie privée (ou au contraire l'inspecter dans un cadre sécurisé)
- **Optimiser** les performances web
- **Cacher** ou **révéler** l’identité réseau selon les besoins

👉 Le choix du type de proxy dépend de l’objectif : **sécurité, anonymat, performance ou contrôle**.

---

<p align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</p>

