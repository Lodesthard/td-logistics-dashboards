# Réflexions — TD5, trois rôles, trois KPI, un réseau logistique

Chiffres issus de `scripts/compute_kpis.py` sur `data/cities.csv` (run de l'Action
« Compute dashboards », identique à la sortie du manuel) : réseau de base
Paris / Lyon / Lille, 20 villes, 7 422 409 habitants, 371 120 t/an de demande estimée.

---

## 1. Avant de construire — quel acteur sera le plus en désaccord ?

**Prédiction : le responsable logistique / opérations (coûts).** Le responsable RSE
et le planificateur réseau devraient tous les deux pousser à ouvrir un entrepôt au
sud : des routes plus courtes, c'est à la fois moins de CO2 et un meilleur service.
Le responsable des coûts, lui, verra un entrepôt de plus comme un coût fixe
supplémentaire, et cherchera plutôt à fermer le site le moins utilisé.

## 2. Quelle hypothèse je défendrais le moins devant un client ?

**La demande estimée par la population.** Les coûts (0,12 €/t·km, 40 €/t/an) ne sont
pas sourcés non plus, mais ils ne changent aucune décision dans ce modèle. Le
stockage vaut 40 €/t partout, et le transport n'est qu'une constante multipliée par
les t·km, donc les classements restent les mêmes quelle que soit la valeur. Le facteur
CO2 a au moins une fourchette publiée et citée (0,047 à 0,12 kg/t·km, ministère et
ADEME) : il peut se tromper d'un facteur 2,5 sur les tonnes, mais pas sur l'ordre des
routes.

La population, elle, n'a aucune source logistique, et c'est **le multiplicateur de
tous les chiffres des trois dashboards**. Elle déforme déjà le résultat : Marseille
pèse 873 076 habitants parce que la commune fait ~240 km², contre 522 250 pour Lyon
sur ~48 km². C'est le même piège des limites communales qu'au TD2. Ça suffit à faire
de Marseille la route n°1 en CO2 et le meilleur scénario. Et le fret ne suit pas les
habitants mais l'industrie, la distribution et les ports (Fos-sur-Mer n'existe pas
dans ce modèle).

## 3. Deux KPI qui poussent à des actions différentes pour le même entrepôt

**Lille, CO2/coût contre réseau.** Sur `co2.html`, Lille émet **0 kg**. Sur
`cost.html`, il coûte **0 € de transport**, c'est le moins cher par tonne (40,00 €/t)
et son seul coût est son stockage (466 196 €). Lu seul, ça dit « entrepôt inutile,
fermons-le ». Le dashboard réseau dit l'inverse : fermer Lille fait passer la
distance moyenne pondérée de **168,04 à 174,43 km**. Recalculé avec les mêmes
constantes, ça ajoute **+237 t de CO2/an** et **+0,29 M€** de transport, parce que
les 233 098 habitants de Lille seraient servis depuis Paris (203,5 km).

**Lyon, coût contre CO2.** La barre empilée montre que le coût de Lyon est d'abord du
stockage (7,79 M€ contre 5,69 M€ de transport), ce qui pousse à « réduire le stock à
Lyon ». Le dashboard CO2 montre que **76 %** des émissions du réseau partent de Lyon,
ce qui pousse à agir sur ses routes. Ce sont deux leviers différents pour le même
site.

Ce que j'ai compris en recalculant : CO2, coût de transport et distance réseau sont
**la même somme de t·km multipliée par une constante** (la demande est
proportionnelle à la population). Le CO2 total vaut exactement
0,005 × population × distance moyenne. Les trois KPI classent donc les scénarios dans
le même ordre. Ils ne divergent que par leur **niveau d'agrégation** (par route, par
entrepôt ou par réseau), et c'est là qu'ils mènent à des actions différentes.

## 4. Est-ce que je recommanderais la route n°1 telle quelle ?

La route n°1 est **Marseille ← Lyon** : 277,6 km, 1 212 t de CO2/an, soit **19,4 %**
du réseau à elle seule. Je la garderais en tête, mais je vérifierais trois points
avant de la recommander :

- **Le remède n'est pas la consolidation.** 43 654 t/an, c'est ~120 t par jour, donc
  des camions déjà pleins : il n'y a rien à regrouper. Le bon levier est le **report
  modal**. L'axe Lyon–Marseille (vallée du Rhône) est l'un des mieux équipés de France
  en fret ferroviaire et fluvial, ce qui rend le report réaliste. Mais Marseille est
  aussi un candidat à un nouvel entrepôt : la vraie question est de savoir s'il faut
  servir cette route autrement ou la supprimer (réflexion 6).
- **La distance est à vol d'oiseau.** La route réelle Lyon–Marseille fait environ
  315 km (+13 %), mais l'écart varie beaucoup selon le relief. Lyon–Toulouse : 360 km
  à vol d'oiseau contre ≈ 535 km par la route (+49 %, détour par le Massif central).
  Lyon–Nice : 298 contre ≈ 470 km (+58 %, les Alpes). Avec des distances routières
  approximatives, Toulouse (≈ 1,32 kt) talonne Marseille (≈ 1,37 kt) et **Nice passe
  de la 6e à la 3e place**. Le classement des routes est donc fragile au-delà du n°1.
- **Le n°1 dépend du proxy population** (réflexion 2) : c'est la taille de la commune
  de Marseille qui la place en tête.

## 5. Quel levier tirer en premier pour l'entrepôt le plus coûteux ?

L'entrepôt le plus coûteux est **Lyon : 13,48 M€/an**. Le stockage y pèse le plus
(7,79 M€, 57,8 %), mais **je tirerais le levier transport**. Le chiffre qui m'a
convaincu : **69,21 €/t à Lyon, contre 40,00 €/t à Lille**. Le stockage vaut 40 €/t
dans tous les entrepôts. Dans ce modèle, il ne peut baisser qu'en stockant moins de
tonnes, c'est-à-dire en déplaçant le coût ailleurs. Les **29,21 €/t** d'écart sont
donc 100 % du transport (contre 10,91 €/t à Paris), et c'est la seule partie
réellement pilotable. La route Marseille représente à elle seule 1,45 M€, soit
**25,6 %** du transport de Lyon.

La barre empilée est trompeuse ici : le plus gros segment (le stockage) est
proportionnel au volume, pas à une inefficacité. Autre détail : la carte KPI « Avg
cost per tonne » fait une moyenne **non pondérée** des trois entrepôts (53 €), alors
que le vrai coût moyen par tonne du réseau est de **60,16 €**. Le petit Lille tire la
moyenne vers le bas.

## 6. Ma recommandation réseau, le caveat, et retour sur la réflexion 1

**Recommandation : ouvrir Marseille, et garder Lille en attendant ses coûts fixes.**

| Scénario | Distance moy. pondérée | CO2 (t/an) | Transport (M€/an) |
|---|---|---|---|
| + Marseille | **115,03 km** | 4 269 | 5,12 |
| + Bordeaux | 139,89 km | 5 192 | 6,23 |
| base (Paris, Lyon, Lille) | 168,04 km | 6 236 | 7,48 |
| − Lille | 174,43 km | 6 473 | 7,77 |

Marseille réduit la distance de **31,5 %**, et donc du même pourcentage le CO2
(−1 967 t/an) et le transport (−2,36 M€/an). Bordeaux fait deux fois moins bien.
Nuance : **62 % du gain** (32,65 km sur 53,01) vient de Marseille qui se sert
elle-même. C'est le même effet que le paradoxe de Lille, et il repose sur le proxy
population.

**Le caveat change ma confiance sur l'ampleur, pas sur la direction, et il permet
de chiffrer l'étape suivante.** Ce modèle n'a aucun coût fixe : le stockage
(14,84 M€) est identique dans tous les scénarios, donc *ajouter un entrepôt est
toujours gagnant* par construction. La vraie question devient un seuil. Marseille
vaut le coup si son coût fixe annuel (bail, personnel, capex amorti) est **inférieur
à ~2,36 M€/an**, ce qui me paraît plausible. Pour Lille, le seuil n'est que de
**0,29 M€/an** (et 237 t de CO2) : un vrai entrepôt peut facilement coûter plus que
ça par an. C'est donc **la seule décision que le caveat pourrait inverser**. Je
garderais aussi un œil sur des scénarios que le modèle ne teste pas :
+ Marseille − Lille donne 121,42 km, et + Marseille + Bordeaux donne 89,62 km. La
liste des scénarios est elle-même un choix.

**Retour sur la réflexion 1 : juste sur la personne, faux sur la raison.** Les trois
dashboards construits **sont d'accord sur Marseille**, puisqu'ils mesurent tous la
même somme de t·km. Le désaccord que j'attendais du responsable des coûts n'apparaît
pas, parce que son modèle n'a pas de coût fixe : son dashboard ne peut pas s'opposer
à une ouverture. Le seul désaccord visible porte sur **Lille**. Les pages CO2 et coût
le montrent inactif (0 kg, 0 €), la page réseau montre que le fermer dégrade tout. Le
responsable des opérations reste bien le plus susceptible de s'opposer aux deux
autres (fermer Lille pour économiser son coût fixe, contre +237 t de CO2 et +6,4 km
de distance moyenne), mais à cause de données **absentes** du modèle, pas des données
qu'il montre.

## 7. Étape 15 — le quatrième graphique : un Pareto sur le dashboard CO2

**Dashboard étendu :** `co2.html`. J'ai ajouté un **diagramme de Pareto** (toutes les
routes triées en barres, avec la part cumulée du CO2 en ligne sur un axe de droite)
et trois cartes KPI.

**Question à laquelle il répond :** *sur combien de routes faut-il agir, et où
s'arrêter ?* Le top 10 classe les routes mais ne dit pas si le CO2 est concentré sur
3 routes ou étalé sur 15. Or un responsable RSE a un budget limité de projets de
consolidation ou de report modal. Réponse sur les vraies données : **4 routes sur 17
couvrent 51,7 %** du CO2 du réseau, **8 routes couvrent 80,3 %**, et **76 %** du CO2
part de Lyon. Le programme peut donc se limiter à 4 routes, toutes au départ de Lyon,
pour un premier palier à 50 %.

**Pourquoi ce type de graphique :** la lecture utile est **cumulative** (« les N
premières routes font X % »), et seule la ligne cumulée posée sur des barres triées la
donne directement. Un camembert ne montre aucun cumul et compare mal des parts
proches. Le top 10 existant s'arrête à 10 et masque la longue traîne. Une simple
carte « top 4 = 52 % » donnerait le chiffre mais pas la forme de la courbe, donc pas
où le rendement des actions suivantes devient faible. J'ai coloré en vert foncé les
routes au départ de Lyon, parce que le constat « tout part de Lyon » relie ce
graphique à la décision réseau (réflexion 6). Le calcul est fait dans la page à
partir de `all_cities` déjà présent dans `co2.json`, sans modifier le script.
