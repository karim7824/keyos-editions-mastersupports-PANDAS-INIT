# Chapitre 12 :  Persistance et formats 

La persistance des données sous Pandas consiste à faire le pont entre la mémoire vive (le DataFrame en RAM) et le stockage sur disque (fichiers physiques). Qu'il s'agisse d'ingérer des données brutes issues de systèmes tiers ou d'exporter des rapports finalisés, Pandas propose des fonctions unifiées et performantes basées sur les préfixes **`read_`** et **`to_`**.

Dans ce chapitre :

* Le format CSV (*Comma-Separated Values*) : Le standard tabulaire
* Le format JSON (*JavaScript Object Notation*) : Le roi du Web
* Le format XML (*Extensible Markup Language*) : Les données semi-structurées
* accès aux données stockées en base relationnelle (`sql`)
* Persistance des données via le format Pickle
* synthèse des méthodes de persistance
* Exemple de synthèse
* Exercices de fin de chapitre

## Le format CSV (*Comma-Separated Values*) : Le standard tabulaire

Le CSV est le format universel pour les données tabulaires. Léger et lisible, il est géré nativement par la quasi-totalité des outils logiciels.

### Lecture d'un fichier CSV (`pd.read_csv`)

La fonction `pd.read_csv()` est la plus utilisée en science des données. Elle intègre de nombreux paramètres pour s'adapter à la structure du fichier source :

```python
import pandas as pd

# Lecture d'un fichier CSV avec gestion du séparateur et de l'encodage
df_ventes = pd.read_csv(
    'ventes_2026.csv',
    sep=',',               # Séparateur de colonnes (virgule, point-virgule ';', tabulation '\t')
    encoding='utf-8',      # Encodage des caractères (utf-8, latin-1 pour les accents français)
    index_col=0,           # Utiliser la première colonne comme index du DataFrame
    na_values=['NA', 'NULL', '?'] # Définition personnalisée des valeurs manquantes
)

print(df_ventes.head())

```

### Sauvegarde d'un DataFrame (`df.to_csv`)

Pour exporter un DataFrame vers un fichier CSV, on utilise la méthode `.to_csv()` :

```python
# Exportation propre sans inclure l'index technique de Pandas
df_ventes.to_csv(
    'export_ventes_nettoye.csv',
    index=False,           # Évite d'enregistrer l'index (souvent inutile)
    sep=';',               # Privilégier le point-virgule pour Excel en version française
    encoding='utf-8-sig'   # 'utf-8-sig' garantit l'affichage correct des accents sous Excel
)

```

## Le format JSON (*JavaScript Object Notation*) : Le roi du Web

Le format JSON est omniprésent dans les architectures web et les API REST. Contrairement au CSV, il gère des structures **hiérarchiques** (imbrications d'objets et de listes).

### Lecture d'un fichier JSON (`pd.read_json`)

Pandas analyse automatiquement la structure JSON pour la convertir sous forme tabulaire :

```python
# Chargement d'un fichier JSON structuré en enregistrements
df_produits = pd.read_json('catalogue.json', orient='records')

print(df_produits.head())

```

### Sauvegarde au format JSON (`df.to_json`)

L'argument **`orient`** est crucial lors de l'exportation JSON pour définir la structure géométrique du fichier final :

```python
# Exportation au format liste d'enregistrements (idéal pour le web/API)
df_produits.to_json(
    'catalogue_export.json',
    orient='records',      # Structure chaque ligne comme un objet JSON distinct
    force_ascii=False,     # Conserve les caractères accentués en clair (non échappés)
    indent=4               # Indentation pour rendre le fichier lisible par un humain
)

```

## Le format XML (*Extensible Markup Language*) : Les données semi-structurées

Bien que plus lourd que le JSON, le format XML reste la référence dans les environnements d'entreprise, bancaires ou administratifs (factures électroniques, flux RSS, configurations).

### Lecture d'un fichier XML (`pd.read_xml`)

Pandas intègre nativement un moteur d'analyse XML (nécessitant les bibliothèques `lxml` ou `html5lib` en arrière-plan) :

```python
# Lecture d'un fichier XML en ciblant la balise répétitive (xpath)
df_factures = pd.read_xml(
    'factures.xml',
    xpath='.//facture'     # XPath ciblant chaque nœud <facture> à transformer en ligne
)

print(df_factures.head())

```

### Sauvegarde au format XML (`df.to_xml`)

Pour sérialiser un DataFrame en XML :

```python
# Exportation en XML avec personnalisation des balises racines
df_factures.to_xml(
    'factures_export.xml',
    root_name='Entreprise', # Nom de la balise racine globale
    row_name='Facture',     # Nom de la balise représentant chaque ligne
    index=False,
    encoding='utf-8'
)

```

## Accès aux données stockées en base relationnelle (`sql`)


Lecture d'une table entière depuis une base de données SQLite locale. Dans ce cas simple, on utilise un connecteur standard (`sqlite3`) pour importer directement une table relationnelle dans un `DataFrame`.

```python
import pandas as pd
import sqlite3

# Établissement de la connexion avec la base de données SQLite
connexion = sqlite3.connect('entreprise.db')

# Lecture intégrale d'une table "employes"
df_employes = pd.read_sql("SELECT * FROM employes", con=connexion)

print("--- Aperçu de la table importée ---")
print(df_employes.head())

# Fermeture de la connexion
connexion.close()

```

Exécution d'une requête filtrée via un moteur SQLAlchemy. En entreprise, on privilégie souvent **SQLAlchemy** pour s'interfacer avec divers SGBD (PostgreSQL, MySQL, SQL Server). Cela permet d'exécuter des requêtes paramétrées complexes directement depuis Pandas.

```python
import pandas as pd
from sqlalchemy import create_engine

# Création du moteur de connexion (ex: base PostgreSQL ou SQLite)
# Syntaxe : dialecte+driver://utilisateur:motdepasse@hote:port/nom_base
engine = create_engine('sqlite:///entreprise.db')

# Requête SQL ciblée (filtrage par département et tri)
requete_sql = """
    SELECT nom, departement, salaire 
    FROM employes 
    WHERE departement = 'Informatique' AND salaire > 3500
    ORDER BY salaire DESC
"""

# Importation du résultat de la requête dans un DataFrame
df_filtre = pd.read_sql(requete_sql, con=engine)

print("--- Résultats filtrés depuis la base ---")
print(df_filtre)

```

## Persistance des données via le format Pickle

Le format Pickle permet de sérialiser des objets Python, ce qui signifie que vous pouvez sauvegarder un DataFrame exactement tel qu'il est, en conservant les types de données complexes et la structure des index. Cette méthode est idéale pour stocker des données temporaires entre deux sessions de travail sans perte d'information. Le format pickle est propre à Python

```python
# Sauvegarde au format binaire Pickle
df.to_pickle('donnees.pkl')

# Chargement du fichier Pickle
df_recharge = pd.read_pickle('donnees.pkl')

```
 
## Synthèse des méthodes de persistance

| Format | Fonction de lecture | Méthode d'écriture | Cas d'usage principal |
| --- | --- | --- | --- |
| **CSV** | `pd.read_csv()` | `df.to_csv()` | Tableaux plats, exports Excel, volumétrie simple. |
| **JSON** | `pd.read_json()` | `df.to_json()` | Échanges avec des API web, données imbriquées. |
| **XML** | `pd.read_xml()` | `df.to_xml()` | Systèmes legacy, facturation électronique, flux RSS. |
| **SQL** | `pd.read_sql()` | `df.to_sql()` | Requêtage direct de SGBD (PostgreSQL, MySQL, SQLite), intégration en entreprise. |
| **Pickle** | `pd.read_pickle()` | `df.to_pickle()` | Sauvegarde binaire rapide en local en conservant fidèlement les types de données et index complexes. |
---

## Exemple de synthèse

Le cycle complet de persistance du catalogue.Ce script démontre comment sauvegarder un même DataFrame sous les trois grands formats standards du marché en respectant les bonnes pratiques (gestion des accents, suppression des index superflus, formats structurés), puis à le recharger.

```python
import pandas as pd

# Données initiales (notre catalogue fil rouge)
df_catalogue = pd.DataFrame({
    'ID_Produit': [101, 102, 103],
    'Produit': ['Clavier Mécanique', 'Souris Ergonomique', 'Casque Audio'],
    'Prix_HT': [50.0, 25.0, 80.0],
    'Disponible': [True, True, False]
})

print("--- DataFrame d'origine ---")
print(df_catalogue)

# ==========================================
# EXPORTATION MULTI-FORMATS
# ==========================================

# A. Format CSV (Idéal pour la bureautique et Excel en France)
# -> index=False évite d'exporter la colonne d'index technique (0, 1, 2)
# -> sep=';' et utf-8-sig garantissent un affichage parfait sous Excel
df_catalogue.to_csv('catalogue.csv', index=False, sep=';', encoding='utf-8-sig')

# B. Format JSON (Idéal pour le Web, les API et NoSQL)
# -> orient='records' structure chaque ligne comme un objet indépendant
df_catalogue.to_json('catalogue.json', orient='records', force_ascii=False, indent=4)

# C. Format XML (Idéal pour les échanges d'entreprise et facturation)
# -> root_name et row_name personnalisent les balises racines et de lignes
df_catalogue.to_xml('catalogue.xml', root_name='Boutique', row_name='Article', index=False)

print("\n[✔] Fichiers CSV, JSON et XML exportés avec succès sur le disque.")

# ==========================================
# RELECTURE ET VÉRIFICATION (EXEMPLE CSV)
# ==========================================
df_recharge = pd.read_csv('catalogue.csv', sep=';', encoding='utf-8-sig')

print("\n--- Données rechargées depuis le fichier CSV ---")
print(df_recharge)

```

### Les règles d'or à retenir pour votre manuel :

**Toujours utiliser `index=False`** lors des exports (`to_csv`, `to_xml`, etc.) sauf si l'index possède un sens métier avéré (comme une date ou une clé primaire explicite).

**Soigner l'encodage (`utf-8-sig` ou `utf-8`)** pour ne jamais corrompre les caractères accentués lors du passage de Python vers des fichiers tiers.

**Choisir le bon format selon l'écosystème cible** : CSV pour l'analyse tabulaire classique, JSON pour le développement web/API, et XML pour les flux d'entreprise stricts.

## Exercices de fin de chapitre
**Exercice  1 :** Exportation, encodage et vérification d'intégrité CSV / JSON

Vous développez un module d'export pour un système de gestion de stocks. Vous devez sauvegarder un catalogue de produits au format **CSV** (destiné aux équipes comptables utilisant Excel en France) et au format **JSON** (destiné à l'équipe de développement web), puis vérifier que la relecture des données conserve parfaitement les informations et les caractères accentués.

```python
import pandas as pd

# Catalogue de produits contenant des caractères accentués
data_catalogue = {
    'Code_SKU': ['SKU-001', 'SKU-002', 'SKU-003', 'SKU-004'],
    'Libelle': ['Écran incurvé 27"', 'Cafetière intégrée', 'Boîtier PC', 'Clavier rétroéclairé'],
    'Prix_TTC': [299.99, 89.50, 110.00, 45.90],
    'En_Stock': [True, False, True, True]
}

df_produits = pd.DataFrame(data_catalogue)

```

**Export CSV Excel :** Exportez le DataFrame `df_produits` dans un fichier nommé `'catalogue_export.csv'`. Veillez à :
Ignorer l'index technique de Pandas (`index=False`).
Utiliser le point-virgule comme séparateur (`sep=';'`).
Appliquer l'encodage `'utf-8-sig'` pour garantir l'affichage correct des accents sous Microsoft Excel.


**Export JSON Web :** Exportez le même DataFrame dans un fichier nommé `'catalogue_export.json'` au format liste d'enregistrements (`orient='records'`), en conservant les caractères accentués en clair (`force_ascii=False`) et avec une indentation de 4 espaces (`indent=4`).

**Vérification d'intégrité :** Rechargez le fichier CSV généré dans une nouvelle variable `df_verification` à l'aide de `pd.read_csv()` en spécifiant les bons arguments de séparateur et d'encodage. Affichez les types de données (`.dtypes`) et le contenu pour confirmer que les colonnes et les accents ont été fidèlement restitués.

---

**Exercice  2 :** Conservation des types complexes via la sérialisation Pickle

Lors des pipelines de prétraitement en data science, la conversion de types (comme les dates ou les types catégoriels) nécessite du temps de calcul. Le format CSV ne conservant pas nativement ces métadonnées (il relit tout sous forme de texte ou de nombres génériques), vous devez expérimenter la persistance binaire avec **Pickle** pour préserver l'état exact de votre DataFrame entre deux sessions.

```python
import pandas as pd

# DataFrame avec des types variés : dates, catégories, flottants et booléens
df_pipeline = pd.DataFrame({
    'Date_Commande': pd.to_datetime(['2026-11-01', '2026-11-02', '2026-11-03']),
    'Statut': pd.Series(['Expédié', 'En attente', 'Livré'], dtype='category'),
    'Montant_EUR': [125.50, 42.00, 310.80],
    'Prioritaire': [True, False, True]
})

```

Inspectez et affichez les types initiaux des colonnes du DataFrame `df_pipeline` avec l'attribut `.dtypes`.

Sauvegardez ce DataFrame sur le disque au format binaire Pickle sous le nom `'session_pipeline.pkl'` à l'aide de la méthode `.to_pickle()`.

Rechargez le fichier sauvegardé dans une nouvelle variable `df_restaure` en utilisant `pd.read_pickle()`.

Comparez les types (`.dtypes`) de `df_restaure` avec ceux de `df_pipeline`. Vérifiez notamment que la colonne `'Date_Commande'` est restée en `datetime64[ns]` et la colonne `'Statut'` en `category` sans qu'aucune re-conversion manuelle ne soit nécessaire.
