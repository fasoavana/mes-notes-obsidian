C'est **Cryptologie** : cryptos + logos

**cryptos** : cryptographie + cryptanalyse
logos : etude

**cryptographie** : cryptos + graphem (étude de l'écriture caché)
**graphem** : écriture
**cryptos** : cacher (sécurité)
**cryptoanalyse** : cryptographie + analyse
**Entropie = Somme Pilog2(1/Pi)** inventé par Claude Shannon (Théorie de l'information)
Pourquoi analyser : pour s'assurer que la cryptographie fait son écriture caché pour la raise de sécurité.

Qui va faire la cryptographie(manangana) : Personne autorisée
et qui va faire la cryptanalyse(mpandromba) : Personne non autorisé

Cryptologie = Cryptographie + cryptanalyse


#### Chiffrer (opération de chifrement)
Le fait de transformer un message claire en message chiffré(cryptogramme) à partir d'une clé.
#### Déchiffrer (pour des personne autorisée) / Décrypter (déchiffrer une message pour le personne non autorisée)


**AES(Advanced Encryption Standart)**

on utlise la meme clé pour le chiffrement et le déchiffrement
le temps d'execution est presque le même

Concet de chiffrement Asymetrique



-  Communication
- Confiance
- Seule le destinataire peut le dechiffrer
- Clé public pour chiffrer(gadana)
- Clé privée pour dechiffer(la clé)
- Il faut partir de clé privée pour créer de clé public (Keygen), généralement ce création est en randoom

1. Seul Alice peut signer mais tout le monde peut verifié en demandant le clé public(Authentification) 2+1
2. Seule Alice peut dechiffer mais tout le monde peut chiffrer à partir de la clé public(Confidentialité) 1+2

Diffi et Elman créer de protocole de  partage de clé (DiffiElman) 

**Pourquoi le chiffrement Asymetriquue est securisé**

# RSA

#RSA : Rivest Shamir Adleman


# RSA — Petite leçon (prête à copier-coller)

## Objectif

Comprendre la génération des clés RSA et le chiffrement/déchiffrement.

---

## 1. Génération des clés

### 1.1 Nombres premiers

```text
Choisir p et q (grands nombres premiers)
```

### 1.2 Calcul de n

```text
n = p × q
```

### 1.3 Fonction d’Euler

```text
φ(n) = (p − 1)(q − 1)
```

---

## 2. Exposant public e

```text
Choisir e tel que :
1 < e < φ(n)
PGCD(e, φ(n)) = 1
```

Note : en pratique, e = 65537.

---

## 3. Exposant privé d

```text
d ≡ e⁻¹ mod φ(n)
```

- d est l’inverse modulaire de e
    
- d sert au déchiffrement
    

---

## 4. Clés RSA

```text
Clé publique : (e, n)
Clé privée  : (d, n)
```

---

## 5. Chiffrement

```text
Message m avec m < n

c = m^e mod n
```

---

## 6. Déchiffrement

```text
m = c^d mod n
```

---

## Résumé

```text
p, q premiers
n = p × q
φ(n) = (p − 1)(q − 1)

e choisi tel que PGCD(e, φ(n)) = 1
d ≡ e⁻¹ mod φ(n)

Chiffrement   : c = m^e mod n
Déchiffrement : m = c^d mod n
```

---

## Remarque

En pratique, RSA chiffre une clé symétrique (ex. AES), pas directement le message.