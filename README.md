# TD5 — Trois rôles, trois KPI, un réseau logistique

Un même réseau (entrepôts Paris, Lyon, Lille ; 20 villes françaises réelles), trois
dashboards pour trois décideurs :

| Page | Pour qui | Décision |
|---|---|---|
| [`co2.html`](https://lodesthard.github.io/td-logistics-dashboards/co2.html) | Responsable RSE | Quelles routes prioriser pour réduire le CO2 |
| [`cost.html`](https://lodesthard.github.io/td-logistics-dashboards/cost.html) | Responsable logistique / opérations | Par entrepôt : levier transport ou stockage |
| [`network.html`](https://lodesthard.github.io/td-logistics-dashboards/network.html) | Planification réseau | Ouvrir Bordeaux ou Marseille, fermer Lille ? |

- **Dashboards :** https://lodesthard.github.io/td-logistics-dashboards/
- **Réflexions :** [REFLECTIONS.md](REFLECTIONS.md)

## Structure

```
data/cities.csv                     # 20 villes, coordonnées, population (proxy de demande)
scripts/compute_kpis.py             # calcule co2.json, cost.json, network.json
docs/index.html, co2.html, cost.html, network.html
docs/js/france-map-background.js    # contour de la France (plugin Chart.js)
docs/data/*.json                    # générés et commités par l'Action
.github/workflows/compute.yml       # lancement manuel (workflow_dispatch)
```

## Résultats (réseau de base)

- CO2 total : 6 236 t/an, 16,8 kg/t en moyenne ; Lyon 76 %, Lille 0 kg.
- Coût : Lyon 13,48 M€ (69,21 €/t), Paris 8,39 M€ (50,91 €/t), Lille 0,47 M€ (40 €/t).
- Scénarios (distance moyenne pondérée par la population) : + Marseille 115,03 km,
  + Bordeaux 139,89 km, base 168,04 km, − Lille 174,43 km.

## Étape 15 — graphique ajouté

Diagramme de Pareto sur `co2.html` : toutes les routes triées par CO2, avec la part
cumulée en ligne. 4 routes sur 17 couvrent 51,7 % du CO2, 8 en couvrent 80,3 %.
Argumentation dans la réflexion 7.

## Hypothèses (affichées sur chaque page)

Demande = population / 1 000 × 50 t/an (proxy illustratif). CO2 = 0,10 kg CO2e/t·km
(dans la fourchette citée de 0,047 à 0,12 ; à vérifier dans la Base Empreinte de
l'ADEME). Coûts de 0,12 €/t·km et 40 €/t/an : illustratifs, non sourcés.

## Relancer

Actions → **Compute dashboards** → Run workflow, ou en local :
`python scripts/compute_kpis.py`.
