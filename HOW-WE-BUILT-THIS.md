# How We Built the Port Angeles Plant Guide

This document records exactly what was asked and how the guide was created,
so you can update it yourself or ask Claude for changes in the future.

---

## What the guide contains

- **68 plants** total:
  - ~34 plants from the nursery tags you photographed (your yard)
  - ~34 bee-forage plants from the NASA Washington State honey bee list
  - Where a plant appeared in both sources (Elderberry, Holly, Hazelnut), it was merged into one row

- For each plant: bloom calendar, deer & rabbit resistance, pollinator value, care info (sun/water/zone/size), notes, and photos loaded automatically from Wikipedia

---

## The step-by-step process

### Step 1 — Mom's plant label photos
Jeremy gave Claude 174 photos from the Archive folder. Claude read each image and identified:
- The plant name from the label front
- Care information (zone, sun, water, size) from the label back
- The actual plant from the accompanying garden photo (triads of front/back/plant)

Two plants that needed extra attention:
- **IMG_3215**: Ceanothus impressus 'Victoria' (had to read the tag carefully)
- **IMG_3218/3219**: Ceanothus 'Beekeeper' and Arbutus 'Marina' (Marina Strawberry Tree) — the plant labels were attached to burlap-wrapped nursery stock

### Step 2 — NASA bee forage list
Loaded `Port Angeles Bee Forage.xlsx` (37 plants, Region 2 Washington State honey bee forage species). These are plants within a typical bee's flight range around Port Angeles that support hive health.

### Step 3 — Research
For each plant, Claude filled in:
- **Bloom months** (1–12 numeric array for the calendar)
- **Deer & rabbit resistance** (High / Moderate / Low with a brief explanation)
- **Pollinator value** (which insects are attracted)
- **Notes** (PNW-specific tips, toxicity where relevant)

### Step 4 — Building the web app
Single-file HTML — everything in one file, no internet connection needed to open it (though photos load from Wikipedia when online).

**Images**: Rather than embedding photos (which would make the file huge), the app fetches plant photos from Wikipedia's free image database when you're online. It uses the scientific name to find the right image. If you're offline, a leaf emoji placeholder shows instead.

**The deer/rabbit section**: Every plant card and detail view shows resistance ratings with colored dots (green = high resistance, amber = moderate, rust = low).

---

## How to update it yourself

### Adding a new plant
Open `Port Angeles Plant Guide.html` in a text editor (TextEdit, or any code editor).

Find the line `const PLANTS = [` — the plant list starts here. Each plant looks like:

```
{name:'Plant Common Name', sci:'Genus species', months:[5,6,7],
 bloom_season:'Late Spring – Summer', peak_bloom:'June',
 pollinator:'Yes – bees', planted:'Y',
 deer:'High – avoided', rabbit:'High',
 water:'Moderate', sun:'Full Sun', size:'2 ft', zone:'4–8',
 origin:'Europe', notes:'Any extra info here.'},
```

Copy an existing entry, change the values, and paste it before the closing `];`

**Month numbers**: 1=January, 2=February, … 12=December

**Deer resistance**: Start with `High –`, `Moderate –`, or `Low –` so the filter works correctly.

### Asking Claude to do it
Give Claude:
1. The plant name and scientific name
2. Any care info you have (from the nursery tag)
3. Say "add this to the Port Angeles Plant Guide" and give Claude the HTML file

### Updating a plant's status
Open the guide in your browser, click on any plant, and use the **Garden Tracker** section at the bottom of the detail panel. You can mark it as Planned / Ordered / In Ground / Established, add a planting date, location in the garden, and personal notes. All of this is saved in your browser (not online) so it stays private.

### Exporting your tracker data
Click the **Export** button in the top bar. It downloads a JSON file with all your tracked plants and notes — good for backup.

---

## Prompts Claude was given

The following are the actual prompts (slightly summarized) that produced this guide:

**To catalog the photos:**
> "Catalog these plant label photographs. For each, one line: IMG_XXXX: [label front: PLANT NAME | label back | plant photo | other]"

**To read the NASA spreadsheet:**
> "Read Port Angeles Bee Forage.xlsx and tell me what columns exist, how many rows, and sample a few rows."

**The design brief:**
> "Audience: parents and gardening friends. Feel: warm and earthy, naturalist's field notebook, clean enough to print. The WOW moment: bloom calendar graphics, fast-loading photos, smooth motion."

**The build instruction:**
> "Build Port Angeles Plant Guide.html — single-file, same architecture as the Sequim app but with: mobile-first design, Wikimedia images loaded via API (lazy), editorial card layout with plant name overlaid on image, deer/rabbit resistance section in modal, deer-resistant filter toggle, Cormorant Garamond + DM Sans fonts."

---

## Technical notes

- **File**: `Port Angeles Plant Guide.html` — one file, open directly in any browser
- **No installation needed** — drag to browser, it works
- **Garden tracker data** is saved in your browser's local storage (stays on your device, not uploaded anywhere)
- **Photos** load from `https://en.wikipedia.org` — free, no account needed
- **Offline**: The app works fully offline; photos just show leaf emoji placeholders until you're back online
