---
description: Installation d'un certificat auto-signé (https) OpenSSL avec nginx
author:
  name: Anthony Javelier
  email: pro@contactit.fr
  link: https://contactit.fr
  avatar: https://docs.contactit.fr/images/avatar.webp
  
categories: [GNU/Linux]
date: 2021-10-22
tags: [GNU/Linux, debian, openssl, ssl, tls, chiffrement, https]
visibility: public
order: 46
---

# :key: Installation d'un certificat auto-signé OpenSSL avec nginx



!!! Warning Cette documentation est en cours d'écriture :writing_hand:
Il pourrait il y a avoir quelques erreures.  
Si vous en remarquez une, contactez-moi [ici](mailto:pro@contactit.fr) :slightly_smiling_face:
!!!

==- :icon-unverified: Un problème avec sudo ?

!!!
Si vous configurez votre serveur directement en root. N'oubliez pas de retirer le `sudo` de chaque commande.
!!!

!!! danger
:arrows_counterclockwise: Si vous avez mis un mot de passe au compte root, la commande `sudo` ne sera pas acceptée.
Connectez-vous directement en root pour exécuter les commandes.  
:arrow_right: Vous pouvez aussi, réinstaller votre système en laissant le mot de passe root vide, lors de l'installation.  
`sudo` s'installera et fonctionnera correctement.
!!!

==-

---

## :closed_lock_with_key: Générer le certificat et la clé

```
sudo openssl req -x509 -days 365 -out client0.crt -nodes -newkey rsa:2048 -keyout client0.key
```

:icon-shield-lock: Renseigner les différentes informations du certificat

```
Generating a RSA private key
..................................+++++
........................................................+++++
writing new private key to 'client0.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:FR
State or Province Name (full name) [Some-State]:RHONE
Locality Name (eg, city) []:Lyon
Organization Name (eg, company) [Internet Widgits Pty Ltd]:CONTACTIT
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:web.it.fr
Email Address []:pro@contactit.fr
```

!!! danger
`Common Name` doit être renseigné le nom de domaine !
!!!

!!!
OpenSSL a généré le certificat et la clé dans le répertoire ou vous êtes au moment ou vous avez tapper la commande.
Vous pouvez les déplacer/renommer comme bon vous semble.
!!!


---

## :memo: Éditer le fichier de configuration nginx

```
sudo nano /etc/nginx/sites-enabled/default
```

:icon-file-symlink-file: Ajouter la configuration SSL à la fin du fichier

``` # /etc/nginx/sites-enabled/default
server {
        listen 443 ssl;
        server_name _;
        root /var/www;
        ssl_certificate /var/www/sites/client1/client0.crt;
        ssl_certificate_key /var/www/sites/client1/client0.key;
        index index.html;
#        location / {
#                proxy_pass http://172.17.0.2;
#        }
}
```

:icon-chevron-right: `Ligne 5-6` Remplacer les chemins par les votres.

!!! success
Vous pouvez maintenant consulter votre site web de manière chifrée (https://monsite.local par exemple).
!!!




