# Importer et convertir des données dans QGIS

QGIS permet de lire un grand nombre de formats de couches vectorielles et raster.

Parmi les formats de données de couches vectorielles, les formats les plus répandus sont:

- le format **ESRI Shape**
- les bases de données **Geopackage**
- les **Geodatabases** d'ESRI
- les fichiers **GeoJSON**

Et parmi les formats de données de couches raster, on trouve très souvent des fichiers TIFF (donc des images) qui contiennent en même temps la localisation géographique. On les appelle des fichiers **GeoTIFF**.

Finalement, pour des données ponctuelles, on utilise parfois des fichiers CSV simples. Ce sont donc des fichiers en format texte qui contiennent des données tabulaires comme un fichier Excel, et où les colonnes sont séparées avec un séparateur, le plus souvent une virgule (d'où le nom; CSV = comma-separated values), un point-virgule ou un tabulateur.

L'ajout de ces données se fait toujours de manière similaire à travers le **gestionnaire des sources de données**. Chaque type de données possède son propre onglet à gauche (Vecteur, Raster, et pour les fichiers CSV *«Texte Délimité»*).

La seule chose à laquelle il faut faire attention: il y a aussi un onglet «GeoPackage» dans la liste à gauche. Cet onglet permet de créer une connexion permanente à un GeoPackage, et ceci à travers l'ensemble des projets SIG. Généralement, on souhaite plutôt utiliser les GeoPackages spécifiquement pour le projet en cours. Dans ce cas, on les traite comme une données vectorielle.

Les différentes opérations de lecture et écriture de couches vectorielles dans QGIS sont illustrées dans les vidéos suivantes:

[QGIS: Lecture de données vectorielles](https://youtu.be/crAKLIPPCqQ)

QGIS: Convertir des couches vectorielles en format Geopackage:
- [vidéo pour Windows et Linux](https://youtu.be/xRDcsOJy6Fw)
- [vidéo pour macOS](/https://youtu.be/RNsoMtcVjUo)

[QGIS: Importer un fichier CSV](https://youtu.be/AsNhCy2ztUg)
