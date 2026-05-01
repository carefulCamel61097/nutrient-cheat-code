# Nutrient Cheat Code

**[→ Open the tool](https://carefulcamel61097.github.io/nutrient-cheat-code/)**

Find a tiny set of nutrient-dense foods that covers most of your daily nutrient targets in one small meal — so the rest of the day is yours.

> Eat the right small set of foods once a day. See which nutrients you cover and which are gaps. Eat whatever you want for the rest of the day.

## How to use it

1. **Add what you eat anyway.** In the *Daily baseline* section at the top, use the picker to add common foods you already eat on a typical day — rice, chicken, eggs, bread, yogurt, fruit, etc. Pick a portion size from the dropdown next to each one. (Sunlight is in there too, as a non-food vitamin D contributor.)

2. **Watch the bars.** The horizontal bars on the right show how close you are to your daily target for each nutrient. The tick mark is 100% (the FDA Daily Value). Where it matters, a second tick on the right marks the upper safe limit.
   - **Red** — under 40% of target
   - **Amber** — 40–79%
   - **Green** — 80% or more (good enough — DVs already include a safety margin)
   - **Bright red** — over the upper limit (only relevant for a few nutrients like selenium, vitamin A, iron)

3. **Fill the gaps with power foods.** Below the baseline, the curated *power foods* are grouped into clusters (leafy greens, colorful fruit/veg, nuts, seeds, specialists). Tick the ones you'd be willing to eat in the morning. Each food shows its dominant nutrient(s) in parentheses — `Kale (Vit K)`, `Brazil nut (Selenium)` — so you can see at a glance what each one buys you.

4. **Investigate.**
   - **Hover a food name** to preview that food's contribution in isolation — the bars temporarily show only what *it* brings.
   - **Hover a nutrient name** for a tooltip ranking the top 5 foods for that nutrient. Click any row in the tooltip to add that food directly. Useful when you have one stubborn gap and want to know your options.

5. **Adjust the view.**
   - The default view is *Common gaps* — 15 nutrients a limited-variety diet typically misses. Toggle to *All nutrients* to see all 19 (adds protein, vitamin A, zinc, selenium — the ones a basic meat-bread-dairy diet already covers).
   - **Don't care about a nutrient?** Click it to untrack — it greys out and demotes to the bottom of the list. Still computed, just out of the way.

Your selections are saved in your browser's `localStorage`, so the dashboard is still there when you come back.

## What it tracks

19 nutrients: 3 macros (protein, fiber, fat) + 9 vitamins (A, C, D, E, K, B6, B9, B12, plus a couple of B's) + 7 minerals (calcium, iron, magnesium, potassium, zinc, selenium, iodine).

Targets come from [FDA Daily Values](https://www.fda.gov/food/nutrition-facts-label/daily-value-nutrition-and-supplement-facts-labels). Upper limits (where overdose is a real risk — selenium, vitamin A, iron, zinc, vitamin D, B6, niacin, folate) come from [IOM Dietary Reference Intakes](https://www.ncbi.nlm.nih.gov/books/NBK545442/).

## Caveat — values are approximate

The per-100g food numbers were assembled from training-data recall, not direct queries to [USDA FoodData Central](https://fdc.nal.usda.gov/). They're roughly right (back-of-the-label level) but not verified. Some nutrients (nori iodine, nutritional yeast B12 fortification) vary so much by brand/species that *any* single number is approximate.

**Don't use this for medical or therapeutic dosing.** It's for getting a rough sense of where your daily eating sits, not clinical work.

## Run it locally

It's a single self-contained HTML file. No build step, no framework, no network calls.

- **Open `index.html` in any browser.** That's it.
- To deploy your own copy on GitHub Pages: fork or clone, push to your repo, then **Settings → Pages → Deploy from branch → main → / (root) → Save**.

## Customizing

All data lives in three arrays at the top of the `<script>` block in `index.html`:

- `NUTRIENTS` — id, label, unit, DV, UL, gap flag
- `CLUSTERS` — groupings for curated power foods
- `FOODS` — per-100g values, portion presets, default portion, category (`cluster` or `common`)

Edit directly and refresh — no rebuild. To add a non-food contributor like sunlight, set `hideGrams: true` so the portion renders as a label (e.g. `20 min`) instead of a gram weight.

If you spot a clearly-wrong value, edit the `FOODS` entry directly. PRs welcome.

## Privacy

100% client-side. No tracking, no analytics, no network requests. Selections live in your browser's `localStorage` and never leave your machine.

## License

MIT.
