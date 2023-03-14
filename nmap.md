# Nmap

Source : <https://tryhackme.com/room/furthernmap>

## Introduction

Nmap est un utilitaire de reconnaissance IP afin de savoir quel services tournent sur une machine cible. Nmap utilise la technique du scan de ports. Chaque terminal a un total de port de 65535 (entier max de 16bit)

## Paramètres

| Arguments | Description |
| --------- | ----------- |
| -sT | Scan TCP|
| -sS | Half Open TCP SYN Scan|
| -sU | UDP Scan|
| -O | OS Detection |
| -v | Verbose mode |
| -vv | Verbose mode level 2 |
| -sV | version des services |
| -oA | Output en version normal, XML et greppable|
| -oN | Output en version normal|
| -oG | Output en version Greppable |
| -A | Mode agressif (Detection des services, des OS, traceroute et script)|
| -T<1-5> | 5 niveau pour les timings (Plus c'est haut, plus c'est detectable facilement) |
| -p 80 | Scan seulement le port 80 |
| -p 1000-1500 | Scan les port compris dans l'intervalle |
| -p- | Scan tous les ports |
| -script=vuln | Active les scripts de la category vuln|

## Scan TCP

D'après le RFC793, si une connexion n'existe pas, un flag RST est envoyé en retour. Le scan TCP s'effectue avec l'argument -sT.

## Scan SYN

Afin des bypass les anciens IDS qui vérifie un handshake en trois temps, nous pouvons utiliser le scan SYn (-sS) aussi appelé scan Half-Open, scan Stealth. ce scan requiert les permissions sudo.

## Scan UDP

Dû au fait que UDP est un protocole sans connexion, les scans sont plus difficile et plus lent. Afin d'effectuer un scan UPD, il faut utiliser l'argument -sU. Si un port ne répond pas, il est marqué en tant que "open|filtered". En revanche, si le port est fermé, un ping (ICMP) est renvoyé.

## Scans exotiques

Ces scans sont les moins utilisé:
| Arguments | Noms | Description |Capture |
| ---- | ---- | ---- | ---- |
| -sN | Null Scan | Paquet TCP envoyé avec aucun flag |![Null Scan](https://i.imgur.com/ABCxAwf.png) |
| -sF | FIN Scan | Paquet TCP envoyé avec le flag FIN |![FIN Scan](https://i.imgur.com/gIzKbEk.png) |
| -sX | Xmas Scan | Paquet TCP malformé envoyé (PSH, URG, FIN) |![Xmas Scan](https://i.imgur.com/gKVkGug.png) |
Le but de ces scan est principalement pour ne pas être intercepté par des firewalls. Comme par exemple Microsoft Windows qui renvoie un flag RST peut importe selon le port si un packet TCP malformé est reçus.

## Scan ICMP

Il est aussi possible d'effectuer un scan seulement avec des ping grace a la commande `nmap -sn 172.16.0.0/16`

## Script NSE

### Introduction aux NSE

Les NSE (Nmap Scripting Engine) sont écrit en Lua. Voici une liste non exhaustive :
| Categories | Description |
| ----- | ----- |
| safe | N'affecte pas la cible |
| intrusive | Affecte la cible (peut causer des crashs) |
| vuln | Check certaines vulnérabilités |
| exploit | Exploite certaines vulnérabilité |
| auth | Bypass certaines authentification  |
| brute | Bruteforce certains services |
| discovery | Recherche certains service (SNMP, SMB,...) |
Nous pouvons retrouver d'autres catégorie de script [ici](https://nmap.org/book/nse-usage.html)

### Utilisation des NSE

Nous pouvons lancer les script avec les arguments `--script=<script-name>` et `--script-args <script-args>`. Par exemple : `nmap -p 21 --script=ftp-anon --script-args ftp-anon.maxlist=-1`.
Nous pouvons trouver tout les scripts [ici](https://nmap.org/nsedoc/scripts/ftp-anon.html)

### Recherche de script NSE

Les scripts NSE son stocké dans le repertoire `/usr/share/nmap/scripts`. Nous pouvons recherche les scripts dans le fichier `/usr/share/nmap/scripts/script.db`
Nous pouvons donc recherche les scripts de la maniere suivante :

- `grep "safe" /usr/share/nmap/scripts/script.db`
- `ls -l /usr/share/nmap/scripts/*smb*`

Nous pouvons donc par exemple voir le script `smb-os-discovery.nse` qui determine l'OS du server Samba. Nous pouvons voir les dépendances dans la section `dependencies` du script.

## Évitement des Firewall

Par exemple, par défaut, le pare feu windows block tout les paquets ICMP. Nous avons donc l'argument `-Pn` qui envoie un ping seulement après le scan. Il y a d'autre argument tel que:
| Arguments | Description |
| ---- | ---- |
| -f | Fragments les paquets |
| -mtu \<number> | Défini la taille du paquet (*number* doit être un multiple de 8)|
| --scan-delay \<time>ms | Défini le temps entre chaque paquet |
| --badsum | Envoie des paquets avec un checksum invalide |
| --data-length \<number> | Défini la taille du paquet avec des données aléatoire a la fin du paquet|
