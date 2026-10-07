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

## Key findings

- **Dry legumes and soy flour are the clear winners.** Red lentils, green lentils, and soy granules (VegeSun) deliver protein at €1–3 per 100g — 5 to 10× cheaper than processed meat substitutes.
- **Meat substitutes are expensive per gram of protein.** Most fall in the €6–15 range; some (e.g. Kasvislauantai) exceed €20. You're paying for convenience and texture, not nutrition.
- **Peanut butter punches above its weight.** At ~€3/100g protein it sits close to dry legumes, and it's ready to eat with no prep. One of the best value-to-effort ratios in the dataset.
- **Wholegrain bread is a solid everyday contributor.** Not protein-dense enough to compete with legumes, but at €3–4/100g protein and zero cooking required it's a reliable background source.
- **Nuts cluster in the middle.** Peanuts and peanut butter are outliers on the cheap end; walnuts and cashews are pricier for the protein they provide.
- **Protein supplements beat everything on pure economics** — but as food they're in a different category from whole products.
- **Tofu and tempeh are surprisingly mid-range.** Not the cheapest, but competitive once you factor in culinary versatility.

---

## Data

**Source:** Prices and nutritional values collected manually from Finnish supermarket product pages (S-kaupat, K-ruoka, Lidl) and product labels.

**Coverage:** 50+ products across 9 categories (Legumes, Cereals & granola, Soy products, Meat substitutes, Nuts & seeds, Dairy substitutes, Grains, Bread, Protein supplements).

**Caveats:** Prices reflect shelf prices at the time of collection and may vary by store, date, and promotion. Rows without a price were identified but not yet priced. Always check the product label for current nutritional values.

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

- [x] Add `store` and `date_observed` columns to the CSV for data provenance
- [x] Show product links in the product table
- [x] Add category-level average markers to the chart
- [ ] Fill in missing prices for unpriced rows (K-ruoka, Lidl products)
- [ ] Improve chart readability: label Pareto-optimal products, size dots by value score, add value frontier line
- [ ] Add carbs and fat columns to the CSV and explore a multi-macro view (e.g. ternary plot or parallel coordinates)
- [ ] Expand coverage: more stores (Lidl, K-ruoka own brands), more categories (protein bars, ready meals)

---

## Contributing

Data contributions are welcome. Open an issue or a pull request.

---

## License

Code: [MIT](LICENSE). Data: [CC BY 4.0](data/LICENSE).
