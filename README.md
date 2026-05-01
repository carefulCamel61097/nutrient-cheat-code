# Nutrient Cheat Code

Find efficient combinations of nutrient-dense foods that cover most of your daily nutrient targets in a single small meal — so the rest of the day is yours.

> Eat the right tiny set of foods once a day. Track which nutrients you cover and which are gaps. Eat whatever you want for the rest of the day.

A single self-contained HTML file. Works offline. No build step, no framework, no network.

## Run it

**Local**: Open `index.html` in any modern browser. That's it.

**Hosted**: <https://carefulcamel61097.github.io/nutrient-cheat-code/>

To deploy your own copy on GitHub Pages: fork or clone, push to your repo, then **Settings → Pages → Deploy from branch → main → / (root) → Save**. Site goes live in a minute or two.

## What it does

You assemble a combination of foods and pick portion sizes. The tool calculates which nutrient daily targets that combination covers and which are gaps. It visualizes coverage as horizontal bars with target ticks (DV) and ceiling ticks (UL — for nutrients where overdose is a real risk).

The food list is split:

- **Daily baseline** — pick from a dropdown of common everyday foods (rice, chicken, eggs, bread, etc.) plus sunlight as a non-food vitamin D contributor. These represent what you eat *anyway*.
- **Curated power foods** — efficient, nutrient-dense picks grouped into clusters (leafy greens, colorful fruit/veg, nuts, seeds, specialists). These are what you'd add as the morning "cheat code" meal to fill the gaps your baseline misses.

## Features

- **19 tracked nutrients** — 3 macros + 9 vitamins + 7 minerals — based on FDA Daily Values and IOM Tolerable Upper Intake Limits where overdose risk is real.
- **~33 hand-curated foods** with per-100g nutrient data and per-food portion presets (`1 nut (5g)`, `handful (15g)`, `1 sheet (3g)`, `20 min` for sunlight, etc.).
- **Two views**: Common gaps (default — 15 nutrients a limited-variety diet typically misses) and All nutrients. Untrack any nutrient to demote it to the bottom (greyed but still computed).
- **Specialty tags** next to each food name — `Kale (Vit K)`, `Brazil nut (Selenium)`, `Beef liver (B12, Vit A)` — at ≥40% DV per default portion, top three.
- **Hover-isolate**: hover any food name and the bars temporarily show only that food's contribution. Useful for understanding what each food brings.
- **Reverse lookup**: hover any nutrient name and a tooltip ranks the top 5 foods by % DV at default portion. Useful for filling a specific gap.
- **Bar coloring**: <40% red · 40–79% amber · ≥80% green · over UL bright red.
- **Daily baseline** with picker dropdown plus check-all/uncheck-all toggle.
- **localStorage persistence** for selections, untracks, view, and baseline foods.
- **Sticky bars panel** with themed scrollbar — bars stay visible while scrolling the food list.
- **Lab dashboard theme** — dark, monospace numbers, no marketing copy.
- **Single self-contained HTML file** — no build, no framework, no network requests.

## Data sources (caveat: not directly queried)

The numbers in this tool — daily values, upper limits, per-100g food contents — were assembled from an LLM's training-data recall, not from live database lookups. They're *approximately right* (the kind of values you'd get if you read off the back of a nutrition label) but they have not been verified against the canonical sources. Treat them as v1 placeholders.

These are the canonical sources to verify against, and the right targets for a future scripted import:

| Source | What it covers |
|---|---|
| [FDA Daily Values](https://www.fda.gov/food/nutrition-facts-label/daily-value-nutrition-and-supplement-facts-labels) | Daily target per nutrient (the bar's 100% mark) |
| [IOM Dietary Reference Intakes — Upper Limits](https://www.ncbi.nlm.nih.gov/books/NBK545442/) | Tolerable upper intake (the bar's red ceiling) |
| [USDA FoodData Central](https://fdc.nal.usda.gov/) | Per-100g food nutrient values |

Of the three, the FDA DVs and IOM ULs are well-established constants and should be close to correct as encoded. The per-100g food values are the ones most likely to be off — and some (nori iodine, nutritional yeast B12 fortification) vary so much by species/brand/source that *any* single number is inherently approximate.

**Don't rely on this tool for medical or therapeutic dosing.** It's for getting a rough sense of where your daily eating sits relative to standard targets, not for clinical use.

If you spot a value that's visibly off, edit it directly in the `FOODS` array in `index.html` and reload — no rebuild needed.

## On "Common gaps"

The default view is a curated subset, not the full nutrient list. It targets nutrients a limited-variety diet (no greens, picky-eater profile) typically misses — *not* the medically most-essential nutrients (all 19 tracked nutrients are essential by the strict biological definition). Vitamin A, zinc, selenium, and protein are hidden by default because a basic meat-bread-dairy diet covers them easily. Toggle to "All nutrients" to see them.

## Customizing the food list

All data lives in three arrays at the top of the `<script>` block in `index.html`:

- `NUTRIENTS` — id, label, unit, DV, UL, gap flag
- `CLUSTERS` — food groupings for the curated power foods
- `FOODS` — per-100g nutrient values, portion presets, default portion, category (`cluster` or `common`), optional `cluster` ID and `hideGrams` flag

Add or edit entries directly. The UI reads from these on load. No build step — just save and refresh.

To add a non-food contributor (like sunlight), set `hideGrams: true` so the portion shows as a label (e.g., `20 min`) and the gram total skips it.

## Privacy

100% client-side. No tracking, no analytics, no network requests. Selections are stored in your browser's `localStorage` and never leave your machine.

## Project layout

```
index.html       Tool — single self-contained file. HTML + inline CSS + inline JS + inline JSON data.
README.md        This file.
CLAUDE.md        Project context for AI-assisted development.
```

## License

MIT — use as you wish.
