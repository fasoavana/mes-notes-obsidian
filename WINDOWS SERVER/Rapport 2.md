
# Rapport de Mise en Œuvre : Infrastructure Active Directory et GPO

## I. Préparation de l'Environnement Réseau

On commence par stabiliser les adresses IP pour que le dialogue entre les machines soit permanent.

1. **Configuration du Serveur (SRVADVAR01)** :
    
    - On fixe l'adresse IP sur `192.168.56.102` avec un masque en `255.255.255.0`.
        
    - On pointe le DNS sur lui-même via l'adresse `127.0.0.1`.
    - 
![[Pasted image 20260210111055.png]]


2. **Configuration du Client (WIN01)** :
    
    - On s'assure que le DNS préféré du client est bien l'IP du serveur : `192.168.56.102`.
        
    ![[Pasted image 20260210111550.png]]
---

## II. Déploiement du Contrôleur de Domaine

On transforme le serveur en pilier central du réseau via AD DS.

3. **Installation des Rôles** :
    
    - On ajoute les rôles **AD DS** et **DNS** via le Gestionnaire de serveur.
        
    ![[Pasted image 20260210112211.png]]
        
4. **Promotion en Forêt** :
    
    - On crée une nouvelle forêt nommée `exemple.youtube`.
        
![[Pasted image 20260210112052.png]]

---

## III. Gestion des Utilisateurs et des Dossiers

On prépare l'arborescence pour l'utilisateur qui testera la redirection.

5. **Création de l'Unité d'Organisation** :
    
    - On crée une OU nommée `Direction` dans l'annuaire.
        
![[Pasted image 20260210113423.png]]
        
6. **Ajout de l'utilisateur Test** :
    
    - On crée l'utilisateur `test` (test) avec le mot de passe **Exemple00+**.
        
    ![[Pasted image 20260210113745.png]]
        
7. **Création du dossier de stockage** :
    
    - Sur le disque `C:` du serveur, on crée le dossier `UsersRedirects`.
    ![[Pasted image 20260210114839.png]]
    
        
    - On le partage en donnant le **Contrôle Total** au groupe `Everyone` dans les paramètres de partage avancé.
        
    ![[Pasted image 20260210115623.png]]
        

---

## IV. Configuration de la GPO de Redirection

C'est l'étape logicielle qui lie les dossiers locaux au partage réseau.

8. **Création de la Stratégie** :
    
    - On crée une nouvelle GPO appelée `Redirection_Documents` liée à l'OU `Direction`.
        
![[Pasted image 20260210120507.png]]
        
9. **Édition des paramètres** :
    
    - On se rend dans `Configuration utilisateur > Stratégies > Paramètres Windows > Redirection de dossiers`.
        
    - On configure la redirection des **Documents** vers `\\SRVADVAR01\UsersRedirects`.
    ![[Pasted image 20260210121746.png]]        
8. **Sécurité Administrative** :
    
    - On décoche l'option "Accorder à l'utilisateur des droits exclusifs sur Documents" pour que l'administrateur puisse vérifier le contenu.
        
![[Pasted image 20260210121856.png]]

---

## V. Phase de Test et Validation Physique

On vérifie ici que les fichiers "voyagent" bien du client vers le serveur.

11. **Jointure et Session** :
    
    - On joint le PC client au domaine `exemple.youtube` et on redémarre.
    ![[Pasted image 20260212144827.png]]
        
    - On se connecte avec le compte `test`.
        
12. **Création du Fichier Témoin (Action Critique)** :
    
    - Sur le poste client, on ouvre le dossier **Documents**.
        
    - On crée un nouveau fichier texte nommé `test_redirection_jean.txt`.
     ![[Pasted image 20260212150644.png]]
    - On écrit à l'intérieur : "Ceci est un test de redirection serveur" et on enregistre.
        ![[Pasted image 20260212150910.png]]
        
13. **Vérification sur le Serveur** :
    
    - On se rend physiquement sur le serveur dans `C:\UsersRedirects\test\Documents`.
        ![[Pasted image 20260212151216.png]]    - On vérifie que le fichier `test_redirection_test.txt` s'y trouve bien sans y avoir touché manuellement.
        
 
       ![[Pasted image 20260212151350.png]]
---

