# Recueil des Exercices - Support de formation

> ⚠️ **AVIS DE CONFIDENTIALITÉ & PROPRIÉTÉ INTELLECTUELLE**
> > **© 2026 Keyos-Éditions. Tous droits réservés.**
> > *Ce document est strictement confidentiel, protégé par les lois sur la propriété intellectuelle, et destiné à un usage pédagogique exclusif. Toute diffusion, reproduction ou copie non autorisée est formellement interdite.*

---

# Chapitre 1 : Rappel NumPy

**Exercice 1 :** Manipulation de matrices et slicing

Créez un tableau NumPy unidimensionnel contenant les entiers de 10 à 50 (inclus).
Extrayez-en une tranche (slicing) contenant uniquement les nombres pairs situés entre les indices 10 et 30.

**Exercice 2 :** Statistiques par axe et gestion des données manquantes

On dispose du tableau de températures suivant relevé dans 3 villes (lignes) sur 4 jours (colonnes) :

```python
temperatures = np.array([
    [21.5, 22.0, np.nan, 24.1],
    [15.0, 16.2, 17.0, 16.5],
    [28.0, np.nan, 30.2, 29.5]
])

```

Calculez la température moyenne pour chaque jour (en effectuant l'opération sur les lignes, `axis=0`) en gérant proprement les valeurs manquantes (`np.nan`).
Identifiez la ville (l'indice de la ligne) qui enregistre la température maximale globale sur l'ensemble de la période.

---

# Chapitre 2 : Présentation des Series

**Exercice 1 :** Gestion d'un catalogue de prix et conversion de types

Vous récupérez un extrait de données textuelles provenant d'un ancien système informatique. Les prix des articles sont stockés sous forme de chaînes de caractères, et on doit les nettoyer, les convertir pour effectuer des calculs, puis structurer le tout dans une `Series` Pandas dotée d'un index explicite.

Partez des listes de données brutes suivantes en Python :
```python
import pandas as pd

articles = ['Casque Audio', 'Clavier Gamer', 'Souris Sans Fil', 'Tapis de souris']
prix_texte = ['120.50', '85.00', '45.99', '19.99']

```

Créez une `Series` nommée `ser_prix` en utilisant la liste `prix_texte` pour les valeurs et la liste `articles` pour l'index explicite. Affichez son type initial (`dtype`).
À l'aide de la méthode `.astype()`, convertissez cette `Series` en nombres à virgule flottante (`float64`).
Calculez et affichez le prix de chaque article après application d'une augmentation générale de 10 % (multiplication vectorisée par `1.10`).

**Exercice 2 :** Analyse temporelle des ventes d'une boutique

Une boutique en ligne souhaite analyser son volume de commandes quotidiennes sur une période de quatre jours au début du mois de novembre 2026. Vous devez manipuler un index temporel et extraire des informations calendaires.

À l'aide de la fonction `pd.date_range()`, générez une plage de 4 jours consécutifs commençant le **1er novembre 2026** (`2026-11-01`).
Créez une `Series` nommée `commandes` contenant les valeurs `[15, 32, 28, 45]` en associant l'index temporel généré à l'étape précédente.
Utilisez le découpage (*slicing*) temporel par texte pour extraire et afficher uniquement les données allant du **2 novembre 2026 au 3 novembre 2026**.
En utilisant l'accesseur temporel `.dt` sur l'index de la série d'origine, extraire et afficher le nom complet des jours de la semaine correspondants (`day_name()`).

---

# Chapitre 3 : Manipuler les Series

**Exercice 1 :** Extraction d'une période spécifique et transformation ciblée des valeurs.

Dans cet exemple, nous disposons d'un suivi journalier du trafic d'un site web. Nous souhaitons **extraire** uniquement la semaine où s'est déroulée une campagne promotionnelle, puis **transformer** ces données en leur appliquant un coefficient de correction lié à une anomalie de comptage.

**Exercice 2 :** Extraction de composantes temporelles (.dt) et transformation conditionnelle.

Comment extraire des informations sémantiques à partir des dates (les jours de la semaine) pour ensuite appliquer une transformation conditionnelle (par exemple, appliquer un tarif week-end ou jour ouvré).

---

# Chapitre 4 : Présentation un DataFrame

**Exercice 1 :** Création d'un DataFrame à partir d'un dictionnaire et manipulation des propriétés structurelles

Vous gérez le parc informatique d'une PME. On dispose des informations relatives à trois ordinateurs portables en stock, qu'on doit structurer sous la forme d'un `DataFrame` Pandas pour en analyser les caractéristiques.

À l'aide d'un dictionnaire de listes, créez un `DataFrame` nommé `df_parc` contenant les données suivantes :
* **`Modele`** : `['Latitude 5420', 'ThinkPad T14', 'MacBook Pro']`
* **`Marque`** : `['Dell', 'Lenovo', 'Apple']`
* **`Prix_HT`** : `[950.0, 1100.0, 1850.0]`
* **`En_Stock`** : `[True, False, True]`

Attribuez un index explicite personnalisé au DataFrame en utilisant la liste d'identifiants : `['PC_01', 'PC_02', 'PC_03']`.
Affichez les dimensions du DataFrame via l'attribut `.shape` ainsi que les types de données de chaque colonne via `.dtypes`.
Extrayez la colonne `Prix_HT` dans une variable, puis vérifiez et affichez son type exact à l'aide de la fonction `type()` pour confirmer qu'il s'agit bien d'une `Series`.

**Exercice 2 :** Utilisation des axes (`axis`) et calculs statistiques orientés

Un centre de formation souhaite analyser les notes semestrielles obtenues par ses étudiants dans différentes matières scientifiques. Vous devez charger ces données et exploiter le paramètre `axis` pour automatiser les calculs.

Créez un `DataFrame` nommé `df_notes` à partir du dictionnaire suivant, en définissant les noms des étudiants comme index (`index=['Élève_A', 'Élève_B', 'Élève_C']`) :
```python
data_notes = {
    'Algorithmique': [14, 16, 10],
    'Base_de_Donnees': [12, 18, 15],
    'Reseaux': [16, 14, 13]
}

```

Calculez et affichez la **moyenne par matière** en utilisant explicitement le paramètre adapté (`axis=0`).
Calculez et affichez la **moyenne générale de chaque élève** en modifiant la direction du calcul avec l'axe horizontal (`axis=1`).
Supprimez la colonne `'Reseaux'` du DataFrame en utilisant la méthode `.drop()` avec l'argument `axis=1` (ou `columns=...`), puis affichez le DataFrame résultant.

---

# Chapitre 5 : Manipuler un DataFrame

**Exercice 1 :** Importez un petit fichier CSV et affichez les noms des colonnes à l'aide de l'attribut approprié.

**Exercice 2 :** Créez un DataFrame à partir d'un dictionnaire, puis utilisez `.iloc` pour extraire uniquement la première colonne de toutes les lignes.

---

# Chapitre 6 : Opérations sur les données

**Exercice 1 :** Diagnostic et Audit d'un Dataset de Ventes

On récupère un fichier CSV contenant des données de ventes brutes d'une entreprise e-commerce (`ventes_techno.csv`). Avant toute analyse marketing, on doit mener un audit rigoureux de la structure et de la santé des données.

```python
import pandas as pd
import io

csv_data = """id_vente,produit,categorie,prix_unitaire,quantite,date_vente
101,Clavier RGB,Accessoire,45.5,4,2026-05-10
102,Écran 27,Moniteur,299.0,2,2026-05-11
103,Souris Sans Fil,Accessoire,25.0,NaN,2026-05-11
104,PC Portable Pro,Ordinateur,1200.0,1,2026-05-12
105,Casque Audio,Accessoire,60.0,3,2026-05-13
"""
df_ventes = pd.read_csv(io.StringIO(csv_data))

```

* Importation et typage : Charge ce jeu de données en veillant à convertir la colonne `date_vente` directement en objets temporels (`datetime`) et à spécifier les colonnes catégorielles (`produit`, `categorie`).
* Inspection structurelle : Affiche les dimensions du DataFrame, la liste des axes (`.axes` ou `.columns`/`.index`), ainsi que le diagnostic complet de l'empreinte mémoire avec `info(memory_usage='deep')`.
* Audit des anomalies : Identifie le nombre de valeurs manquantes par colonne.
* Statistiques descriptives : Affiche un résumé statistique global (incluant les variables textuelles/catégorielles) pour évaluer les fourchettes de prix et les volumes.

**Exercice 2 :** Pipeline de Transformation et Synthèse Commerciale

À partir du catalogue de produits consolidé,on doit appliquer une série de transformations sécurisées, calculer des métriques personnalisées ligne par ligne, et extraire un rapport de synthèse décisionnel.

```python
import pandas as pd

df_catalogue = pd.DataFrame({
    'Reference': ['SKU-01', 'SKU-02', 'SKU-03', 'SKU-04'],
    'Produit': ['Clavier', 'Souris', 'Casque', 'Ecran'],
    'Prix_HT': [50.0, 25.0, 80.0, 300.0],
    'Stock': [15, 42, 8, 5]
})

```

* Isolation sécurisée (`.copy()`) : Crée une copie indépendante nommée `df_promo` pour préparer une opération promotionnelle sans risquer de corrompre le catalogue d'origine.
* Transformation par Pipeline (`.pipe()`) : Écris une fonction `appliquer_taxe(df, tva)` qui ajoute une colonne `Prix_TTC` (calculée avec une TVA de 20%), puis enchaîne-la via `.pipe()`.
* Traitement ligne par ligne (`.apply(axis=1)`) : À l'aide de `.apply()` et `axis=1`, génère une colonne `Valeur_Stock_HT` représentant la valorisation financière totale du stock pour chaque produit (`Prix_HT * Stock`).
* Synthèse globale (`.agg()`) : Utilise `.agg()` pour obtenir simultanément le prix moyen, minimum et maximum des produits HT, ainsi que la somme totale de la valeur du stock de l'entreprise.

---

# Chapitre 7 : Filtrage et Mappage dans les DataFrames Pandas

**Exercice 1 :** Analyse temporelle et extraction des pics d'activité
Manipuler les séries temporelles, convertir des chaînes de caractères en objets `datetime`, extraire des composantes temporelles et réaliser une agrégation croisée.

On dispose d'un journal de transactions brutes sous forme de texte. La direction souhaite identifier quels jours de la semaine génèrent le plus de chiffre d'affaires.

Créez un DataFrame contenant ces données brutes :
```python
import pandas as pd

df_logs = pd.DataFrame({
    'Horodatage': ['2026-10-01 09:30:00', '2026-10-02 14:15:00', '2026-10-03 18:45:00', 
                   '2026-10-04 10:00:00', '2026-10-05 16:20:00', '2026-10-06 11:10:00'],
    'Montant_EUR': [120.5, 45.0, 300.0, 85.0, 210.0, 95.0]
})

```
Convertissez la colonne `Horodatage` en un type `datetime` exploitable.
Extrayez le **nom du jour de la semaine** (ex : *Monday*, *Tuesday*...) dans une nouvelle colonne nommée `Jour_Semaine`.
Réalisez un regroupement (`groupby`) par `Jour_Semaine` pour calculer la somme totale du chiffre d'affaires (`Montant_EUR`) et le nombre de transactions par jour.

**Exercice 2 :** Enrichissement relationnel, Mapping et Filtrage
Croiser deux tables hétérogènes avec `pd.merge()`, traduire des codes via `.map()` et appliquer un filtre conditionnel multicritère.

On gére deux sources de données distinctes dans notre boutique : un historique de ventes contenant des identifiants produits, et un catalogue général fournissant les détails et les catégories.

Partez des deux DataFrames suivants :
```python

---

# Chapitre 8 : Nettoyage et Préparation des Données

**Exercice 1 :** Détection et correction des anomalies de texte et de valeurs manquantes

On dispose du DataFrame suivant simulant des inscrits à une newsletter :

```python
import pandas as pd
import numpy as np

df_inscrits = pd.DataFrame({
    'Nom': [' Alice ', 'BOB', 'charlie', ' Alice ', np.nan],
    'Age': [25, np.nan, 30, 25, 45],
    'Score': [85, 90, np.nan, 85, 120] # 120 est une valeur aberrante (score max = 100)
})

```

Supprimez les lignes strictement dupliquées.
Nettoyez la colonne `Nom` pour supprimer les espaces superflus et homogénéiser la casse (première lettre en majuscule).
Remplacez les âges manquants par la médiane des âges, et corrigez le score aberrant (> 100) en y insérant une valeur manquante (`np.nan`) avant de combler les scores par la moyenne.

**Exercice 2 :** Optimisation mémoire et traitement de chaînes

Créez un DataFrame contenant 10 000 lignes avec deux colonnes :

* `Departement` : contenant aléatoirement les valeurs `'75'`, `'69'`, `'13'` et `'31'`.
* `Montant` : des chaînes de caractères représentant des prix textuels (ex : `"150.50 €"`).

Nettoyez la colonne `Montant` pour la convertir en un type numérique (`float64`).
Convertissez la colonne `Departement` en type `category`.
Vérifiez l'empreinte mémoire du DataFrame optimisé à l'aide de `.info()`.

---

# Chapitre 9 : Gestion des dates et séries temporelles

**Exercice 1 :** Créez un `Timestamp` correspondant à la date d'aujourd'hui et ajoutez-lui un `Timedelta` de 48 heures.

**Exercice 2 :** Utilisez `period_range` pour générer une liste de 6 périodes trimestrielles commençant au premier trimestre 2026.

---

# Chapitre 10 : Statistiques et agrégations

**Exercice 1 :** Rapport de synthèse décisionnel par catégorie (`groupby` et `agg`)

En tant qu'analyste de données, la direction te demande de produire un tableau de bord synthétique pour analyser les performances des ventes par grande catégorie de produits.

```python
import pandas as pd

df_catalogue = pd.DataFrame({
    'Produit': ['Clavier', 'Souris', 'Casque', 'Webcam', 'Tapis', 'Écran'],
    'Categorie': ['Accessoire', 'Accessoire', 'Audio', 'Audio', 'Accessoire', 'Moniteur'],
    'Prix_HT': [50.0, 25.0, 80.0, 70.0, 15.0, 250.0],
    'Quantite_Vendue': [12, 45, 8, 15, 30, 5],
    'Chiffre_Affaires': [600.0, 1125.0, 640.0, 1050.0, 450.0, 1250.0]
})

```

À l'aide d'un enchaînement `groupby()` et `agg()`, construis un DataFrame de synthèse qui regroupe les données par `Categorie` et calcule pour chacune d'elles :

* Le nombre total de références distinctes (`count` sur la colonne `Produit`).
* Le prix unitaire moyen HT (`mean` sur la colonne `Prix_HT`), renommé explicitement `Prix_Moyen`.
* Le volume total d'articles vendus (`sum` sur la colonne `Quantite_Vendue`), renommé `Volume_Total`.
* Le chiffre d'affaires cumulé (`sum` sur la colonne `Chiffre_Affaires`), renommé `CA_Total`.
* Transforme l'index de groupe en colonne classique pour obtenir un tableau propre et plat.

**Exercice 2 :** Pipeline de transformation sécurisée et enrichissement (`copy`, `pipe`, `apply`)

Préparer une simulation commerciale : appliquer une politique de remise promotionnelle ciblée et générer des libellés textuels automatisés pour chaque ligne, sans altérer le jeu de données initial.

```python
import pandas as pd

df_stocks = pd.DataFrame({
    'Reference': ['REF-01', 'REF-02', 'REF-03'],
    'Produit': ['Clavier', 'Souris', 'Casque'],
    'Prix_Actuel': [50.0, 25.0, 80.0],
    'Stock_Restant': [10, 25, 4]
})

```

* Isolation sécurisée :** Créer une copie indépendante du DataFrame nommée `df_simulation` pour travailler sans risque de modification de l'original.
* Pipeline de remise :** Écrire une fonction personnalisée `appliquer_promo(df, taux)` qui calcule un nouveau prix `Prix_Promo` en appliquant le taux de réduction fourni (par exemple 15%), puis intègre cette fonction dans un appel `.pipe()`.
* Traitement ligne par ligne :** À l'aide de `.apply()` avec `axis=1`, ajoute une colonne `Alerte_Reappro` qui retourne la chaîne de caractères `"Prioritaire"` si le stock est strictement inférieur à 5, et `"Normal"` dans le cas contraire.

---

# Chapitre 11 : Jointures et fusion de données

**Exercice 1 :** Créez deux petits DataFrames avec une colonne identique et utilisez `merge` pour les fusionner.

**Exercice 2 :** Utilisez `pd.concat` pour ajouter une ligne supplémentaire à un DataFrame existant en créant un second DataFrame contenant uniquement cette ligne.

---

# Chapitre 12 : Persistance et formats

**Exercice 1 :** Créez un DataFrame, sauvegardez-le en format CSV, puis rechargez-le dans une nouvelle variable pour vérifier l'intégrité des données.

**Exercice 2 :** Expérimentez la sauvegarde d'un DataFrame contenant des types de données variés (nombres, textes, dates) au format Pickle et confirmez que les types sont conservés après le rechargement.

---

# Chapitre 13 : Gestion des structures multidimensionnelles

**Exercice 1 :** Manipulation d'un MultiIndex et sélection

Créez un DataFrame comportant un `MultiIndex` à deux niveaux : `"Ville"` (`"Paris"`, `"Marseille"`) et `"Mois"` (`"Janvier"`, `"Février"`), contenant une colonne `"Consommation_Elec_kWh"`.
Utilisez l'attribut `.loc` pour extraire l'ensemble des données relatives à la ville de `"Paris"`.

**Exercice 2 :** Utilisation de `pivot_table` et de `melt`

À partir du jeu de données suivant :

```python
import pandas as pd
df_donnees = pd.DataFrame({
    'Equipe': ['Dev', 'Dev', 'Ops', 'Ops'],
    'Projet': ['Alpha', 'Beta', 'Alpha', 'Beta'],
    'Budget': [50000, 30000, 20000, 45000]
})

```

Transformez ce tableau à l'aide de `pivot_table` pour obtenir les équipes en lignes, les projets en colonnes, et les budgets en valeurs.
Appliquez ensuite un `melt()` sur le résultat obtenu pour le ramener sous un format long.

---

# Chapitre 14 : Rappel Pyplot – Les fondations graphiques de Python

**Exercice 1 :** Création d'une figure multi-axes personnalisée

Créez un script Python qui génère une figure de taille `(8, 4)` contenant deux graphiques côte à côte (1 ligne, 2 colonnes) à l'aide de `plt.subplots()`.
Sur le premier sous-graphique, tracez une fonction carrée ($y = x^2$ pour $x$ allant de 0 à 5) avec une ligne rouge en pointillés.
Sur le second sous-graphique, tracez une fonction racine carrée ($y = \sqrt{x}$) avec une ligne bleue continue, puis enregistrez l'ensemble sous le nom `exercices_maths.png` à 300 DPI.

**Exercice 2 :** Habillage et superposition de courbes

À l'aide de `plt.plot()`, tracez sur un même repère deux courbes représentant l'évolution hypothétique des ventes de deux produits ("Produit A" et "Produit B") sur 4 trimestres.
Personnalisez le graphique en ajoutant un titre explicite, des libellés sur les axes $X$ et $Y$, une légende positionnée en haut à droite, ainsi qu'une grille de fond en pointillés.

---

# Chapitre 15 : Visualisation de données avec Pandas (`df.plot()`)

**Exercice 1 :** Création d'un graphique linéaire multi-courbes et habillage personnalisé

On suit l'évolution mensuelle des ventes de deux produits phares d'une entreprise au cours du premier semestre 2026. On doit représenter ces tendances sur un même graphique en veillant à soigner l'habillage visuel.

À l'aide de Pandas, créez un `DataFrame` nommé `df_suivi` contenant les données suivantes, avec les mois comme index :
* **`Mois`** : `['Janvier', 'Février', 'Mars', 'Avril', 'Mai', 'Juin']`
* **`Produit_A`** : `[1200, 1350, 1500, 1420, 1600, 1750]`
* **`Produit_B`** : `[900, 1100, 1050, 1300, 1450, 1600]`
Définissez la colonne `'Mois'` comme index du DataFrame.
Utilisez la méthode `.plot()` pour tracer ces deux séries sous forme de graphique linéaire (`kind='line'`).
Personnalisez le graphique en ajoutant :
* Une dimension physique de `(8, 4)` via `figsize`.
* Un titre explicite (ex : *"Comparatif des ventes - 1er semestre 2026"*).
* Des libellés clairs sur les axes (`xlabel` et `ylabel`).
* Une grille en pointillés (`plt.grid()`).

**Exercice 2 :** Génération d'une grille de graphiques multi-subplots

La direction souhaite analyser simultanément plusieurs indicateurs de performance d'un site web (visites, conversions et taux de rebond) sur une semaine. Plutôt que de multiplier les figures séparées, on doit concevoir un tableau de bord condensé.

Créez un `DataFrame` nommé `df_dashboard` à l'aide de la structure suivante :
```python
import pandas as pd

df_dashboard = pd.DataFrame({
    'Date': pd.date_range(start='2026-10-01', periods=5, freq='D'),
    'Visiteurs': [1200, 1450, 1300, 1600, 1850],
    'Conversions': [45, 60, 52, 70, 85],
    'Taux_Rebond': [0.45, 0.42, 0.48, 0.39, 0.35]
}).set_index('Date')

```

Utilisez la méthode `.plot()` en combinant les arguments **`subplots=True`** et **`layout=(2, 2)`** pour répartir les métriques dans une grille de sous-graphiques.
Ajustez les dimensions globales à `(10, 6)`, activez le partage de l'axe horizontal (`share=True`), et ajoutez des marqueurs sur les points (`marker='o'`).
Ajoutez un titre global à la figure (`plt.suptitle(...)`) et utilisez `plt.tight_layout()` pour éviter tout chevauchement avant d'afficher le résultat.

---

# Chapitre 16 : Étude de cas

**Exercice 1 :** Analyse par discipline sportive

À l'aide du DataFrame nettoyé `df_propre` :

Réalisez un groupement (`groupby`) sur la colonne `Discipline` pour compter le nombre total d'épreuves disputées.
Identifiez la discipline qui a mobilisé le plus grand nombre d'athlètes uniques.

**Exercice 2 :** Suivi temporel des performances

Convertissez la colonne `Date_Epreuve` en index temporel (`DatetimeIndex`).
Utilisez un regroupement par date (`resample` ou `groupby` sur la date) pour tracer l'évolution quotidienne du nombre total de médailles décernées au cours de la quinzaine olympique.

---

