---
marp: true
theme: default
paginate: true
---
# ordre du jour
présentation du projet d'optimisation de l'etl d'order item
- contexte
    - warehouse
    - schema
- problème
- solution
    - batch_id
    - table
- encore meilleure solution

---
### warehouse
<img src="system.drawio.svg" style="width: auto; height: 35%;" />

---
### schema
<img src="double-star.svg" style="width: auto; height: 75%;" />

---
## problème
- outils de calcul de sli de databricks limité
- aucune manière de tracker le sli e2e (ie. creation to staging)

## solution
- une table de monitoring populée à chacun des stage
    - exposer a prometheus et grafana

---
## nouveau schema
<img src="monitoring-table.svg" style="width: auto; height: 75%;" />

