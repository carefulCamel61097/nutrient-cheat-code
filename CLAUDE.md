# Nutrients Tool

A tool for finding efficient combinations of nutrient-dense foods that cover most daily nutrient requirements in a single small meal.

## The idea

Eat a tiny, nutrient-dense set of foods once a day (typically morning) to cover the bulk of vitamins, minerals, fiber, and omega-3 — then eat whatever you want the rest of the day without guilt. The tool's job is to:

1. Let the user assemble a candidate combination of foods (with portion sizes).
2. Show which nutrient daily targets that combination covers, and which are gaps.
3. Help discover small additions/swaps that close the biggest gaps with the least extra eating.

The optimization target is **coverage per gram** (or per "unit of effort to eat"), not taste, variety, or meal appeal.

## Baseline reference combination (sanity check)

The tool itself is general-purpose — not tailored to any specific user. As a sanity check the tool's food list should be capable of reproducing the user's current morning stack:

- Some kale
- Half a red bell pepper
- Some frozen blueberries
- Some chia seeds
- Some pumpkin seeds
- One Brazil nut
- A few walnuts

## Terminology

**Nutrients** — the umbrella. Anything in food the body uses. Splits into two groups by how much you need:

**Macronutrients ("macros")** — needed in large amounts (grams/day). They provide energy (calories):
- **Protein** — building/repair, ~4 cal/g
- **Carbohydrates** — main energy source, ~4 cal/g (includes fiber, which is a carb the body doesn't digest for energy but still matters)
- **Fats** — energy + hormone/cell function, ~9 cal/g
- *(Water is sometimes counted here too — needed in large amounts, no calories.)*

**Micronutrients** — needed in tiny amounts (mg or μg/day). No calories, but essential. Two subgroups:
- **Vitamins** — *organic* compounds (carbon-based, made by living things). Examples: A, C, D, E, K, the B-vitamins. Split into **fat-soluble** (A, D, E, K — stored in body fat) and **water-soluble** (C, Bs — excreted, need regular intake).
- **Minerals** — *inorganic* elements (from rocks/soil, taken up by plants/animals). Examples: iron, calcium, zinc, magnesium, selenium, potassium. Split into **major minerals** (>100 mg/day, e.g. calcium, magnesium) and **trace minerals** (<100 mg/day, e.g. iron, zinc, selenium — one Brazil nut covers selenium for the day).

Hierarchy:

```
Nutrients
├── Macronutrients (energy)
│   ├── Protein
│   ├── Carbs (incl. fiber)
│   └── Fats
└── Micronutrients (no energy, essential)
    ├── Vitamins (organic)
    └── Minerals (inorganic)
```

**Often grouped with nutrients but technically separate:**
- **Phytonutrients / phytochemicals** — plant compounds (e.g. anthocyanins in blueberries, sulforaphane in kale) that aren't strictly "essential" but have health benefits. Not on official RDA charts but worth tracking as "bonus" scoring.
- **Omega-3 / omega-6** — *essential fatty acids*, technically a subset of fats but tracked separately because most people are deficient in omega-3 (walnuts cover this).

## Nutrients to track

~30 daily targets, grouped:

- **Macros:** protein, carbs, fiber, fat
- **Vitamins (~13):** A, C, D, E, K, B1 (thiamine), B2 (riboflavin), B3 (niacin), B5, B6, B7 (biotin), B9 (folate), B12
- **Minerals (~15):** calcium, iron, magnesium, phosphorus, potassium, sodium, zinc, copper, manganese, selenium, iodine, chromium, molybdenum, fluoride, chloride
- **Essential fatty acids:** omega-3 (ALA, EPA, DHA), omega-6
- *(Optional: phytonutrients like anthocyanins, sulforaphane — useful for "bonus" scoring but not on RDA charts)*

## Food clustering approach

Nutrients are not independent — they ride along with each other in real foods. Rather than picking the top-N foods per nutrient (which produces ~300 candidates with massive overlap), the tool organizes **at least 5 efficient foods per cluster**, plus a handful of "specialist" picks for nutrients that don't cluster.

| Cluster | Nutrients it primarily covers |
|---|---|
| Leafy greens (kale, spinach, chard) | Vitamin K, A (β-carotene), folate, some C, magnesium, fiber |
| Colorful fruit/veg (bell pepper, berries, citrus, tomato) | Vitamin C, A, phytonutrients |
| Nuts (almonds, walnuts) | Vitamin E, magnesium, omega-3 ALA (walnuts), copper, manganese |
| Seeds (chia, pumpkin, sunflower, flax) | Magnesium, zinc, iron, calcium, fiber, omega-3 ALA |
| Legumes / whole grains | B1, B6, folate, fiber, iron (non-heme), magnesium, protein |
| Dairy / fortified plant milk | Calcium, B12 (if fortified), B2, vitamin D (if fortified), iodine, protein |
| Eggs | Choline, B12, B2, selenium, vitamin D, protein |
| Fatty fish (salmon, sardines, mackerel) | Omega-3 EPA/DHA, vitamin D, B12, selenium, iodine |

**Specialist picks** for loner nutrients that don't cluster:

| Nutrient | Specialist food |
|---|---|
| Selenium | Brazil nut (one nut ≈ a full day's DV; UL hit fast) |
| Iodine | Iodized salt, seaweed/nori, dairy |
| Vitamin K2 | Natto, hard cheese, egg yolk (distinct from K1 in greens) |
| Vitamin D | Sunlight, fatty fish, fortified milk |
| B12 | Animal products or fortified foods (no plant has it) |

Target: at least 5 efficient foods per cluster + the specialists. ~40 foods total in the v1 data set.

## Daily intake targets (decided)

- **Primary target per nutrient:** [FDA Daily Values (DV)](https://www.fda.gov/food/nutrition-facts-label/daily-value-nutrition-and-supplement-facts-labels) — one number per nutrient, the same set used on US nutrition labels. Trivial to encode and pairs cleanly with USDA FoodData Central.
- **Upper limits:** [NIH ODS / IOM Dietary Reference Intakes (DRI) Tolerable Upper Intake Levels (UL)](https://www.ncbi.nlm.nih.gov/books/NBK545442/table/appJ_tab9/) for the handful of nutrients where overdosing is a real risk: **selenium** (Brazil nuts!), **vitamin A** (preformed, not β-carotene), **iron**, **zinc**, **vitamin D**, **vitamin B6**, **niacin**, **folate (synthetic)**.
- **Food nutrient data source:** [USDA FoodData Central](https://fdc.nal.usda.gov/) — public, comprehensive, free, standardized per 100g.

If/when the user wants more precision (age/sex-specific), upgrade to full DRIs (RDA + AI + EAR).

## v1 scope (decided)

- **Architecture:** single self-contained HTML file (`index.html`) — HTML + inline CSS + inline JS + inline JSON food data. No build step, no framework. Open by double-click; later drop on GitHub Pages unchanged.
- **Responsive layout** so the same file works on a phone browser (eases the later Flutter decision — the HTML may turn out to be enough).
- **Foods:** ~40 hand-curated entries grouped by cluster, with USDA per-100g nutrient values. Each food declares 3-4 **per-food portion presets** (e.g. Brazil nut: `1 nut (5g)` / `2 nuts (10g)`; kale: `small handful (15g)` / `30g` / `100g`).
- **Selection:** checkbox + portion preset dropdown per food. Changing the portion dropdown implies the food is selected.
- **Bars:** per-nutrient horizontal bars showing % of DV, with tick marks at DV (always at 60% of track) and UL (right edge, where applicable). Color coding (DV has built-in safety margins, so 80% is "good enough"):
    - <40% — muted red `#a14545`
    - 40-79% — amber `#c9a227`
    - 80% and above — green `#3fa34d`
    - over UL — sharp red `#d63b3b`
- **View toggle:** `[Common gaps]` / `[All nutrients]` at top. *Common gaps* (default) shows the 15 nutrients a limited-variety diet (no greens / picky-eater profile) is likely to miss (`gap: true` flag in `NUTRIENTS`). *All nutrients* shows all 19. Heading shows the count (e.g. `Common gaps · 15 of 19`) so the subset is explicit. The 4 hidden by default — protein, vit A, zinc, selenium — are covered easily by any basic meat-bread-dairy diet. Per-cluster view was removed; clusters live on as the food-list grouping but no longer get a bar view.
- **Untracked nutrients:** user can mark nutrients as "don't track" — they grey out (opacity ~0.4) and demote to the bottom of the list. Not hidden, still computed.
- **Calorie + total grams readout** at the top.
- **localStorage persistence** of selections, portion choices, untracked nutrients, and view mode.

**Daily baseline section** (added 2026-05-01) — separate top section with a dropdown picker of common everyday foods (rice, chicken, bread, etc.). Selected foods appear as rows with the same checkbox + portion controls plus a × to remove. Their nutrient contributions sum into the same bars as cluster foods, so the bars represent total daily coverage and the cluster section shows what gaps remain to be filled by the curated power foods.

**Punted to v2+:** URL-shareable combos, phytonutrient bonus scoring, multiple meals/day, food search (the picker is sufficient for v1), USDA API integration, age/sex-specific DRI targets.

## Visual theme: lab dashboard

Anti-wellness aesthetic. The tool is a diagnostic instrument for the body, not a wellness coach. No green leaves, no smiling vegetables, no encouragement copy.

| Element | Choice |
|---|---|
| Background | Dark charcoal (`#0e1116` / panel `#1a1b1e`) |
| Text | Off-white labels (`#e6e6e6`), dim secondary (`#8a8f98`) |
| Numbers | Monospace (JetBrains Mono / IBM Plex Mono / system mono) |
| Labels/UI | Clean sans (Inter / system-ui) |
| Bar style | Slim horizontal bars like an audio level meter, with tick marks at DV and UL |
| Accent | One signal color (cyan `#4ec9d6`) for active selections — never decorative |
| Imagery | None. Foods are text. |
| Copy tone | Factual, terse. *"Selenium: 142% DV — exceeds UL at this portion."* Not *"Great selenium coverage!"* |

Mental model: car dashboard, mixing console, htop. Not a recipe app.

## Project status

- **2026-05-01** — Initial scaffold + multiple iterations:
  - Nutrients: 19 (3 macros + 9 vitamins + 7 minerals). 12 flagged as `gap: true` (the Common gaps subset).
  - Cluster foods: 28 total (5–6 per cluster across leafy greens, colorful, nuts, seeds, specialists). Specialists cover loner nutrients: brazil nut (selenium), nori (iodine), nutritional yeast (B12), natto (K2 via vitK), beef liver (vitamin A — UL risk).
  - Common (baseline) foods: 25 total — rice (white/brown), chicken, beef, pork, egg, bread, banana, apple, Greek yogurt, salmon, tofu, cheddar, milk, olive oil, plus sunlight (non-food, `hideGrams: true`), potato, sweet potato, pasta, oatmeal, lentils, chickpeas, canned tuna, shrimp, avocado. Accessible via picker.
  - Daily baseline section with picker dropdown + check-all/uncheck-all toggle.
  - Bars use piecewise scaling (DV always at 60% of track, UL at right edge for UL nutrients). Color thresholds: <40% red, 40-79% amber, ≥80% green, over UL bright red.
  - Two view modes: Common gaps (default), All nutrients. Per-cluster view removed.
  - Hover any food *name* → bars show single-food preview (checkbox/dropdown don't fire it). Heading reads `Preview · {Food} ({portion})`. Bars panel is sticky on scroll with themed scrollbar.
  - Hover any *bar* (nutrient row) → tooltip lists top 5 foods by per-100g density of that nutrient. Reverse-lookup for "where does this nutrient come from?".
  - **Specialty tags**: foods show their dominant nutrient(s) in a dim parenthetical, e.g. `Kale (Vit K)`, `Beef liver (B12, Vit A)`. Threshold ≥40% DV at default portion (matches color coding), up to 3 tags per food, sorted by % desc.
  - Specialist additions: sardines (B12, calcium-with-bones, omega-3, D), oysters (zinc king, B12, iron), kombu (extreme iodine — used in pinches).
  - Blueberries moved from cluster to baseline picker — phytonutrient-rich but doesn't drive any tracked nutrient.
- **Values are still approximate** — sourced from training-data-recall of USDA values, not pixel-verified against the live USDA FoodData Central database. Final verification pass is the remaining curation work.
- **Next:** verify all per-100g values against live USDA, optionally add new clusters (legumes/grains, dairy, eggs, fatty fish) if the current cluster organization feels incomplete.

### Subsequent iterations (2026-05-01, post-deploy)

- **`FIRST_LOAD_DEFAULTS`** constant near the top of the script — list of food ids auto-selected on a fresh load (no localStorage). Anyone forking can edit. Currently seeds: kale + pepper + chia + pumpkin seeds + sunflower seeds + brazil nut (cluster), plus white rice + chicken breast + whole-wheat bread + sunlight (baseline). Demonstrates the tool with a populated dashboard for first-time visitors.
- **Walnut moved to baseline picker** — chia covers omega-3 ALA more efficiently. Walnut still available via picker for users who want it.
- **Cultural-coverage additions in baseline picker**: black beans (Latin America/Caribbean), peanuts (Africa/SE Asia/global), coconut milk (SE Asia/South India).
- **Tooltip rows are clickable** — hovering a nutrient name shows top 5 foods + clicking one auto-adds (baseline) and selects (cluster or baseline) that food. A 250ms grace period on hide lets the cursor traverse from the label into the tooltip without dismissal. "Selected" rows show greyed/non-interactive.
- **Repo deployed** at <https://github.com/carefulCamel61097/nutrient-cheat-code>, hosted at <https://carefulcamel61097.github.io/nutrient-cheat-code/>.

## Working notes for Claude

- Frame suggestions around **efficiency and minimal effort**, not enjoyment or meal planning. The user is not trying to enjoy the healthy portion — he wants to get it over with fast.
- When suggesting foods, prefer ones that are dense in *multiple* under-covered nutrients at once (high "marginal coverage per gram").
- Don't assume cooking. Raw / frozen / no-prep foods are preferred unless explicitly opened up.
- Tool is general-purpose — do not hard-code anything specific to the user's stack.
