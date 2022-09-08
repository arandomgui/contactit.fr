---
description: Installation basique de debian
authors:
- name: Olivier Pouzadoux
  email: contact@leshirondellesdunet.com
  link: https://leshirondellesdunet.com
  avatar: https://www.leshirondellesdunet.com/img/headlogo.png
categories: [GNU/Linux]
date: 2021-11-03
tags: [GNU/Linux, debian, chmod, chown, droits, permissions]
visibility: public
order: 30
---

# La gestion des droits sous GNU/Linux

:icon-list-unordered: Un système d'exploitation GNU/Linux a trois types d'utilisateurs et trois type de droits distincts.
Cette page explique et présente les options les plus souvent utilisées pour des scripts et plus précisément la gestion de dossiers et fichiers de sites web sur un serveur Apache sous Linux.

:icon-plus-circle: Pour un complément d'information vous pouvez consulter les documentations d'ubuntu-fr.org:  
- [les permissions](http://doc.ubuntu-fr.org/permissions)
- [Les répertoires de travail d'un serveur lamp](http://doc.ubuntu-fr.org/tutoriel/lamp_repertoires_de_travail)

---

## :round_pushpin: La commande CHmod

:icon-people: Les types d'utilisateurs:  
- Le propriétaire du fichier (user)
- Le groupe du propriétaire du fichier (group)
- Les autres utilisateurs, ou encore le reste du monde (others)

:icon-repo-clone: Les types de droits:
- r : droit de lecture (read)
- w : droit d'écriture (write)
- x : droit d'exécution (eXecute)

---

## :abacus: Correspondances des droits en binaire/octale et leurs significations

Position Binaire | Valeur octale | Droits | Signification
--- | --- | --- | ---
000 | 0 | - - - | Aucun droit
001 | 1 | - -x | Exécutable
010 | 2 | - w - | Ecriture
011 | 3 | - w x | Ecrire et exécuter
100 | 4 | r - - | Lire
101 | 5 | r - x | Lire et exécuter
110 | 6 | r w - | Lire et écrire
111 | 7 | r w x | Lire écrire et exécuter

Ainsi pour modifier les droits de façon octale, la meilleure façon pour être certain du résultat, est d'additionner ceux-ci.

Par exemple : changer les droits du fichier "monscript" pour que je sois (moi le propriétaire) le seul à pouvoir le modifier, que les personnes de mon groupe puissent le lire comme l'exécuter et que le reste du monde puisse uniquement l'exécuter :

Type d'utilisateurs | Propriétaire | Groupe | Les autres
---  | --- | --- | ---
Droits | r w x | r - x | - - x
Position Binaire | 111 | 101 | 001
Valeur Octale | 7 | 5 | 1

Modifier les droits sur `monscript` :

```
sudo chmod 751 monscript
```

---

## :books: Récapitulatif des différentes valeurs possibles fréquemment utilisées

!!!
644 - Lecture, écriture pour le propriétaire / Lecture pour les autres.  
Valeur par défaut d'un fichier sous GNU/Linux.  
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w - | r - - | r - -
Position Binaire | 110 | 100 | 100
Valeur Octale | 6 | 4 | 4

!!!warning
666 - Lecture, écriture pour tout le monde, déconseillé  
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w - | r w - | r w -
Position Binaire | 110 | 110 | 110
Valeur Octale | 6 | 6 | 6

!!!
700 - Lecture, écriture, execution juste pour le propriétaire  
Valeur par défaut d'un dossier sous GNU/Linux  
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | - - - | - - -
Position Binaire | 111 | 000 | 000
Valeur Octale | 7 | 0 | 0

!!!info
705 - Le propriétaire à tous les droits / Le groupe aucun / Les autres lire et executer  
Préconisé par certains providers pour le répertoire du site, si celui-ci est inaccessible (message : "Forbidden...")  
!!! 

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | - - - | r - x
Position Binaire | 111 | 000 | 101
Valeur Octale | 7 | 0 | 5

!!!light
755 - Le propriétaire à tous les droits / Les autres lire et executer  
Utile pour des scripts par exemple et certains fichiers d'un site web
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | r - x | r - x
Position Binaire | 111 | 101 | 101
Valeur Octale | 7 | 5 | 5

!!!light
764 - Tous droits pour le propriétaire / Lecture, écriture pour le groupe / Lecture seule pour les autres  
Parfois utile pour des fichiers d'un site web appartenant au groupe www-data
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | r w - | r - -
Position Binaire | 111 | 110 | 100
Valeur Octale | 7 | 6 | 4

!!!light
774 - Tous les droits pour le propriétaire et le groupe / Lecture seule pour les autres  
Utile pour certains fichiers d'un site sur un serveur local de développement
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | r w x | r - -
Position Binaire | 111 | 111 | 100
Valeur Octale | 7 | 7 | 4

!!!light
775 - Tous les droits pour le propriétaire et le groupe / Lecture et exécution pour les autres  
Très pratique pour se simplifier la vie avec la gestion d'un site en développement sur un serveur local (dans le dossier media)
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | r w x | r - x
Position Binaire | 111 | 111 | 101
Valeur Octale | 7 | 7 | 5

!!!danger
777 - Tous les droits pour tous  
Fortement déconseillé ! Mais peut-être nécessaire pour le cache de CMS en local (par exemple) sur un serveur Lamp
!!!

Type d'utilisateurs | Propriétaire | Groupe | Les autres
--- | --- | --- | ---
Droits | r w x | r w x | r w x
Position Binaire | 111 | 111 | 111
Valeur Octale | 7 | 7 | 7

Pour modifier les droits d'un répertoire et de ses sous répertoires, utilisez la fonction récursive -R  
Modifier le répertoire /var/www/html/monsite avec les droits modifiés en 755:

```
sudo chmod -R 755 /var/www/html/monsite
```

---

## :clipboard: Afficher les droits d'un répertoire

:icon-triangle-right: Les droits des fichiers d'un dossier peuvent être affichés par la commande "ls -l"  

:icon-eye: Afficher les droits des fichiers du dossier /var/www/html/monsite:

```
ls -l /var/www/html/monsite
```

---

## :bookmark_tabs: Format des droits

Le format des droits d'accès est une liste de 10 symboles.  

Le 1er symbole est soit un "-" soit un "l" soit un "d", ils indiquent s'il s'agit:  
:icon-triangle-right: d'un fichier (-)  
:icon-triangle-right: d'un lien (l)  
:icon-triangle-right: ou d'un dossier (d).  

  Ensuite suivent les trois groupes des trois symboles des permissions (rwx-). Par exemple:  
`-rw-r--r--`	signifie que c'est un fichier dont les droits sont définis 644  
`drwx------`	signifie que c'est un dossier dont les droits sont définis en 700  
`lrwxrwxrwx`	signifie que c'est un lien dont les droits sont définis en 777  

---

## :heavy_check_mark: L'avantage du CHown

:icon-shield: Pour éviter des chmod 666 (ou pire encore des 777), CHown permet de modifier le propriétaire d'un fichier.  

:icon-triangle-right: Si vous souhaitez vous réaproprier les droits sur l'ensemble d'un dossier et que vous êtes l'utilisateur "martin":

```
sudo chown -R martin /chemin/vers/dossier
```

:icon-triangle-right: Récupérer les droits sur l'ensemble du dossier:

```
sudo chmod -R 755 /chemin/vers/dossier
```
