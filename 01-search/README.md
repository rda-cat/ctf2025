# exploration / search

## introduction

Cette section présente la phase de reconnaissance qui a été effectuée dès que l'IP de la cible nous a été communiquée. Elle consiste à récupérer le maximum d'information *SANS* connexion ni tentative d'exploitation. Elle permets ensuite de construire un "plan d'attaque" pour trouver et récupérer les flags sur la machines.

## préparation de l'environnement

```sh
export RHOST=51.68.36.97
```

voir `.envrc` et l'outil `direnv`.

## Open Source INTelligence (OSINT)

### Whois

L'outil `whois` permets de récupérer des informations sur l'organisation à laquelle appartient une adresse IP.

```sh
whois $RHOST | grep -E -i "(OrgName:|address:|OrgTechName:|descr:)"
ddress:        2 rue Kellermann
address:        59100
address:        Roubaix
address:        FRANCE
address:        OVH SAS
address:        2 rue Kellermann
address:        59100 Roubaix
address:        France
```

La machine est donc une machine provided by [OVH Cloud](https://www.ovhcloud.com/fr/) ce qui fait du sens car il serait bien trop risqué d'avoir la machine cible sur le SI Catamania !

### Shodan.io

https://www.shodan.io/host/51.68.36.97

Le site montre deux éléments notables : un wordpress et un mysql.

machine asj.xxxxx.fr https://asj.xxxxx.fr/ <-- wordpress
```sh
nslookup asj.xxxxx.fr
Server:		127.0.0.53
Address:	127.0.0.53#53

Non-authoritative answer:
Name:	asj.xxxxx.fr
Address: XX.XX.XXX.X
```

(Je caviarde car on trouve le nom de famille d'un célèbre collaborateur).

le DNS ne résoud pas sur la même machine...

- ? fausse piste
- ? autre machine à gratter

Au final le rapport shodan montre des éléments qui sont en contradiction avec ce qui sera trouvés dans les autres phases. Cela provient du fait que la machine est un recyclage d'un développement wordpress pour un club de basket local et mis à disposition pour l'organisation du CTF.

## ports ouverts via nmap

`nmap` est l'outil qui va nous permettre de voir (sans s'y connecter!) ce que propose le challenge.

### discovery TCP

*tips:* parce certains scans, notamment les plus exhaustifs peuvent être trèèèèès long, il est vivement recommandé d'utiliser l'option `-o file_prefix` pour sauver les rapports de scan !

```sh
nmap -v -oA discovery.tcp -Pn -p- -T4 $RHOST
```

voir [discovery.tcp.nmap](./discovery.tcp.nmap)

Ports ouvertes notables:

- FTP 21
- SSH 22
- http 80
- https 443
- SMB 445
- ?? 53814 apache2 aussi ?

pas de mysql : confirme la fausse piste shodan ?

### discovery UDP

```sh
sudo nmap -v -sU -oA discovery.udp -Pn -p "0-65535" -T4 $RHOST
```

voir [discovery.udp.nmap](./discovery.udp.nmap)

Rien de notable.

## conclusion

On sait donc qu'il y a un apache et un nginx sur la machine, présentant a priori des challenges de type _Application Web_ sur les ports 80, 443, 8080 et 53814.

Il y a des ports consacrés à de l'échange de fichiers : `ftp` et `SMB` sur lesquels on fera face à des challenges d'authentification pour accèder à leurs contenus.

Le port `ssh` apparait aussi comme ouvert mais puisque la machine est une machine OVH il s'agit avant tout du port de management par l'organisation : quasiment aucune chance que ce soit un vecteur d'attaque potentiel.

Le plus facile semble être immédiatement `ftp` pour lequel `nmap` nous remonte que l'accès anonyme est autorisé ! direction [02-exploitation](../02-exploitation/).
