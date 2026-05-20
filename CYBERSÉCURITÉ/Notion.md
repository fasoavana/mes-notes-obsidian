# Sécurité Informatique : Menaces, Vulnérabilités et Risques

**Menaces :** c'est un acteur ou un événement capable d'attaquer un système ex: hacker, employé, malware, catastrophe naturelle)

**Vulnérabilité :** est une faiblesse dans un système qui peut être exploitée par une menace ex : logiciel non à jour, os non à jour, mot de passe faible, mauvaise configuration du réseau

**Risques :** le risque correspond à la probabilité qu'une menace exploite une vulnérabilité et cause un dommage

$$R = M \times V \times I$$

---

## Matrice de risque
### Vulnérabilité / Risque (CMLC)

| Menace \ Vulnérabilité | Faible (C) | Moyenne (M) | Élevée (L) | Critique (C) |
| :--- | :--- | :--- | :--- | :--- |
| **Faible (C)** | Commun | Commun | Léger | Majeur |
| **Moyenne (M)** | Commun | Léger | Majeur | Critique |
| **Élevée (L)** | Léger | Majeur | Critique | Critique |
| **Critique (C)** | Majeur | Critique | Critique | Critique |

## Exercices d'application : Menace, Vulnérabilité, Risque

### Exemple 1 : Le mot de passe faible
*   **Menace :** Hacker, employé
*   **Vulnérabilité :** Mot de passe faible
*   **Risque :** Accès non autorisé

---

### Exemple 2 : Le serveur exposé et obsolète
> **Situation :** Une entreprise utilise un serveur accessible depuis internet sans mise à jour depuis 2 ans.

*   **Menace :** Hacker (cybercriminel, robot d'analyse automatisé)
*   **Vulnérabilité :** Serveur non mis à jour depuis 2 ans (failles de sécurité publiques non corrigées) et accessible directement depuis internet
*   **Risque :** Fuite d  =é (ou piratage complet du serveur, ransomware)

p
0 
### Exemple 3 : L'administrateur et les mots de passe identiques
> **Situation :** Un admin utilise le même mot de passe sur plusieurs systèmes critiques.

*   **Menace :** Employé (l'administrateur lui-même par négligence, ou un hacker qui récupère ce mot de passe)
*   **Vulnérabilité :** Réutilisation de mot de passe sur plusieurs systèmes critiques
*   **Risque :** Accès à plusieurs systèmes (compromission en chaîne / effet domino) et fuite de données