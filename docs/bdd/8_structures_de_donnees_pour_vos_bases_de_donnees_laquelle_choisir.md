# 8 STRUCTURES DE DONNÉES POUR VOS BASES DE DONNÉES. LAQUELLE CHOISIR ?

La réponse variera en fonction de votre cas d'utilisation. Les données peuvent être indexées en mémoire ou sur disque. De même, les formats de données varient : nombres, chaînes de caractères, coordonnées géographiques, etc. Le système peut être lourd en écriture ou en lecture. Tous ces facteurs influencent le choix du format d'indexation de la base de données.

![8_data_structures_that_power_your_databases.jpg](../_media/bdd/8_data_structures_that_power_your_databases.jpg ':size=700')

Voici quelques-unes des structures de données les plus utilisées pour l'indexation des données :

- **Skiplist**: un type d'index en mémoire courant (in-memory). Utilisé dans Redis
- **Hash index**: une implémentation très courante de la structure de données "**Map**" (ou "**Collection**")
- **SSTable**: implémentation immuable de "**Map**" sur disque
- **LSM tree**: **Skiplist** + **SSTable**. Haut débit d'écriture
- **B-tree**: sur disque. Performances constantes en lecture/écriture
- **Inverted index**: utilisé pour l'indexation des documents. Utilisé dans Lucene
- **Suffix tree**: pour la recherche de motifs de chaînes de caractères
- **R-tree**: la recherche multidimensionnelle, telle que la recherche du voisin le plus proche

---

_Source: https://blog.bytebytego.com/i/99358794/data-structures-that-power-your-databases-which-one-should-we-pick_
