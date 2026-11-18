# Nutrition Tracker

A single-page nutrition tracker. You set calorie and macro targets, log foods by the gram, and watch more than 60 nutrients (vitamins, minerals, amino acids and fatty acids) fill in against your personal targets as you go.

**Live site:** https://marcusdenice.github.io/food-tracker/

## Features

- **Personal targets.** Calories are estimated from sex, age, weight, height and activity level (the Mifflin-St Jeor equation). You then adjust the protein, carb and fat split by percentage.
- **Nutrient targets that fit you.** Vitamins and minerals follow NIH adult RDAs by sex and age. Fiber and B1/B2/B3 scale with calories. Essential amino acids use WHO/FAO/UNU requirements based on body weight. Limits are shown for sodium, saturated fat, trans fat, sugar and cholesterol.
- **Editable targets.** You can click any target number to override it, and reset it later.
- **Planning across days.** You can multiply every target by up to 10 days to build a shopping list.
- **Hover details.** Hovering over a nutrient shows what it does, which logged foods provided it, and general food types that are good sources.
- **Custom foods.** You can add foods from a nutrition label. Amounts are converted to per-100 g automatically.
- **Renamable categories.** Food sections can be renamed or hidden. For example, you could use Breakfast, Lunch and Dinner instead of food types.
- **Export and restore.** You can download your log as JSON, CSV or text, and re-upload a JSON or CSV file later to restore it.

## Files

```
index.html   the whole app (layout, styles and code in one file)
foods.csv    the food database the app loads at startup
```

## The food database (`foods.csv`)

`foods.csv` is built from the USDA **SR Legacy** dataset, with a few custom additions. SR Legacy was released in April 2018 as the final version of that dataset, and USDA won't update it again. A fixed local copy therefore doesn't fall behind the official source.

Each row is one food, and every nutrient value is **per 100 g**:

| Column | Meaning |
|---|---|
| `id` | USDA FDC ID. Custom foods without an ID are automatically given numbers from 900000000 up. |
| `name` | Food name, which is what the search box matches against |
| `group` | Original USDA food category (for reference) |
| `calories_kcal`, `protein_g`, `fat_g`, … | One column per nutrient. The suffix is the unit (`_g`, `_mg`, `_ug` = mcg). |

**To add or edit foods:** open `foods.csv` in any spreadsheet app, add or change rows, save it as CSV, and replace the file in the repo. Blank nutrient cells are treated as 0, so you only need to fill in what you know. The app also has a **Download Current Food Database** button that exports the file in the correct format.

### Why a CSV file instead of the USDA API?

- **Fast searching.** The whole database loads once when the page opens. After that, each search runs in under a millisecond in the browser. An API would need a network request for every search, plus separate requests to get the full nutrient details for each food.
- **No API key in the code.** USDA's FoodData Central API requires a key and deactivates any key found posted publicly, for example in a code repository. A static site on GitHub Pages has no safe place to keep one. The API is also limited to 1,000 requests per hour per IP address.
- **Easy to share and edit.** Anyone can open the file in a spreadsheet, add their own foods and use a copy without signing up for anything. The site keeps working even if the USDA API changes or goes down.
- **Trade-off:** the first page load downloads the whole file (a few MB, compressed in transit by GitHub Pages) and takes a fraction of a second to read. With about 8,000 rows, that's a one-time cost.

## Running it

The page loads `foods.csv` with `fetch()`, so it has to be served over http/https. **Opening `index.html` directly from your computer won't work.**

- **GitHub Pages:** In the repo, go to Settings → Pages and serve from the main branch's root folder. Keep `foods.csv` next to `index.html`.

## Your data

Everything stays in your own browser. Your profile, targets, custom foods and food log are saved in localStorage, and nothing is sent to a server. Clearing your browser data erases them, so use the export buttons to keep backups.

## Notes

- This is a planning tool, not medical advice. Talk to a doctor or dietitian about specific needs, especially kidney, thyroid or other conditions where potassium or phosphorus intake matters.
- Iodine isn't tracked. Insoluble and soluble fiber aren't measured in SR Legacy, so those rows show 0.
- This project was built with the help of AI tools.

## Data source

U.S. Department of Agriculture, Agricultural Research Service. *USDA National Nutrient Database for Standard Reference, Legacy Release* (2018), via [FoodData Central](https://fdc.nal.usda.gov/).
