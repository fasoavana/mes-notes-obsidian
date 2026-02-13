# Architecture 1-tiers  
Le lien se fait directement vers le serveur.

Dans ce contexte, les utilisateurs se connectent à l’application exécutée sur le serveur central (le mainframe) à l’aide de terminaux passifs qui se comportent comme des esclaves.

---

# Architecture 2-tiers  

Une architecture deux-tiers est composée de deux éléments : un client et un serveur.  
Le mot **tiers** ne fait pas référence à une entité physique mais à une entité logique.  

**Exemple :** Wave.

---

# Architecture 3-tiers  

L’architecture 3-tiers est composée de trois éléments (trois couches) :

1. La couche présentation (ou affichage), associée au client, qui est dite **légère** dans la mesure où elle n’assure aucune fonction de traitement, à la différence du modèle 2-tiers.  
   *(front-end)*  

2. La couche fonctionnelle, liée au serveur, qui est dans de nombreux cas un serveur web muni d’extensions applicatives.  
   *(back-end)*  

3. La couche de données, liée au serveur de base de données.  

---

L’architecture 3-tiers a été pensée pour pallier les limitations des architectures 2-tiers et pour concevoir des applications puissantes et simples à maintenir.
