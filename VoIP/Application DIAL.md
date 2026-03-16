# Guide Complet : L'Application Dial dans Asterisk

## 1. Fondamentaux et Emplacement

L'application **Dial** est le moteur principal qui permet de connecter un appelant à une destination, qu'elle soit interne (un autre poste) ou externe (vers un opérateur). Toutes ces configurations s'effectuent dans le fichier de configuration suivant :

- **Fichier :** `/etc/asterisk/extensions.conf`.
    
- **Rôle :** Définir le **Dialplan** (plan de numérotation), c'est-à-dire l'intelligence de routage des appels.
    
---

## 2. Syntaxe Universelle

Chaque ligne de commande doit suivre une structure stricte pour être interprétée correctement par le serveur Asterisk:


`exten => NUMÉRO, Priorité, Dial(TECHNOLOGIE/destination, DURÉE, OPTIONS)`


- **NUMÉRO** : Le numéro ou le modèle de numéro (pattern) composé par l'utilisateur.
    
    - **Priorité** : L'ordre d'exécution de la commande (généralement `1` pour la première action).
    
    
- **TECHNOLOGIE** : Le protocole de communication utilisé (ex: `SIP`, `PJSIP`, `IAX2`).
    
- **DURÉE** : Le temps maximal de sonnerie en secondes avant que l'appel ne soit considéré comme un échec (ex: `30` secondes).
    

---

## 3. Gestion des Appels Sortants (Sortie vers FAI)

Pour passer des appels vers l'extérieur via un fournisseur d'accès (ex: **FAIOVH**), on utilise souvent des **préfixes** pour distinguer les appels internes des appels externes.



### A. Utilisation des Préfixes

Si un préfixe est utilisé pour sortir (ex: `987654321`), il doit être supprimé avant que le numéro ne soit envoyé à l'opérateur.



- **Outil :** Le symbole `:` (deux points) est utilisé pour tronquer le numéro.
    
   
    
- **Exemple :** `${EXTEN:9}` permet de supprimer les 9 premiers chiffres du numéro composé (le préfixe).
    
    
    

### B. Formats acceptés par le FAI

Selon les exigences techniques de l'opérateur, le numéro doit être formaté spécifiquement:


- **Format International :** `0033XXXXXXXXX` ou `+33XXXXXXXXX`.
    

    
- **Format National :** `0XXXXXXXXX`.
    
    
    

### C. Exemples de configurations corrigées

- **Appel avec préfixe long :**
    
    `exten => _987654321.,1,Dial(SIP/FAIOVH/${EXTEN:9},30)`.
    
    
    
- **Appel de campagne (Conversion du 0 en +33) :**
    
    `exten => _0XXXXXXXXX,1,Dial(SIP/FAIOVH/+33${EXTEN:1},30)`.
    
    
    

---

## 4. Gestion des Appels Entrants

Lorsqu'un appel arrive de l'extérieur sur l'un de vos numéros publics, vous devez le diriger vers un terminal ou un poste spécifique.

- **Exemple pour le numéro 0345011102 vers le poste interne 2026 :**
    
    `exten => 0345011102,1,Dial(SIP/2026,30)`.
    

---

## 5. Résumé des Symboles Clés

| Symbole        | Signification                                                                         |
| -------------- | ------------------------------------------------------------------------------------- |
| **`_`**        | Indique que le numéro est un modèle (pattern) et non un numéro fixe.<br><br>          |
| **`.`**        | Remplace n'importe quelle suite de chiffres (joker de fin).<br><br>                   |
| **`${EXTEN}`** | Variable système contenant le numéro exact composé par l'utilisateur.<br>             |
| **`:`**        | Utilisé à l'intérieur de la variable `${EXTEN}` pour tronquer (couper) le numéro.<br> |
