**Wi-Fi Profiles**: Push enterprise Wi-Fi settings.

1. # qu'est-ce qu'une stratégie de groupe ou GPO ?
2. GPO  Active Directory VS GPO Local ?
3. Quel est l'ordre d'application des GPO sur un PC  ?



# Qu'est ce qu'un stratégie de groupe ou GPO ?

(Groupe Policy Object)

c'est un ensemble d'outlis integrere a windows qui permet au administeur systeme de centraliser la gestion de l'environnement utlisateur et la configuration  des machines.

**Objectif** : Centraliser la gestion de l'environnement utlisateur et la configuration des machine(postes de travail et serveurs).

Exemple :
- Bloquer l'invite de commande
- Définir un fond d'ecran 
- Déployer un logiciel
- Connecter un lecteur réseau
- Ajouter des favoris dans le navigateur
- Préconfigurer la suite Office 
- Connecter  une imprimante
- Désactiver la télémétrie de Windows 10
- Copier un fichier sur les PC

## GPO  Active Directory VS GPO Local ?

A l'aide d'une console unique, on peut gérere différentes stratégies de groupe(GPO) à appliqer sur nos machines et notre utilisateurs.

## Quel est l'ordre d'application des GPO sur un PC  ?

Local, Site, Domain, Organizational Unit

Les GPO peuvent s'appliquer à différents endroits :

-   En local, c'est à dire la stratégiee de groupe locale
-  Stratégie de groupe au niveau du site 
-  Stratégie de groupe au niveau du domaine
-  Strategie de groupe au niveau d'une unirté d'organisation

LSDOU
Local - Site - domaine - Unité d'Organisation 