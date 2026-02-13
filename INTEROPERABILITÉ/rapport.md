# 📑 Rapport de Projet : Système de Calcul Distribué (RMI & Docker)

**Équipe de développement :**
- **Fahasoavana**
- **Volasoa**

**Matière :** Interopérabilité (Licence 3)

**Tags :** #RMI #Docker #SystèmesDistribués #Java #L3

---

## 🏗️ 1. Architecture du Système
L'application repose sur une architecture client-serveur distribuée. Contrairement à une exécution locale, chaque composant est isolé dans son propre conteneur Docker.



> [!INFO] Composants réseau
> - **Réseau** : Bridge Docker nommé `rmi-net`.
> - **Serveur** : Service `srv-calcul` (IP dynamique, DNS interne).
> - **Client** : Service `cli-calcul` (interactif).

![[Pasted image 20260203193145.png]]

---

## 🛠️ 2. Défis Techniques & Expertise
La conteneurisation de RMI présente des défis majeurs que notre équipe a résolus :

### 3.1. Le problème des ports dynamiques
RMI utilise un port aléatoire pour la communication des données. Dans un container, cela provoque un échec de connexion.
**Solution de l'équipe :** Nous avons forcé l'objet distant à utiliser le port **1099** (le même que le registre) via `super(1099)` dans le constructeur.

### 3.2. Résolution du Hostname
**Solution de l'équipe :** Utilisation de la propriété système `java.rmi.server.hostname` pour forcer le serveur à s'identifier par son nom DNS Docker `srv-calcul`.

---

## 💻 3. Analyse de l'Implémentation Client (Valeur Ajoutée)

### 3.1. Analyseur d'expressions complexes
Nous avons implémenté un traitement via `StringTokenizer` permettant de dépasser la limitation des calculs simples.

> [!SUCCESS] Point Fort
> Notre client peut traiter des chaînes comme : `10 + 5 * 2 / 4`.
> Le client décompose l'expression et sollicite le serveur itérativement pour chaque opération.

![[Pasted image 20260203193551.png]]

---

## 🚀 4. Démonstration des Résultats

### 4.1. Calcul complexe et historique
Le client affiche en temps réel l'avancement du calcul et l'historique des opérations traitées par le serveur distant.
![[Pasted image 20260203193708.png]]
### 4.2. Robustesse et gestion d'erreurs
Le système est capable de détecter et de remonter les erreurs mathématiques (division par zéro) sans interrompre le service.
![[Pasted image 20260203194114 1.png]]
---

### Présentation Technique du Code

## 4. Analyse Détaillée des Composants Logiciels

Notre solution est structurée autour de quatre fichiers Java clés, chacun jouant un rôle précis dans le cycle de vie de l'appel distant.

### 4.1. L'Interface `Calculatrice.java` (Le Contrat)

C'est la pièce maîtresse de l'interopérabilité.

- **Rôle** : Elle définit les méthodes que le serveur s'engage à fournir.
    
- **Particularité** : Elle hérite de `java.rmi.Remote`. Chaque méthode doit lever une `RemoteException`, car dans un système distribué, le réseau est considéré comme "non fiable".

![[Pasted image 20260205052005.png]]
### 4.2. L'Implémentation `CalculatriceImpl.java` (La Logique)

C'est ici que réside l'intelligence de calcul.

- **Héritage** : Elle étend `UnicastRemoteObject` pour être exportable sur le réseau.
    
- **Innovation technique** : Nous avons utilisé le constructeur `super(1099)`. C'est ce choix précis qui permet de fixer le port de communication des données et de traverser le pont réseau de Docker.
![[Pasted image 20260205051950.png]]

### 4.3. Le `ServeurRMI.java` (L'Hébergeur)

Ce fichier initialise l'environnement serveur.

- **Registre** : Il crée le registre RMI (`LocateRegistry.createRegistry(1099)`).
    
- **DNS Interne** : Il configure la propriété `java.rmi.server.hostname` sur `srv-calcul`. Sans cette ligne, le client Docker ne pourrait jamais localiser l'adresse IP interne du serveur.

![[Pasted image 20260205052026.png]]
### 4.4. Le `ClientRMI.java` (L'Analyseur et l'UI)

C'est la partie la plus interactive du projet.

- **Lookup** : Il utilise `Naming.lookup("rmi://srv-calcul:1099/...")` pour obtenir une référence (Stub) vers l'objet distant.
    
- **Parsing Avancé** : L'utilisation de `StringTokenizer` permet au client de décomposer une chaîne complexe (ex: `10+20-5`) en une suite d'appels distants.
    
- **Gestion d'État** : Le client maintient un historique local des résultats renvoyés par le serveur pour permettre le calcul en chaîne.
![[Pasted image 20260205052051.png]]

---

## 5. Analyse du Flux d'Exécution (Diagramme de Séquence)

Voici ce qui se passe techniquement lors d'une saisie utilisateur :

1. **Saisie** : L'utilisateur entre `78 + 1651`.
    
2. **Analyse** : Le `StringTokenizer` extrait `78`, `+` et `1651`.
    
3. **Appel Distant** : Le client appelle `stub.calculer(78, 1651, "+")`.
    
4. **Sérialisation** : Java transforme les données en octets, les envoie au conteneur `srv-calcul`.
    
5. **Désérialisation & Calcul** : Le serveur effectue l'opération et renvoie le résultat.
    
6. **Affichage** : Le client reçoit la réponse et met à jour l'interface colorée.
## 🏁 5. Conclusion
Ce projet réalisé par **Fahasoavana et Volasoa** démontre une maîtrise complète de l'interopérabilité Java. L'utilisation de Docker garantit une portabilité totale de la solution, tandis que l'optimisation RMI assure une communication fluide et robuste entre les services.

![[Pasted image 20260203194605.png]]
---
