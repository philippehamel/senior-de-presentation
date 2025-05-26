---
marp: true
paginate: true
style: |
  /* Minimalist Dark Theme */
  section {
    background-color: #1c1c1c; /* Neutral dark gray background */
    color: #d6d6d6; /* Soft light gray text */
    font-family: 'Helvetica', 'Arial', sans-serif;
  }

  h1, h2, h3, h4, h5, h6 {
    color: #ffffff; /* Pure white headers for contrast */
    font-weight: bold;
  }

  p, li {
    color: #bfbfbf; /* Softer gray for body text */
  }

  code {
    background-color: #2a2a2a; /* Slightly lighter gray for code blocks */
    color: #d6ba73; /* Muted gold for code text */
    padding: 2px 4px;
    border-radius: 4px;
  }

  img {
    border: none; /* No border around images for a clean look */
    border-radius: 0; /* No rounding for a minimalist style */
  }

  footer {
    color: #888888; /* Dimmed footer text */
    font-size: 0.8em;
  }

  hr {
    border: 1px solid #333333; /* Subtle horizontal rule */
  }
---

## ordre du jour

présentation du projet de monitoring de l'etl d'order item

- situation
  - contexte
  - warehouse
  - schema
- problème
- solution
  - batch_id
  - table
- encore meilleure solution

---

### contexte

- infrastructure etl
  - databrick lakehouse
  - architecture médaillon
  - delta lake pour storage
  - spark pour batch streaming
  - auncun besoin de données historique, upsert pour les changements de dimension
- staging mysql
  - <i>row level security</i>
  - integration avec l'authentification de l'api
  - beaucoup plus rapide

---

### warehouse

<img src="system.drawio.png" />

---

### stratégie de test
- test ETL
  - Unit test pour les transformations (pytest, pyspark.testing)
  - test d'integration entre les stages, tester la déduplication
  - voulais vrm implémenter great expectation pour data contract, schema, garbage entry (backlogged)
- test bd
  - data contract et schema
- test api
  - test fonctionnel (MockMvc)
  - test unitaire
  - test sécurité

---

## schema

<img src="double-star.svg"/>

---

## problème

- outils de calcul de sli de databricks limité
- aucune manière de tracker le sli e2e (ie. creation à requete)

## solution

- une table de monitoring populée à chacun des stages
  - exposer a prometheus et grafana
  - detection de bris de slo
  - alertage

---

## nouveau schema

<img src="monitoring-table.svg"/>

---

## résultat

on se rend compte que le délai ne venais pas de l'etl, mais de la lecture de la base de donnés mysql.

---

## encore meilleure solution

stage de landing en scylla.
avec mon schema préféré :
aka <b>ONE BIG TABLE</b>
