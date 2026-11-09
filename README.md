# Segmentation clients RFM – E-commerce

Analyse de 541 909 transactions d'une boutique en ligne britannique pour segmenter 4 327 clients selon leur comportement d'achat, et en tirer des recommandations marketing.

**Résultat principal : 25,6 % des clients génèrent 67,5 % du chiffre d'affaires, et 653 bons clients sont en train de partir.**

## Liens

- 📊 [Tableau de bord interactif (Looker Studio)](https://datastudio.google.com/reporting/48250ffc-ccb1-43be-accb-47f7e971c9dc/page/VW6CG)
- 📓 [Notebook complet](Segmentation_RFM.ipynb)

## Contexte

Une boutique en ligne veut savoir quels clients fidéliser, lesquels relancer et lesquels elle est en train de perdre. La méthode **RFM** classe chaque client selon trois critères :

- **Récence** : nombre de jours depuis le dernier achat
- **Fréquence** : nombre de commandes
- **Montant** : total dépensé

## Données

Jeu de données public [Online Retail (UCI Machine Learning Repository)](https://archive.ics.uci.edu/dataset/352/online+retail) : 541 909 lignes de ventes entre décembre 2010 et décembre 2011.

## Démarche

1. **Exploration** : structure, types de colonnes, valeurs manquantes, statistiques descriptives.
2. **Nettoyage** : suppression des lignes sans identifiant client (25 %), des annulations et des quantités ou prix négatifs. 397 884 lignes conservées.
3. **Calcul des indicateurs RFM en SQL** (DuckDB) : une ligne par client.
4. **Détection et correction d'une anomalie** : certains clients apparaissaient parmi les meilleurs alors qu'ils avaient annulé leur commande. Exemple : un client affiché à 168 000 £ n'avait réellement dépensé que 2,90 £. Correction en SQL (CTE, `NOT EXISTS`) en retirant les achats suivis d'une annulation identique.
5. **Scores de 1 à 5 par quintiles** et attribution de 6 segments selon la récence et la fréquence.
6. **Restitution** dans un tableau de bord Looker Studio.

## Résultats

| Segment | Clients | Part des clients | Part du CA |
|---|---|---|---|
| Champions | 1 108 | 25,6 % | 67,5 % |
| Clients fidèles | 321 | 7,4 % | 8,7 % |
| À risque | 653 | 15,1 % | 7,9 % |
| Perdus | 1 074 | 24,8 % | 6,3 % |
| Nouveaux / prometteurs | 632 | 14,6 % | 5,3 % |
| À relancer | 539 | 12,5 % | 4,2 % |

## Recommandations

- **Champions** : priorité à la fidélisation (programme VIP, avant-premières). Perdre 10 % de ces clients coûterait près de 7 % du chiffre d'affaires.
- **À risque** : campagne de reconquête rapide. Ces clients commandaient en moyenne 3,4 fois mais n'ont rien acheté depuis 153 jours ; ils connaissent la boutique et restent récupérables.
- **Nouveaux / prometteurs** : déclencher une 2e puis une 3e commande pour en faire de futurs Champions.
- **Perdus** : investissement minimal (emails automatiques).

## Limites

- Seules les annulations exactes (même client, même produit, même quantité) ont été retirées ; les annulations partielles restent comptées.
- Les segments reposent sur la récence et la fréquence ; le montant sert à mesurer le poids de chaque segment.
- Une seule année de données, d'une boutique principalement britannique.

## Outils

Python (Pandas) · SQL (DuckDB) · Google Colab · Looker Studio
