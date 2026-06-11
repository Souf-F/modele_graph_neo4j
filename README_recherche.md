# Bases de données graphe — Recherches & comparaison avec les modèles relationnels

**Auteur :** Soufiane Filali  
**Date :** Juin 2026  
**Contexte :** Holberton School France — Trimestre 2

---

## 1. C'est quoi une base de données graphe ?

Une base de données graphe stocke les données sous forme de **nœuds** (les entités) et de **relations** (les liens entre ces entités). Chaque nœud et chaque relation peut porter des **propriétés** sous forme de paires clé/valeur.

Ce modèle est directement inspiré de la théorie des graphes en mathématiques. Il a été conçu pour répondre à une problématique précise : **modéliser et interroger des données fortement connectées** là où les bases relationnelles montrent leurs limites.

Les trois éléments fondamentaux d'une base graphe :

- **Nœud** — une entité (un utilisateur, un film, un genre)
- **Relation** — un lien typé et directionnel entre deux nœuds (`:A_VU`, `:SUIT`)
- **Propriété** — une information attachée à un nœud ou une relation (`name: 'Alice'`, `note: 9`)

---

## 2. Pourquoi ce modèle existe-t-il ?

Les bases relationnelles comme MySQL organisent les données en **tableaux**. Interroger des données connectées sur plusieurs niveaux implique des **jointures** — et plus les relations sont profondes, plus les jointures deviennent coûteuses en performance.

Exemple concret : sur une plateforme de streaming, répondre à *"quels films ont regardé les amis des amis d'Alice qui appartiennent au même genre qu'un nouveau film ?"* nécessite 4 jointures en SQL. En Cypher (le langage de Neo4j), c'est une seule requête qui suit les flèches :

```cypher
MATCH (new:Movie {title:'Dune 2'})
      -[:IN_GENRE]->(g)<-[:IN_GENRE]-(other)
      <-[:WATCHED]-(user)
RETURN DISTINCT user.name
```

Neo4j a été créé en 2007 par Emil Eifrem précisément pour résoudre ce problème : **rendre les connexions entre données aussi simples à interroger qu'à dessiner**.

---

## 3. Comparaison : MySQL vs Neo4j

| Critère | MySQL (relationnel) | Neo4j (graphe) |
|---|---|---|
| Structure | Tables, lignes, colonnes | Nœuds, relations, propriétés |
| Langage | SQL | Cypher |
| Jointures | Nécessaires et coûteuses | Remplacées par la traversée |
| Point fort | Données tabulaires structurées | Données fortement connectées |
| Cas d'usage | E-commerce, CRM, ERP | Réseau social, fraude, recommandation |
| Performance sur relations | Diminue avec la profondeur | Constante quelle que soit la taille |
| Schéma | Fixe | Flexible |

La différence fondamentale : dans MySQL la **jointure est calculée** à chaque requête en scannant les tables. Dans Neo4j chaque nœud stocke un **pointeur direct** vers ses relations voisines — la traversée suit ces pointeurs sans scanner la base entière.

---

## 4. POC réalisé

Implémentation d'un graphe style Netflix avec Neo4j :

- Nœuds `:Person`, `:Movie`, `:Genre`
- Relations `:FRIENDS_WITH`, `:WATCHED`, `:IN_GENRE`
- CRUD complet en Cypher
- Fonctionnalité distinctive : **recommandation par genre** — quand un nouveau film sort, la requête remonte automatiquement les utilisateurs à notifier

Environnement : Neo4j 5 via Docker, requêtes exécutées dans Neo4j Browser.

---

## Sources

- Neo4j, Inc. — *What is a Graph Database?*  
  https://neo4j.com/docs/getting-started/graph-database/

- Neo4j, Inc. — *Cypher Manual v5*  
  https://neo4j.com/docs/cypher-manual/current/

- Robinson, I., Webber, J., Eifrem, E. — *Graph Databases*, O'Reilly Media, 2nd edition, 2015  
  https://neo4j.com/graph-databases-book/

- DB-Engines — *Relational vs. Graph DBMS comparison*  
  https://db-engines.com/en/system/MySQL%3BNeo4j

- Angles, R., Gutierrez, C. — *Survey of Graph Database Models*, ACM Computing Surveys, 2008  
  https://dl.acm.org/doi/10.1145/1322432.1322433
