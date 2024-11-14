# Introduction à PyQGIS

QGIS, logiciel SIG open source écrit en C++, permet d’interagir avec Python. Cela offre plusieurs avantages :

- **Automatisation** : simplifiez des tâches répétitives via des scripts Python.
- **Transparence** : documentez chaque manipulation pour plus de traçabilité.
- **Extensibilité** : réalisez des analyses personnalisées grâce à des modules Python spécialisés.
- **Développement de plugins** : créez des plugins pour enrichir les fonctionnalités disponibles avec la possibilité de les partager avec la communauté QGIS.

Vous pouvez exécuter du code Python dans QGIS de trois manières :

1. Avec la **console Python** intégrée.
2. En exécutant un **script Python** dans QGIS ou en ligne de commande.
3. En créant un **plugin** Python pour QGIS.

## 1. La console Python

La console Python est intégrée dans QGIS. Elle peut être ouverte via le menu **Extension > Console Python** :

![](assets/menu-console-python.png)

ou en cliquant sur le bouton avec l'icône Python dans la barre d'outils *Extensions*.

Par défaut, la console s'affiche en bas à droite de l'écran, mais elle peut être déplacée où vous le souhaitez :

![](assets/qgis-console-python.png)

La console fonctionne en mode **REPL** (Read, Execute, Print, Loop) : chaque commande entrée est lue et exécutée directement, et son résultat est affiché dans la console. Vous pouvez ensuite entrer une autre commande, formant ainsi une boucle continue.

La console permet d'exécuter n'importe quel code Python, même s'il n'est pas directement lié à QGIS. Par exemple, vous pouvez définir des variables ou effectuer des calculs classiques :

![](assets/qgis-repl-python.png)

Si un module Python doit être installé, cela peut se faire avec l'outil de ligne de commande `pip`. La console QGIS supporte les commandes système en utilisant le préfixe `!`. Par exemple, pour installer le module `pooch` (utile pour télécharger des données), tapez :

```python
!pip install pooch
```

---

**Attention**: il peut arriver que la commande `!pip install pooch` ne fonctionne pas, avec un message comme quoi la `pip` n'a pas pu être trouvée. Dans un tel cas, essayez les choses suivantes:

1. Assurez-vous d'abord d'avoir la dernière version QGIS (LTR ou non) installée (désinstallez des versions plus anciennes d'abord).

2. Si vous avec un ordinateur Apple, veuillez consulter le document [«Configurer PyQGIS sur macOS»](./pyqgis-on-macos.md).

3. Si vous n'avez pas d'ordinateur Apple, ou que le problème persiste, essayez la procédure décrite sous [«Résoudre le problème des commandes introuvables»](./pyqgis-fix-path-issue.md).

---


### Ajouter une couche vectorielle

L'objectif principal de la console Python est d'interagir avec QGIS. Toutes les actions disponibles dans l'interface graphique de QGIS peuvent également être effectuées via Python.

L'interface QGIS est accessible en Python grâce à la variable `iface`, qui est automatiquement définie au lancement de QGIS. Une méthode clé de cette interface est `addVectorLayer`, qui permet de charger une couche vectorielle. Il suffit de spécifier le chemin d'accès de la couche, le nom de la couche, et la méthode de chargement (la méthode correspond aux sources de données dans le gestionnaire).

Avant de charger une couche vectorielle, assurez-vous d'avoir téléchargé les données sur votre ordinateur. Cet exemple utilise les [GIS Starter Data](https://www.geoinformatique.ch/data/gis-starter-data), que vous devrez d'abord télécharger et placer dans un dossier accessible.

Pour charger une couche vectorielle, utilisez la commande `addVectorLayer`, en remplaçant le chemin d'accès (path) par l'emplacement où vous avez enregistré les données. Par exemple :

```python
lyr_osm_bati = iface.addVectorLayer(
    '/votre/chemin/vers/gis-starter-data/osm/greater-bern-extract/gis_osm_buildings_a.shp',
    'batiments',
    'ogr'
)
```

Dans cet exemple, remplacez `/votre/chemin/vers/gis-starter-data/...` par le chemin exact de votre fichier `.shp`. Ce code fonctionnera également avec d'autres jeux de données SIG, à condition d'indiquer le bon chemin.

Pour afficher les noms des attributs d'une couche, utilisez :

```python
for attr in lyr_osm_bati.fields():
    print(attr.name())
```

Pour ouvrir la table d'attributs dans QGIS :

```python
iface.showAttributeTable(lyr_osm_bati)
```

Dans cet exemple, la couche a un attribut `type` indiquant le type de bâtiment (selon les données OSM). Nous allons compter les bâtiments par type en extrayant la valeur de cet attribut pour chaque entité (`feature`). Pour cela, nous utilisons un dictionnaire spécial (`defaultdict`) qui initialise chaque nouvelle clé à 0 :

```python
from collections import defaultdict
```
``` python
# Initialiser le dictionnaire pour stocker le nombre de bâtiments par type
type_cnts = defaultdict(int)
```
```python
# Récupérer les entités de la couche
batiments = lyr_osm_bati.getFeatures()
```
```python
# Boucler sur chaque bâtiment pour compter les types
for bati in batiments:
    type_bati = bati['type'] or 'non défini'  # Définit 'non défini' si le type est vide
    type_cnts[type_bati] += 1
```
```python
# Afficher les nombres d'écoles et d'églises
print(f"Écoles : {type_cnts['school']}")
print(f"Églises : {type_cnts['church']}")
```

## 2. Scripts Python

Dans la console Python, chaque instruction doit être saisie une par une, ce qui est pratique pour tester rapidement des commandes. Cependant, pour exécuter plusieurs lignes de code, il est préférable d'utiliser des fichiers de script.

La console Python de QGIS intègre un éditeur de scripts. Cliquez sur le bouton correspondant pour ouvrir l'éditeur sur la droite :

![](assets/console-python-script.png)

Un script peut être enregistré sur le disque (sous l'extension `.py`) pour conserver une trace du code et le réutiliser plus tard. Pour exécuter le script actif dans l'éditeur, cliquez sur le bouton avec le triangle vert.

## 3. Outils de géotraitement

QGIS dispose de nombreux outils de géotraitement. La boîte à outils est accessible via **Traitement > Boîte à outils** ou depuis la barre d'outils :

![](assets/geoprocessing-toolbox.png)

Tous les outils fonctionnent de manière similaire : un double-clic ouvre une boîte de dialogue où vous pouvez paramétrer le traitement. Par exemple, voici comment projeter une couche de bâtiments OSM en coordonnées suisses (en créant une couche temporaire) :

![](assets/traitement-project.png)

Pour les scripts, le bouton « Avancé » en bas de la fenêtre est utile. Il fournit, entre autres, la commande Python correspondant au traitement paramétré. Une fois le géotraitement effectué avec succès, vous pouvez copier cette commande pour l'intégrer dans un script Python.

![](assets/processing-copy-py-command.png)

La commande générée peut être longue. Il est souvent conseillé de formater le code pour en faciliter la lecture et la compréhension. Un bouton dans l'éditeur de code permet d'ajuster automatiquement le format.

## Pour aller plus loin...

- [Documentation PyQGIS](https://qgis.org/pyqgis/master/index.html)
- [PyQGIS Cookbook](https://docs.qgis.org/3.40/en/docs/pyqgis_developer_cookbook/index.html)
- [PyQGIS 101: Introduction to QGIS Python programming for non-programmers](https://anitagraser.com/pyqgis-101-introduction-to-qgis-python-programming-for-non-programmers) par Anita Graser
