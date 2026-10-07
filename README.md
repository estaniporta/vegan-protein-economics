# Vegan Protein per Euro (Finnish products)

Which plant-based protein sources offer the best value in Finnish supermarkets, comparing protein density against cost?

🔗 **[View the chart](https://estaniporta.github.io/vegan-protein-economics/)**

---

## How to read the chart

- **X axis** — grams of protein per 100g of product (higher = more protein-dense)
- **Y axis** — euros per 100g of protein (lower = cheaper)
- **Bottom-right quadrant** — high protein density and low cost: the best value zone
- Click a category in the legend to show or hide it; double-click to isolate it

---

## Data

**Source:** Prices and nutritional values collected manually from Finnish supermarket product pages (primarily [s-kaupat.fi](https://www.s-kaupat.fi)) and product labels.

**Coverage:** 37 products across 9 categories (Legumes, Cereals & granola, Soy products, Meat substitutes, Nuts & seeds, Dairy substitutes, Grains, Bread, Protein supplements).

**Caveats:** Prices reflect shelf prices at the time of collection and may vary by store, date, and promotion. Always check the product label for current nutritional values.

**Licence:** [CC BY 4.0](data/LICENSE) — free to reuse with attribution.

---

## Method

`generate_chart.py` reads `data/products.csv` and computes:

- **Price per 100g protein** (euros): recalculated from `price_per_kg` and `protein_per_100g` — the CSV column is not used directly.
- **Value score** (0-100): equally weights protein density and cost efficiency, both normalised to [0, 1].

The script then builds an interactive Plotly scatter plot and a sortable DataTables product table, and writes everything to `docs/index.html` for GitHub Pages.

---

## How to reproduce

```bash
pip install -r requirements.txt
python generate_chart.py
```

This overwrites `docs/index.html`. Commit and push to update the live site.

For a smaller file that loads Plotly from CDN:

```bash
python generate_chart.py --deploy
```

**GitHub Pages setup (forks):** go to Settings → Pages → Source → Deploy from branch → `main` / `docs`.

---

## Project structure

```
vegan-protein-economics/
├── data/
│   ├── products.csv      # product database
│   └── LICENSE           # CC BY 4.0 for the data
├── docs/
│   └── index.html        # generated chart (served by GitHub Pages)
├── generate_chart.py     # builds the chart from the CSV
├── requirements.txt      # Python dependencies
└── README.md
```

---

## Adding a product

Open `data/products.csv` and add a row:

```
product;category;package_size_g;protein_in_package_g;price_per_kg_eur;protein_per_100g;price_per_100g_protein;url
```

The `price_per_100g_protein` column is recomputed by the script, so it can be left as a placeholder. The `url` column is optional.

**Categories in use:** Legumes, Cereals & granola, Soy products, Meat substitutes, Nuts & seeds, Dairy substitutes, Grains, Bread, Protein supplements

---

## Limitations

- Data covers Finnish supermarkets only, primarily one retail chain (S-kaupat).
- Prices are point-in-time and not dated at the row level.
- 19 of 37 products have no store URL.
- One known data quality issue in the CSV (Impolan lentils row: `protein_per_100g` appears to be a copy of `protein_in_package_g`). The script recomputes this value from the other columns, so the chart is unaffected, but the raw CSV is incorrect.

---

## TODO

- [ ] Add `store` and `date_observed` columns to the CSV for data provenance
- [ ] Show product links in hover tooltips
- [ ] Add category-level average markers to the chart

---

## Contributing

Data contributions are welcome. Open an issue or a pull request.

---

## License

Code: [MIT](LICENSE). Data: [CC BY 4.0](data/LICENSE).
