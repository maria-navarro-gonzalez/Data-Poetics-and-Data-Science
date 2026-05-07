# Joaquín Sabina — Lyrical Evolution Analysis

A data visualization project exploring how the emotional vocabulary and thematic focus of Joaquín Sabina's lyrics evolved across his studio discography (1978–2017), using word cloud animations, bar chart races, and sales-scaled visual metaphors.

---

## Project Overview

This project analyzes the **top 15 most frequent words** across Sabina's studio albums, maps them to emotional categories (Love, Longing, Hope, Melancholy), and visualizes their evolution over time through three complementary formats:

1. **Bowler Hat Word Cloud GIF** — words inside Sabina's iconic hat, hat size scaled by estimated album sales
2. **Tree Word Cloud GIF** — words in an organic tree canopy with a gradient trunk
3. **Bar Chart Race GIF** — animated ranking of word share percentages across albums

---

## Repository Structure

```
sabina-lyrical-analysis/
├── README.md
├── requirements.txt
├── data/
│   └── lyrics_txt/
│       └── sabina/
│           ├── 1978/          ← one folder per album year
│           │   ├── 01_song_title.txt
│           │   └── ...
│           └── .../
├── outputs/
│   ├── sabina_album_wordclouds.gif       ← hat word cloud animation
│   ├── sabina_tree_wordclouds.gif        ← tree word cloud animation
│   ├── sabina_bar_chart_race.gif         ← bar chart race animation
│   ├── sabina_all_albums_grid.png        ← hat word clouds, all albums
│   ├── sabina_tree_all_albums_grid.png   ← tree word clouds, all albums
│   ├── sabina_top15_trends.csv           ← raw word frequencies per album
│   ├── sabina_top15_trends_normalized.csv
│   └── pngs/ & pngs_tree/               ← individual album frames
├── sabina_tree_wordclouds.py             ← hat word cloud script
├── sabina_tree_wordclouds_final.py       ← tree word cloud script
└── sabina_bar_chart_race.py              ← bar chart race script
```

---

## Setup

### Requirements

```bash
pip install wordcloud imageio pillow matplotlib pandas numpy nltk
```

Or install from the requirements file:

```bash
pip install -r requirements.txt
```

### NLTK Data

The scripts will auto-download the required NLTK data on first run:
- `stopwords`
- `punkt`
- `punkt_tab`

---

## Usage

### Step 1 — Hat Word Cloud (run first — generates the trend CSVs)

```bash
python sabina_tree_wordclouds.py
```

Outputs:
- Individual album PNGs in `outputs/pngs/`
- `outputs/sabina_album_wordclouds.gif`
- `outputs/sabina_all_albums_grid.png`
- `outputs/sabina_top15_trends.csv`

### Step 2 — Tree Word Cloud

```bash
python sabina_tree_wordclouds_final.py
```

Outputs:
- Individual album PNGs in `outputs/pngs_tree/`
- `outputs/sabina_tree_wordclouds.gif`
- `outputs/sabina_tree_all_albums_grid.png`

### Step 3 — Bar Chart Race (requires trend CSV from Step 1)

```bash
python sabina_bar_chart_race.py
```

> **Note on scope:** While the word cloud scripts track the **top 15 most frequent words** across the full discography, the bar chart race displays only the **top 10 bars** at any given time. The y-axis values show each word's **percentage share of the combined total count of all top-15 words for that album** — so the bars represent relative prominence within each album, not raw counts. This normalization allows fair comparison across albums of different lengths.

Outputs:
- `outputs/sabina_bar_chart_race.gif`

---

## Configuration

Each script has a configuration section at the top for easy adjustment:

### Hat Word Cloud (`sabina_tree_wordclouds.py`)
| Parameter | Default | Description |
|-----------|---------|-------------|
| `TOP_N` | 15 | Number of top words to track |
| `INCLUDE_CHORUS` | False | Whether to include repeated chorus lines |
| `SECONDS_PER_FRAME` | 3.0 | GIF frame duration in seconds (3.0 = current setting) |
| `MIN_SIZE` | 1200 | Minimum hat size in pixels (lowest-selling album) |
| `MAX_SIZE` | 2000 | Maximum hat size in pixels (best-selling album) |

### Tree Word Cloud (`sabina_tree_wordclouds_final.py`)
| Parameter | Default | Description |
|-----------|---------|-------------|
| `TOP_N` | 15 | Number of top words to track |
| `INCLUDE_CHORUS` | False | Whether to include repeated chorus lines |
| `SECONDS_PER_FRAME` | 3.0 | GIF frame duration in seconds (3.0 = current setting) |

### Bar Chart Race (`sabina_bar_chart_race.py`)
| Parameter | Default | Description |
|-----------|---------|-------------|
| `TOP_N_BARS` | 10 | Number of bars visible at any one time (top 10 of the 15 tracked words) |
| `SECONDS_PER_ALBUM` | 5.0 | Transition duration per album |
| `GLOBAL_X_MAX` | 38.0 | Fixed x-axis maximum (%) — bars show each word's % share of all top-15 word counts for that album |

---

## Emotional Color Coding

Words are assigned to one of four emotional categories, each with a distinct color:

| Emotion | Color | Words |
|---------|-------|-------|
| Love | `#c0407a` (pink) | amor, besos, cama, mujer, dos |
| Longing | `#6b3daa` (purple) | corazón, nunca, mañana, vez |
| Hope | `#1e7a4a` (green) | vida, siempre |
| Melancholy | `#2e5f98` (blue) | noche, día, mientras, tres |

---

## Data & Methods

### Lyrics Corpus
Lyrics were collected as plain `.txt` files, one file per song, organized by album year. Song files are prefixed with track numbers (e.g., `01_song_title.txt`).

### Text Processing Pipeline
1. **Header removal** — strip section markers like `[Verse]`, `[Chorus]`
2. **Low-information line removal** — filter lines with high repetition or very short tokens
3. **Chorus detection & removal** — identify and optionally exclude repeated blocks
4. **Normalization** — lowercase, remove punctuation, retain Spanish characters (á, é, í, ó, ú, ü, ñ)
5. **Stopword removal** — Spanish NLTK stopwords + custom set (`va`, `si`, `ya`, etc.)
6. **Frequency capping** — each word capped at 3 occurrences per song to prevent any single song from dominating

### Album Sales Estimates
Exact per-album sales figures are not publicly available. Hat sizes are scaled using best estimates derived from documented platinum certifications, chart positions, and press reports. Física y Química (1992) is documented at 1M+ copies; other albums are estimated relative to this benchmark. These estimates should be treated as approximate.

---

## Requirements File

```
wordcloud>=1.9.0
imageio>=2.28.0
Pillow>=9.0.0
matplotlib>=3.7.0
pandas>=2.0.0
numpy>=1.24.0
nltk>=3.8.0
```

---

## Notes

- Run the hat word cloud script first — it generates the `sabina_top15_trends.csv` that the bar chart race reads.
- Rendering at 4× supersampling means each album takes 1–3 minutes to generate. The full run is ~20–45 minutes depending on machine.
- **Viewing GIFs — recommended methods:**
  - **Chrome/Firefox/Safari:** Drag and drop the `.gif` file directly onto the browser window — it will play automatically and loop.
  - **Google:** Drag the `.gif` file onto the Google search bar — it will open and animate in full screen.
  - **macOS Quick Look:** Select the file in Finder and press Space — it will animate in place.
  - **Frame by frame:** Open the GIF in a browser, right-click → Open Image in New Tab, then use your keyboard's left/right arrow keys to step through frames one at a time in some viewers (e.g. GIMP or online GIF editors like ezgif.com).
- The bar chart race GIF does not loop (`repeat=False`) — it stops on the last album.
- The hat and tree word cloud GIFs loop continuously by default.

---

## Author

Maria Navarro Gonzalez — Independent Study, Claremont McKenna College
