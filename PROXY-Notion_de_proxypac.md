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

# Guide Proxy PAC / WPAD

Cible : expliquer clairement ce qu'est un fichier PAC/WPAD, comment il fonctionne avec un proxy Squid, et donner des exemples concrets (wpad.dat, hébergement, WPAD DNS/DHCP, tests, sécurité).

## Concepts clefs
- PAC (Proxy Auto‑Config) : script JavaScript local (fonction `FindProxyForURL(url, host)`) exécuté par le client pour décider `PROXY host:port` ou `DIRECT` pour chaque requête.
- WPAD (Web Proxy Auto‑Discovery) : mécanisme pour propager automatiquement l'URL du PAC aux clients (via DNS ou DHCP).

Pourquoi utiliser PAC/WPAD ?
- Déployer/révoquer des règles de proxy sans reconfigurer chaque poste.
- Appliquer des règles fines : bypass intranet, plusieurs proxys selon domaine, priorités.

## Schéma simplifié (flux)

Client → (WPAD DNS/DHCP) → URL PAC (ex. http://wpad.example.com/wpad.dat)
Client GET /wpad.dat → Serveur web (Apache) renvoie le PAC
Client exécute `FindProxyForURL` → décide `PROXY ip:3128` ou `DIRECT`
Si `PROXY` → requête envoyée à Squid (proxy explicite)

ASCII rapide :

Client
  |
  +-- DNS: wpad.example.com -> 192.0.2.10 (web)
  |
  +-- GET /wpad.dat --> Apache
  |
  +-- Evaluate PAC -> use PROXY 192.0.2.20:3128
  |
  +-- Browser -> Squid

## Exemple minimal de fichier PAC (wpad.dat)

```javascript
function FindProxyForURL(url, host) {
  // accès direct pour intranet
  if (dnsDomainIs(host, ".intra.example.com")) return "DIRECT";
  // bypass pour IPs privées
  if (isInNet(dnsResolve(host), "10.0.0.0", "255.0.0.0")) return "DIRECT";
  // sinon utiliser proxy principal puis fallback direct
  return "PROXY 192.0.2.20:3128; DIRECT";
}
```

Notes :
- Fonctions utiles : `dnsDomainIs()`, `shExpMatch()`, `isInNet()`, `myIpAddress()`, `dnsResolve()`.
- Valeurs retournées : `PROXY host:port`, `SOCKS host:port`, `DIRECT`. Plusieurs alternatives séparées par `;`.

## Héberger le fichier PAC (Apache example)

1. Placer `/var/www/html/wpad.dat` (ou `wpad.dat`) contenant le PAC.
2. Forcer le bon Content‑Type pour certains clients :

Apache VirtualHost minimal :

```
<VirtualHost *:80>
  ServerName wpad.example.com
  DocumentRoot /var/www/html
  <Files "wpad.dat">
    ForceType application/x-ns-proxy-autoconfig
    Header set Cache-Control "max-age=600, must-revalidate"
  </Files>
</VirtualHost>
```

Cache : pour déploiement rapide utilisez `max-age` bas, puis augmentez.

## WPAD discovery (déployer automatiquement)

- DNS (méthode recommandée interne) : ajouter un enregistrement A/CNAME `wpad` dans la zone DNS de votre domaine :
  - `wpad.example.com. A 192.0.2.10`
- DHCP (option 252) : configurer le serveur DHCP pour fournir l'URL `http://wpad.example.com/wpad.dat`.

Sécurité : WPAD peut être spoofé (risque d'empoisonnement). N'activez WPAD que sur des réseaux contrôlés, sécurisez DNS/DHCP.

## Intégration avec Squid
- Le PAC déclare quel proxy utiliser (ex. `192.0.2.20:3128`). Squid écoute ce port en mode explicite (`http_port 3128` non `intercept`).
- Exemple ACL Squid basique :

```
acl localnet src 10.0.0.0/8 192.168.0.0/16
http_access allow localnet
http_access deny all
```

Remarque : le PAC est côté client — Squid ne lit ni ne sert le PAC.

## Tests pratiques
- Vérifier accessibilité du PAC :

```bash
curl -I http://wpad.example.com/wpad.dat
```
- Contrôler le contenu et le type MIME.
- Tester sur un poste : config manuelle du navigateur → pointer vers `http://wpad.example.com/wpad.dat` et observer le comportement.
- Sur le serveur Squid :

```bash
sudo tail -f /var/log/squid/access.log
```

## Pièges et bonnes pratiques
- Toujours restreindre WPAD au réseau interne. WPAD public = risque de compromission.
- Tester `dnsResolve()` : certains navigateurs désactivent la résolution DNS depuis le PAC.
- Protéger le serveur web qui sert `wpad.dat` (auth unnecessary; accès interne suffit).
- Documenter les règles PAC pour l'équipe (qui fait quoi si changement).

## Comparaisons (pour comprendre rapidement)
- PAC (client aware) vs Proxy explicite manuel : PAC = règles dynamiques côté client.
- PAC/WPAD vs Proxy transparent : PAC = explicit proxy configuré côté client (HTTPS géré normalement). Transparent = réseau redirige sans config client (HTTPS non filtrable sans ssl‑bump).

## Check‑list rapide pour déploiement
1. Créer `wpad.dat` et tester localement.
2. Héberger sur serveur web interne et définir MIME type.
3. Publier `wpad` en DNS interne (ou option DHCP 252).
4. Tester navigateurs (Chrome/Edge/Firefox) et vérifier logs Squid.
5. Surveiller cache et réduire TTL pendant rollout.

---

<div align="center">
  <a href="https://github.com/0xCyberLiTech" target="_blank" rel="noopener">
    <img src="https://skillicons.dev/icons?i=linux,debian,bash,docker,nginx,git,vim,python,markdown" alt="Skills" width="440">
  </a>
</div>

<div align="center">
  <b>🔒 Un guide proposé par <a href="https://github.com/0xCyberLiTech">0xCyberLiTech</a> • Pour des tutoriels accessibles à tous. 🔒</b>
</div>

