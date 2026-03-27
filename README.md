# DataStory — CSV Analyzer (`CSV_Analyzer.html`)

A browser-only **CSV storyteller**: upload a file, get **summary stats**, **Chart.js** visualizations, an **AI-written narrative**, and six **insight cards**. The UI brands the app as **DataStory**.

## What it does

1. **Parse** the CSV with [Papa Parse](https://www.papaparse.com/) (header row, skipped empty lines, basic dynamic typing).
2. **Classify columns** using the first data row: numbers vs strings; name heuristics mark possible “date” fields (`date`, `month`, `year` in the header).
3. **Compute** min / max / sum / average / count for each numeric column.
4. **Call Anthropic** (optional) with a prompt built from filename, row count, column names, first 20 rows as JSON, and numeric stats.
5. **Render**:
   - Stats strip: row count, column count, numeric vs category field counts  
   - Typewriter-style **AI narrative**  
   - **Column inventory** pills  
   - Six **insight** cards (parsed from the model reply after an `INSIGHTS:` block)  
   - Up to **four charts**: bar (first numeric), line (second numeric), donut (first text column frequency), scatter or horizontal bar depending on schema  

If the API is unavailable or you skip the key, the page uses a **fallback** story and insights derived from local stats.

## Requirements

- A modern browser and **internet** (fonts, Papa Parse, Chart.js, and—if used—Anthropic load over the network).
- For **full AI narrative and curated insights**, an [Anthropic API](https://www.anthropic.com/) key. The app calls `https://api.anthropic.com/v1/messages` with model **`claude-sonnet-4-20250514`**.

## How to run

1. Clone this repository.
2. Open `CSV_Analyzer.html` in your browser (double-click or `open CSV_Analyzer.html` on macOS).

- **Upload**: drag and drop a `.csv` onto the zone, or use **Choose File**.
- **Sample data**: use **Load sample dataset** to try the flow without your own file.
- **API key**: click **API Key** in the header. The key is stored in **`localStorage`** under `anthropic_api_key`. Leave the prompt empty and confirm to clear it.

## Keyboard / interactions

There are no special shortcuts; everything is click or drag-and-drop.

## Security and privacy

- The HTML file includes an inline comment that **API keys in client-side pages are visible** to anyone who can open the page or devtools. For production, call Anthropic from a **backend** you control instead of the browser.
- When you use the AI path, **dataset metadata and a sample of rows** are sent to Anthropic as described in the prompt (first 20 rows plus column stats). Do not use real secrets or highly sensitive personal data without reviewing that behavior and your compliance needs.
- **Charts and fallback text** are computed entirely in the browser from the parsed CSV.

## Dependencies (loaded from CDNs)

| Resource | Use |
|----------|-----|
| Google Fonts | Playfair Display, DM Sans, DM Mono |
| Papa Parse | CSV parsing |
| Chart.js | Bar, line, doughnut, scatter charts |

## File layout

| Path | Role |
|------|------|
| `CSV_Analyzer.html` | Single-file app (markup, styles, scripts) |
| `README.md` | This document |

## Troubleshooting

- **“API key is required”** — Enter a key via **API Key**, or continue with the automatic fallback story (no live model).
- **“Failed to connect to AI service”** — Network, CORS, invalid key, or quota; fallback runs after the alert.
- **Empty or wrong charts** — Charts use the **first** numeric/text columns and **first N rows** for previews; very wide or messy CSVs may need cleaning for meaningful plots.

## Author

Built by **sasi pretham**.

## License

This project is open source and available for personal and commercial use.

## Credits

Footer in the app: built with **Chart.js** and **Papa Parse**; branding **DataStory — AI Dataset Storyteller**.
