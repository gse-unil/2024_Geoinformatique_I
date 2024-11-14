# Résoudre le problème des commandes introuvables

**Note importante**: Si vous êtes sur macOS, essayez d'abord l'approche décrite dans le document [«Configurer PyQGIS sur macOS»](./pyqgis-on-macos.md).

Dans QGIS, il peut parfois arriver que certaines commandes lancées depuis la console PyQGIS ne fonctionnent pas. Une de ces commande est `pip`. Pour contourner ce problème, une solution consiste à créer un environnement Python séparé avec Anaconda/Miniconda et à le configurer pour QGIS. Voici les étapes :

## 1. **Installer Miniconda ou Anaconda**  
   Téléchargez et installez Miniconda (ou Anaconda) en suivant les instructions officielles : [Instructions pour l'installation de Miniconda](https://docs.anaconda.com/miniconda/miniconda-install/).

## 2. **Créer un environnement Python dédié pour QGIS**  
   Ouvrez une console `Anaconda Prompt` et créez un environnement pour QGIS en incluant les bibliothèques SIG nécessaires, comme `pooch`, `geopandas`, `shapely`, `fiona` et `rasterio` :

   ```bash
   conda create -n qgis_env python=3.9 pooch geopandas shapely fiona rasterio
   ```

## 3. **Pointer QGIS vers cet environnement**  
   Une fois l'environnement configuré, liez-le à QGIS en suivant ces quatre étapes :
   - 3.1. Allez dans **Préférences > Options > Système** dans QGIS.
   - 3.2. Dans la section **Environnement**, cochez "Utiliser des variables personnalisées".
   - 3.3. Cliquez sur le bouton vert "+" pour ajouter une nouvelle variable nommée `PYTHONPATH`.
     - Colonne **Appliquer** : sélectionnez "Écraser".
     - Colonne **Valeur** : entrez le chemin vers le dossier `site-packages` de l'environnement : `chemin/lib/python3.9/site-packages`, où `chemin` est le chemin copié de l'étape suivante.
   
   Pour trouver ce chemin :
   - Ouvrez une console Anaconda.
   - Activez l'environnement avec:

     ```bash
     conda activate qgis_env
     ```

   - Utilisez cette commande pour obtenir le chemin du dossier `site-packages` :

     ```bash
     python -c "import site; print(site.getsitepackages()[0])"
     ```

   N'oubliez pas la quatrième étape :
   
   - 3.4. **Ajouter le chemin au PATH** : Ajoutez une autre variable, nommée `PATH`, en sélectionnant "Écraser" dans **Mode Appliquer** et définissez la **Valeur** à `chemin/bin`, où `chemin` est le même que précédemment.

## 4. **Redémarrer QGIS**
   Redémarrez QGIS pour appliquer les changements. Vous pouvez maintenant utiliser `pip` et installer des packages dans cet environnement. Vérifiez l'installation avec :
   - `import pooch` suivi de `print(pooch.__version__)`, qui doit afficher la version installée dans `qgis_env`.
   - `import pip` suivi de `print(pip.__version__)`, qui doit correspondre à la version installée dans `qgis_env`.
