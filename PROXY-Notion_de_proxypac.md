<!-- MASTER-HEADER-START -->
<div align="center">

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/headers/header-proxy.svg" alt="ATELIER PROXY // FILTRAGE APPLICATIF" width="100%" />
</a>

<p align="center">
  <a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-portal.svg" alt="Portail 0xCyberLiTech.com" width="24%" /></a>
  <a href="https://0xcyberlitech.com/soc-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-soc-rd.svg" alt="Livre Blanc SOC Cockpit" width="24%" /></a>
  <a href="https://0xcyberlitech.com/jarvis-rd.html"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-jarvis-rd.svg" alt="Livre Blanc JARVIS IA" width="24%" /></a>
  <a href="https://github.com/0xCyberLiTech?tab=repositories"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-github.svg" alt="Catalogue des 23 Dépôts" width="24%" /></a>
</p>
<p align="center">
  <a href="README.md"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/btn-back-readme.svg" alt="Retour au sommaire du dépôt" width="340" /></a>
</p>

<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />
</div>
<!-- MASTER-HEADER-END -->

<!-- DEBIAN-TRUTH-START -->
> [!IMPORTANT]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="18" height="18" align="absmiddle" alt="" /> ** Matrice Officielle Debian — `Debian 12 (Bookworm)` · `Debian 13 (Trixie)` · `Debian 14 (Forky)`**
> - **Sources Officielles Debian.org (`squid`) :** `Bookworm (12)` : **`5.7-2+deb12u5`** &nbsp;·&nbsp; `Trixie (13 — Stable)` : **`6.13-2+deb13u3`** &nbsp;·&nbsp; `Forky (14 — Next-Gen)` : **`7.7-1`**
> - **Références Officielles :** [Paquet Debian Trixie (`squid`)](https://packages.debian.org/trixie/squid) &nbsp;·&nbsp; [Paquet Debian Forky (`squid`)](https://packages.debian.org/forky/squid) &nbsp;·&nbsp; [Documentation Officielle Debian](https://packages.debian.org/trixie/squid)
> - **Note d'Ingénierie (Divergence 13 vs 14) :** Sous Debian 13 (Trixie) et Debian 14 (Forky), Squid 6+ privilégie l'inclusion modulaire dans **`/etc/squid/conf.d/`** plutôt que la modification directe du fichier maître `/etc/squid/squid.conf`.

<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-doc.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Afficher les Encarts Ligne de Commande : Debian 13 (Trixie — Stable) vs Debian 14 (Forky — Next-Gen)</b></summary>

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-ok.svg" width="42" height="16" align="absmiddle" alt="[OK]" /> Profil Production Stable — Commandes pour `Debian 13 (Trixie)`
```bash
# Debian 13 (Trixie — Squid 6.x) : Verification de la configuration & rechargement sans coupure
sudo squid -k parse && sudo systemctl reload squid
sudo tail -f /var/log/squid/access.log
```

#### <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-rd.svg" width="46" height="16" align="absmiddle" alt="[R&D]" /> Profil Anticipation Next-Gen — Commandes pour `Debian 14 (Forky)`
```bash
# Debian 14 (Forky — Squid 6.x/7.x) : Configuration modulaire dans /etc/squid/conf.d/*.conf
sudo journalctl -u squid.service -n 50 --no-hostname
```

</details>
<!-- DEBIAN-TRUTH-END -->

<!-- CIEL-PEDAGOGY-START -->
<details>
<summary><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-puzzle.svg" width="18" height="18" align="absmiddle" alt="" /> <b>Synthèse d'Architecture, Standards Officiels &amp; Diagnostic (Niveau 1 : Fondamentaux · Niveau 2 : Repères · Niveau 3 : Diagnostic)</b></summary>

- **Niveau 1 — Fondamentaux &amp; Principe d'Architecture :** Un **Serveur Proxy** agit comme passerelle intermédiaire de sécurité et de performance : en **Proxy Explicite / Sortant (Squid)**, il contrôle, journalise et met en cache les accès Web des postes internes ; en **Reverse Proxy (Nginx / HAProxy)**, il protège les applications internes derrière un point d'entrée unique (TLS, WAF, mTLS).
- **Niveau 2 — Repères Officiels (Ports IANA, Fichiers Debian &amp; Commandes de Validation) :** **Ports standards :** `TCP 3128` (Squid Forward Proxy) · `TCP 80 / 443` (Reverse Proxy HTTP/HTTPS) &nbsp;|&nbsp; **Fichiers Debian :** `/etc/squid/squid.conf`, `/etc/nginx/nginx.conf` &nbsp;|&nbsp; **Validation officielle :** `sudo squid -k parse` / `sudo nginx -t`.
- **Niveau 3 — Diagnostic d'Incidents &amp; Ingénierie Avancée :** **Diagnostic & Architecture :** Dans Squid, les directives `http_access` sont évaluées séquentiellement selon le principe du **premier match gagnant** : placez impérativement vos règles d'autorisation (`http_access allow <acl_lan>`) au-dessus de la règle de fermeture `http_access deny all`.
- **Sources de Vérité Officielles Éditeurs &amp; Standards :** [Documentation Officielle Squid Cache](https://www.squid-cache.org/Doc/config/) &nbsp;·&nbsp; [Documentation Officielle Nginx](https://nginx.org/en/docs/)

</details>
<!-- CIEL-PEDAGOGY-END -->


# <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-shield.svg" width="20" height="20" align="absmiddle" alt="" /> Guide Proxy PAC / WPAD

Cible : expliquer clairement ce qu'est un fichier PAC/WPAD, comment il fonctionne avec un proxy Squid, et donner des exemples concrets (wpad.dat, hébergement, WPAD DNS/DHCP, tests, sécurité).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 01 // Concepts clefs
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
- PAC (Proxy Auto‑Config) : script JavaScript local (fonction `FindProxyForURL(url, host)`) exécuté par le client pour décider `PROXY host:port` ou `DIRECT` pour chaque requête.
- WPAD (Web Proxy Auto‑Discovery) : mécanisme pour propager automatiquement l'URL du PAC aux clients (via DNS ou DHCP).

Pourquoi utiliser PAC/WPAD ?
- Déployer/révoquer des règles de proxy sans reconfigurer chaque poste.
- Appliquer des règles fines : bypass intranet, plusieurs proxys selon domaine, priorités.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 02 // Schéma simplifié (flux)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

Client → (WPAD DNS/DHCP) → URL PAC (ex. http://wpad.example.com/wpad.dat)
Client GET /wpad.dat → Serveur web (Apache) renvoie le PAC
Client exécute `FindProxyForURL` → décide `PROXY ip:3128` ou `DIRECT`
Si `PROXY` → requête envoyée à Squid (proxy explicite)

ASCII rapide :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> Client
>   |
>   +-- WPAD discovery (DNS/DHCP) -> wpad.example.com (192.0.2.10)
>   |
>   +-- HTTP GET /wpad.dat --> Apache (serve wpad.dat)
>   |
>   +-- Browser evaluates PAC -> returns "PROXY 192.0.2.20:3128" or "DIRECT"
>   |
>   +-- If PROXY: Browser sends request to Squid (192.0.2.20:3128)
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 03 // Exemple minimal de fichier PAC (wpad.dat)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> function FindProxyForURL(url, host) {
>   // accès direct pour intranet
>   if (dnsDomainIs(host, ".intra.example.com")) return "DIRECT";
>   // bypass pour IPs privées
>   if (isInNet(dnsResolve(host), "10.0.0.0", "255.0.0.0")) return "DIRECT";
>   // sinon utiliser proxy principal puis fallback direct
>   return "PROXY 192.0.2.20:3128; DIRECT";
> }
> ```
<!-- CODE-WINDOW-END -->

Notes :
- Fonctions utiles : `dnsDomainIs()`, `shExpMatch()`, `isInNet()`, `myIpAddress()`, `dnsResolve()`.
- Valeurs retournées : `PROXY host:port`, `SOCKS host:port`, `DIRECT`. Plusieurs alternatives séparées par `;`.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 04 // Héberger le fichier PAC (Apache example)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

1. Placer `/var/www/html/wpad.dat` (ou `wpad.dat`) contenant le PAC.
2. Forcer le bon Content‑Type pour certains clients :

Apache VirtualHost minimal :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```apache
> <VirtualHost *:80>
>   ServerName wpad.example.com
>   DocumentRoot /var/www/html
>   <Files "wpad.dat">
>     ForceType application/x-ns-proxy-autoconfig
>     Header set Cache-Control "max-age=600, must-revalidate"
>   </Files>
> </VirtualHost>
> ```
<!-- CODE-WINDOW-END -->

Cache : pour déploiement rapide utilisez `max-age` bas, puis augmentez.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 05 // WPAD discovery (déployer automatiquement)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />

- DNS (méthode recommandée interne) : ajouter un enregistrement A/CNAME `wpad` dans la zone DNS de votre domaine :
  - `wpad.example.com. A 192.0.2.10`
- DHCP (option 252) : configurer le serveur DHCP pour fournir l'URL `http://wpad.example.com/wpad.dat`.

Sécurité : WPAD peut être spoofé (risque d'empoisonnement). N'activez WPAD que sur des réseaux contrôlés, sécurisez DNS/DHCP.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 06 // Intégration avec Squid
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
- Le PAC déclare quel proxy utiliser (ex. `192.0.2.20:3128`). Squid écoute ce port en mode explicite (`http_port 3128` non `intercept`).
- Exemple ACL Squid basique :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```ini
> acl localnet src 10.0.0.0/8 192.168.0.0/16
> http_access allow localnet
> http_access deny all
> ```
<!-- CODE-WINDOW-END -->

Remarque : le PAC est côté client — Squid ne lit ni ne sert le PAC.

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 07 // Tests pratiques
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
- Vérifier accessibilité du PAC :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> curl -I http://wpad.example.com/wpad.dat
> ```
<!-- CODE-WINDOW-END -->

- Contrôler le contenu et le type MIME.
- Tester sur un poste : config manuelle du navigateur → pointer vers `http://wpad.example.com/wpad.dat` et observer le comportement.
- Sur le serveur Squid :

<!-- CODE-WINDOW-START -->
> [!CAUTION]
> <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-code-red.svg" width="100%" alt="TERMINAL // BLOC DE CODE" />
>
> ```bash
> sudo tail -f /var/log/squid/access.log
> ```
<!-- CODE-WINDOW-END -->

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 08 // Pièges et bonnes pratiques
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
- Toujours restreindre WPAD au réseau interne. WPAD public = risque de compromission.
- Tester `dnsResolve()` : certains navigateurs désactivent la résolution DNS depuis le PAC.
- Protéger le serveur web qui sert `wpad.dat` (auth unnecessary; accès interne suffit).
- Documenter les règles PAC pour l'équipe (qui fait quoi si changement).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 09 // Comparaisons (pour comprendre rapidement)
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
- PAC (client aware) vs Proxy explicite manuel : PAC = règles dynamiques côté client.
- PAC/WPAD vs Proxy transparent : PAC = explicit proxy configuré côté client (HTTPS géré normalement). Transparent = réseau redirige sans config client (HTTPS non filtrable sans ssl‑bump).

## <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/icons/wire-plan.svg" width="18" height="18" align="absmiddle" alt="" /> 10 // Check‑list rapide pour déploiement
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/glow-section-cyan.svg" width="100%" alt="" />
1. Créer `wpad.dat` et tester localement.
2. Héberger sur serveur web interne et définir MIME type.
3. Publier `wpad` en DNS interne (ou option DHCP 252).
4. Tester navigateurs (Chrome/Edge/Firefox) et vérifier logs Squid.
5. Surveiller cache et réduire TTL pendant rollout.

<!-- MASTER-FOOTER-START -->
<div align="center">
<img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/divider-hud.svg" alt="" width="100%" />

<a href="https://0xcyberlitech.com/">
  <img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/footer-master.svg" alt="0xCyberLiTech — Architecture Cyberdéfense, Ingénierie Déterministe & IA Souveraine" width="100%" />
</a>

<p align="center"><a href="https://0xcyberlitech.com/"><img src="https://raw.githubusercontent.com/0xCyberLiTech/0xcyberlitech/main/assets/badge-antigravity.svg" alt="Architecturé par Marc & Antigravity (Google DeepMind)" width="520" /></a></p>

<sub><b>Architecturé &amp; Forgé par Marc &amp; Antigravity (Google DeepMind)</b> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/"><b>0xcyberlitech.com</b></a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/soc-rd.html">Architecture SOC Cockpit v5.5</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/jarvis-rd.html">Architecture JARVIS Mark-VII</a> &nbsp;·&nbsp; <a href="https://0xcyberlitech.com/blueprint-jarvis.html">Blueprint Matériel RTX 5080</a></sub>

</div>
<!-- MASTER-FOOTER-END -->
