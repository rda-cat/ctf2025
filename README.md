# ctf2025

Write-up par Raphaël D. pour l'équipe `teamName=N0N4M3[default("Best Security Operational Developpers")`

Membres:

- Daoiya-Nadia H. (Paris)
- Geoffrey B. (Niort)
- Simon B. (Niort)
- Raphaël D. (Structure / Niort)
- Source Anonyme (Anonyme)


Playlist musicale pour accompagner la lecture du rapport : https://www.youtube.com/watch?v=L_XJ_s5IsQc&list=PL6br-ciY5WMtWleRhuOhomOhBav0EkCNq

C'est le genre de musique qui m'accompagné durant le CTF.

## introduction

ce document contient un **write-up** du challenge présenté par le CTF2025 de Catamania. **Il ne s'agit pas d'un rapport de pentesting.** Ce write-up illustre toutefois les techniques classiques de pentesting, notament la collecte d'information, le scanning, l'assesment de vulnérabilité, l'exploitation, le social engineering et ce document, le reporting.

Le format choisi est un repo `git`. avec les fichiers, les payloads, des makefiles etc... Pour nous, il s'agit d'un format bien mieux adapté qu'un rapport de type exam OCSP ou de pentesting Word : dire à l'organisation du CTF que la machine est daubée ca ne leur apportera rien.

Il est destiné à donner aux participants du challenge qui n'ont pas pu trouvé toutes les valeurs notre approche pour sa résolution. Comment on a rebondi d'une phase à l'autre, comment on a essayé d'approfindir dès qu'on a trouvé un truc qui clochait. Le tout dans un format facile, avec une rédaction légère qui devrait permettre à des néophytes de comprendre et expérimenter de leur côté.

## contenu du challenge CTF2025

Le challenge consiste à retrouver 10 flags valant de 10 à 100 points, par pallier de 10, pour un total de 450 points. (10\*10\*9/2 selon Gauss).

Le challenge a été mené au sein de l'équipe en individuel pour laisser à chacun le temps d'essayer et d'explorer ses propres solutions. Les membres les plus en avancés dans le CTF ont ainsi pu donner des indices en plus des indices donnés par l'organisation.

Sur un rythme pépère, j'y ai passé moins de 8h au total (incluant ce write-up).

## structure de ce write-up

Il est conseillé de lire ce write-up en commencant par la section [01-search](./01-search/).

La section [02-exploitation](./02-exploitation/) contient lui un répertoire par tentatives d'attaques / énumération effectués sur le système.

Phases appliquées:

- [01-search](./01-search)
- [02-exploitation](./02-exploitation)

Outils généraux utilisés:

- `linux` : pour ma part le challenge a été réalisé depuis une `Ubuntu 24.04` avec certains repositories de `Kali` ajouter comme source d'apt pour obtenir les outils adhoc.
- `direnv` : permets d'avoir une variable d'environnement RHOST contenant l'ip de la cible et évite d'avoir à la copier/coller tout le temps, ce qui est une plaie lors qu'on fait un challenge. Au fur et à mesure pour ajouter d'autres variables comme des mots de passe, des ports et ainsi scripter et automatiser les tentatives.
- `tmux` : multiplexeur de terminal, permets d'avoir x fenêtes en 1, très utile lorsqu'on lance un exploit d'un côté pour obtenir une session interactive de l'autre (_remote shell_).
- `vscode` : pour ce document qui sert à la fois de rapport et de notes tout au long du challenge.
- `git`: pour ne rien perdre des modifications / tentatives : sur un évènement long, lorsqu'on y consacre quelques dizaines de minutes tout les deux jours, rien de pire que de ne pas retrouver le mot de passe découvert 4 jours avant...

les outils propres aux exploitations seront détaillés dans les section kivonbien&copy;&reg;&trade;

## resources

- https://www.thehacker.recipes/
- http://www.pentest-standard.org/

pour ceux qui veulent vraiment faire un rapport de pentest (amusez-vous, moi faudra me payer pour ca):

- https://pentestreports.com/templates
- https://pentestreports.com/reports pour des vrais exemples caviardés.
- https://github.com/noraj/OSCP-Exam-Report-Template-Markdown (que je gratterai par contre si je passe une certif OCSP un jour).
