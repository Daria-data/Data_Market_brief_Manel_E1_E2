
# Data Market — Sales Data Mart (Product Marketing)

## 1. Objectif
Prototype d’une vitrine données pour le **Product Marketing** :
- centraliser ventes / produits / paiements / avis / géolocalisation,
- nettoyer et harmoniser,
- produire un **mart** au niveau ligne de commande pour analyses (lancements, saisonnalité, clusters, géo).

## 2. Structure du projet

```

data\_market1/
├─ data/
│  ├─ raw/        # CSV sources (olist\_\*.csv)
│  └─ clean/      # sorties nettoyées + mart
├─ notebooks/
│  └─ market.ipynb  # notebook principal (collecte, nettoyage, mart)
├─ src/             # (optionnel) scripts futurs
├─ pyproject.toml   # dépendances gérées par uv
└─ README.md

```

## 3. Données attendues (placer dans `data/raw/`)

- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_customers_dataset.csv`
- `olist_products_dataset.csv`
- `product_category_name_translation.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`
- `olist_sellers_dataset.csv` 
- `olist_geolocation_dataset.csv`

## 4. Environnement

- Python 3.11+
- Gestion des dépendances : **uv**
- Packages principaux : `pandas`, `numpy`, `jupyter`

### Installation 

```bash
uv sync             # installe les dépendances du pyproject.toml
```

## 5. Exécution

1. Ouvrir `notebooks/market.ipynb`.
2. Vérifier les chemins en tête de notebook :

   * `RAW = <repo>/data/raw`
   * `CLEAN = <repo>/data/clean`

3. Exécuter séquentiellement les cellules :

   * **Chargement & profiling** des datasets
   * **Nettoyage minimal** (dates, types, traductions catégories)
   * **Checks de jointure** (Vérification que toutes les clés des tables trouvent leur correspondance (orders, items, products, payments, reviews))
   * **Mart prototype** (niveau *order line*)

## 6. Sorties (dans `data/clean/`)

* `data_profile_overview.csv` – profil rapide des jeux de données
    - `orders_clean.csv`, 
    - `order_items_clean.csv`, 
    - `products_clean.csv`,
    - `customers_clean.csv`, 
    - `payments_clean.csv`, 
    - `reviews_clean.csv`

* `sales_mart_marketing_prototype.csv` – mart v1 (ligne de commande + produit + dates + revenue)

## 7. Logique (résumé)

* Normalisation des dates (`orders`, `reviews`)
* Types numériques (`price`, `freight_value`, `payment_value`)
* Traduction catégories via `product_category_name_translation`
* Enrichissement géo (zip → city/state) basique
* Mart v1 = `order_items` ⟂ `orders` ⟂ `products` (+ métrique `revenue = price + freight`)


## 8. Next steps / Pour aller plus loin (TBD)

* **Automatisation** : script CLI (`uv run python -m src.build_mart`) ou Makefile
* **Orchestration** : tâche planifiée (cron) / workflow léger
* **Qualité** : tests de contraintes (unicité clés, référentiel catégories)
* **Géo** : normalisation régions + cartes
* **Analytique** : agrégats hebdo/mensuels, market basket, post-promo lift
* **Packaging** : Docker + compose (environnement reproductible)
* **BI** : connecteurs vers Power BI / Tableau


