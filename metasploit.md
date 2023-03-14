# Metasploit

## Introduction

Metasploit est le framework le plus utilisé pour l'exploitation de vulnérabilités. Il est composé de plusieurs modules qui permettent de faire des scans, des exploits, des payloads, des post-exploitation, etc. Les principaux composants sont:

- msfconsole : Interface en ligne de commande
- Modules : Modules qui permettent de faire des scans, des exploits, des payloads, etc.
- Tools : Outils "stand-alone" aidant a la recherche de vulnérabilités, les tests de penetration,...
  
## Composants principaux

### Auxiliary

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/auxiliary`, il y a tout les modules tel que les scanners, crawler et fuzzer.

### Encoders

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/encoders`, il ya tout les encoders qui nous permettrons d'encoder nos payloads pour qu'il ne soit pas détecté par les antivirus.

### Evasion

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/evasion`, les modules d'evasion tente de contourner les protections tel que les antivirus.

### Exploits

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/exploits`, il y a toutes les vulnérabilités ordonnées par OS.

### Nops

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/nops`, les fichiers représente des CPU et des architectures.

### Payloads

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/payloads`, il y a tout les payloads qui peuvent être utilisés pour les exploits. Metasploit offre la possibilité d'envoyer différents payloads pouvant ouvrir des shells sur le système cible.
Il y a différents types de payloads:

- adapters: Les adaptateurs permette de convertir les payloads dans différents formats.
- singles: Payloads autonome executant une commande par exemple.
- stagers: Met en place un canal de connexion entre Metasploit et le système cible.
- stages: Télécharger par le stagers, permet d'utiliser des payloads de plus grosses tailles.

### Post

Situé dans le répertoire `/opt/metasploit-framework/embedded/framework/modules/post`, les post modules sont utilisé dans la dernières étape des test de pénétration.

## MSFConsole

Afin de lancer la console Metasploit, il y a la commande `msfconsole`. Il y a plusieurs commandes qui peuvent être utilisées dans la console tel que certaines commande Linux, la commande `help`qui affiche l'aide propre a msfconsole et les commandes disponible :

- `use` : Permet d'utiliser un module
- `show options` : Affiche les options du module
- `show payloads` : Affiche les payloads disponibles
- `back` : Permet de revenir au menu principal
- `info` : Affiche les informations du module
- `search` : Effectue une recherche en fonction du numero CVE,...

## Utiliser les modules

Nous avons différents commandes comme :

- `set LPORT 6666` : Permet de définir localement LPORT à 6666
- `setg RHOSTS 10.10.19.23` : Permet de définir la valeur global de RHOSTS
- `unset payload` : Permet de supprimer les valeur du scope payload
- `exploit -z` : Lance l'exploit. Une fois que la communication est établi, la session est mis en arrière plan
- `background` : Mettre la session meterpreter en arrière plan pour revenir a msfconsole
- `sessions` : Affiche les sessions ouvertes
- `sessions -i 1` : Permet de revenir sur la session meterpreter
