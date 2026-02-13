
PLD y = g^x  mod p
il est facile de x pour avoir y
il est difficile à partir de y pour connaitre x
y = g^x % p == > x = log de g y mod p

exemple : 3^9 % 11 = 4
3^99 % 11 = 4
Prenoms un nombre 5 
2q + 1 = 11

Groupe Cyclique d'ordre q
{e,A^1 , A^1, A^1,....................... , A^q-1 }
A^q = 1

2^0% 11 = 1
2^1% 11 = 2
2^2% 11 = 4
2^3% 11 = 8
2^4% 11 = 5

3^0% 11 = 1
3^1% 11 = 3
3^2% 11 = 9
3^3% 11 = 5
3^4% 11 = 4
3^5% 11 = 1
q = 5 ; p = 11 ; G5

G5{1, 3, 4, 5, 9}
g: <3>

# Le Problème du Logarithme Discret (PLD)

Le PLD est le fondement de nombreux protocoles de cryptographie (comme Diffie-Hellman ou l'Ecliptic Curve Cryptography).

## 1. Le Principe

Le calcul repose sur l'asymétrie de complexité :

- **Sens facile :** Calculer y=gx(modp) (Exponentiation modulaire).
    
- **Sens difficile :** Retrouver x à partir de y,g et p. C'est ce qu'on appelle calculer le logarithme discret :
    
    x=logg​(y)(modp)
    

---

## 2. Structure Mathématique : Groupe Cyclique

Pour que la sécurité soit assurée, on travaille souvent dans un **sous-groupe cyclique d'ordre q**.

- **Paramètres :**
    
    - p : un nombre premier (ex: p=11).
        
    - q : l'ordre du groupe (le nombre d'éléments).
        
    - g : le générateur du groupe.
        
- **Propriété :** Si g est un générateur d'un groupe d'ordre q, alors gq≡1(modp).
    

### Exemple avec p=11

On cherche un sous-groupe d'ordre q=5. On vérifie la relation p=2q+1 (où p=11 est un "premier sûr").

#### Test du générateur g=2 :

- 20≡1(mod11)
    
- 21≡2(mod11)
    
- 22≡4(mod11)
    
- 23≡8(mod11)
    
- 24≡5(mod11)
    
- 25≡10(mod11) ← (Ici l'ordre n'est pas 5, car on n'est pas revenu à 1).
    

#### Test du générateur g=3 :

- 30≡1(mod11)
    
- 31≡3(mod11)
    
- 32≡9(mod11)
    
- 33≡27≡5(mod11)
    
- 34≡15≡4(mod11)
    
- **35≡12≡1(mod11)** ← **L'ordre q est bien 5.**
    

---

## 3. Synthèse de l'Exemple

Le sous-groupe cyclique G5​ engendré par g=3 dans Z/11Z est :

> **G5​={1,3,4,5,9}**

- **Générateur (g)** : 3
    
- **Ordre (q)** : 5
    
- **Modulo (p)** : 11
    

Dans ce groupe, si je vous donne y=4, il est facile pour vous de vérifier que 34≡4(mod11), mais sur des nombres de 2048 bits, retrouver l'exposant est quasi-impossible.