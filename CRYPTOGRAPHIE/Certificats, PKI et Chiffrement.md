## I. Le Chiffrement Symétrique (La Base)

Historiquement le premier, il repose sur une clé unique partagée.

- **Sécurité :** Tout repose sur le **Partage de clé**.
    
- **Problème :** Risque élevé de **corruption/interception** lors du partage de la clé.
    
- **Confiance :** Ratio **50/50** (Chaque partie est responsable du secret).
    

---

## II. L'Échange de Clés : Protocole Diffie-Hellman

Pour résoudre le problème du partage de clé, on utilise les mathématiques du logarithme.

- **DLP (Discrete Logarithm Problem) :** À la base, c'est un problème de logarithme classique.
    
- **Le passage au "Discret" :** Si on ajoute l'opération **Modulo** (n(modp)), le chiffrement devient "discret" (les valeurs sautent de manière imprévisible).
    
- **Utilité :** Permet de créer une clé secrète commune sur un réseau public.
    

---

## III. Le Chiffrement Asymétrique (L'Évolution)

Utilise une clé publique et une clé privée pour sécuriser les échanges sans partage préalable.

- **Sécurité :** Basée sur un problème à **sens unique** (facile à faire, dur à inverser).
    
- **Problèmes identifiés :**
    
    - **Temps élevé :** Calculs lourds pour le processeur.
        
    - **Attaque MITM (Man In The Middle) :** Un pirate peut intercepter et remplacer la clé publique.
        
- **Confiance :** Repose sur un **tiers** (Autorité de Certification).
    
- **Algorithme RSA :** Basé sur le **Problème de Factorisation (F.P)** : n=p×q. Retrouver p et q à partir de n est mathématiquement complexe.
    

---

## IV. La Certification & PKI

**Pourquoi ?** On a créé le certificat pour vérifier l'**authenticité de la clé publique**.

- **PKI (Public Key Infrastructure) :** L'ensemble du système qui garantit que la clé appartient à la bonne personne.
    
- **Usage :** Il faut d'abord s'authentifier au système via le certificat avant d'échanger des données.
    

---

## V. Le Chiffrement Hybride (La Pratique)

C'est la combinaison des deux mondes pour être à la fois **sûr** et **rapide**.

1. **Authentification :** Utilisation du certificat.
    
2. **Échange :** Alice et Bob calculent leurs clés.
    
    - **Alice :** Stocke a (secret) sur sa machine et envoie ga sur le serveur.
        
    - **Bob :** Stocke b (secret) sur sa machine et envoie gb sur le serveur.
        
3. **Session :** Ils génèrent une clé symétrique pour la suite.
    

---

## VI. Courbes Elliptiques (ECC) : Le Futur

Plus robuste et plus léger que RSA.

- **Nature :** La courbe peut être **discrète** ou continue.
    
- **Discret :** Devient aléatoire en ajoutant le **modulo**.
    
- **Formule de validation :** 4a3+27b2=0 (pour éviter les courbes invalides).
    
- **Avantages :** - Algèbre géométrique très complexe à casser.
    
    - Robuste face aux **attaques physiques** (Side-channel).
        
    - Possibilité de créer un nombre aléatoire à partir d'un **Seed** (graine).
        

### Protocole ECDH (Elliptic Curve Diffie-Hellman)

Alice et Bob utilisent un point générateur commun G(Gx​,Gy​) sur la courbe :

- **Alice :** Calcule aG (Clé publique).
    
- **Bob :** Calcule bG (Clé publique).
    
- **Échange :** Ils s'échangent les résultats.
    
- **Calcul Final :** Alice fait a(bG) et Bob fait b(aG). Ils obtiennent le même point secret.
    

---

**Tags :** #Crypto #PKI #RSA #ECC #DiffieHellman

> [!NOTE] Rappel a et b sont toujours les **clés secrètes** (scalaires), tandis que aG et bG sont les **clés publiques** (points sur la courbe).