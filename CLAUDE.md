# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Single-file static HTML app — no build step, no dependencies, no package.json. Open `Sequim Plant Guide.html` directly in a browser.

## Architecture

Everything lives in `Sequim Plant Guide.html` (~660 lines):

- **PLANTS array** (inlined JS, ~line 260–320): All 123 plant records as JS objects. Each plant has: `name`, `sci` (scientific name), `img` (relative path), `months` (array of bloom month numbers 1–12), `pollinator`, `planted` (`'Y'`/`''`), `bloom_season`, `peak_bloom`, `water`, `sun`, `size`, `growth`, `zone`, `origin`, `evergreen`, `edible`, `toxicity`, `forager`, `notes`.

- **Garden Tracker** (localStorage key: `sequimGarden`): Stores `statuses` (plant name → `{status, date, location, updated}`) and `notes` (plant name → array of `{ts, text}`). Statuses: `Planned`, `Ordered`, `In Ground`, `Established`.

- **Two views**:
  - Cards (`#cards-view`): filterable grid, 260px min columns
  - Calendar (`#calendar-view`): month summary bar chart + bloom timeline table, sorted by earliest bloom month

- **Filters**: text search (name + sci), month (1–12), pollinator toggle, planted toggle, "My Garden" toggle (only localStorage-tracked plants)

- **Modal**: opens on card/timeline row click, shows full plant detail + garden tracker panel + notes log. Uses `currentModalName` global (not index) since modal can re-render in place while open.

## Data Sources

- `Sequim Plant List - Enriched FULL.xlsx` — canonical plant data spreadsheet (source of truth for PLANTS array)
- `Sequim Plant List - Enriched Sample.xlsx` — subset used for testing/prototyping
- `Archive/` — plant photos (JPEG); referenced via relative `img` paths in PLANTS records

## Key Conventions

- `escHtml()` must wrap all plant data inserted into innerHTML — plant names/notes can contain `&`, `<`, quotes.
- `currentModalName` (string) is the global tracking which plant is open in the modal. All garden actions (`startTracking`, `updateStatusCur`, etc.) operate on this global — never pass plant names as onclick string args (XSS risk).
- `render()` must be called after any state or localStorage mutation to keep the card grid/calendar in sync.
- Month numbers are 1-indexed throughout (January = 1).
