# Orbitel : prédiction du churn optimisée sur un coût métier

Le notebook part d'un modèle interne de régression logistique, le diagnostique, le corrige, puis l'optimise sur le coût réel des erreurs plutôt que sur un score technique.

## Hypothèses métier
- Faux négatif (client perdu non détecté) : 200 €
- Faux positif (action de fidélisation inutile) : 50 €

## Démarche
1. Diagnostic du modèle initial : AUC 0,357, coût 2 350 €, dont 85 % dû aux faux négatifs.
2. Corrections : normalisation (StandardScaler), convergence (max_iter), pondération des classes.
3. Feature engineering : variable `frustration` (plus de 5 réclamations et satisfaction < 2,5).
4. Optimisation du seuil de décision de 0,1 à 0,9 pour minimiser le coût total.
5. Random Forest testé en alternative, écarté.

## Résultats sur le jeu de test

| Modèle | AUC | FN | FP | Coût |
|---|---|---|---|---|
| Modèle initial (seuil 0,5) | 0,357 | 10 | 7 | 2 350 € |
| Régression logistique corrigée | 0,542 | 11 | 4 | 2 400 € |
| + frustration, seuil 0,1 (retenu) | 0,696 | 2 | 18 | 1 300 € |

Coût réduit de 45 %, 10 churners détectés sur 12 contre 2.

## Limites
105 observations d'entraînement et 40 de test : résultats à confirmer sur un volume plus grand.

## Lancer
Modifier `DATA_DIR` dans le notebook pour pointer vers les fichiers de données (non versionnés), puis exécuter les cellules dans l'ordre.

Stack : Python, pandas, scikit-learn, matplotlib, seaborn.
