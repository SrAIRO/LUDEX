<div align="center">

# LUDEX — HLTB Tracker

Lightweight browser-based game backlog manager powered by  
HowLongToBeat data and RAWG scores.

No backend. No accounts. Fully local.

</div>

---

## Overview

Ludex is a single-file web application designed to manage and analyze your video game backlog using:

- A locally imported HowLongToBeat dataset
- RAWG API for review scores
- Local browser storage (IndexedDB + localStorage)
- Optional automatic file-based backups

Everything runs entirely in your browser.

---

## Core Features

### Game Search
Autocomplete search powered by a locally stored HLTB database.

### Duration Modes
Switch between:
- Main Story
- Main + Extras
- Completionist

### Score Integration
Fetches scores from RAWG:
- Metacritic score (if available)
- RAWG community rating (fallback)

### Ratio Calculation
Automatically calculates score-to-hours efficiency.

### Status Management
Each game can be marked as:
- Pending
- Completed
- Dropped

### Backup System
- Manual export
- Manual import
- Automatic overwrite backup (same file updated each time)

---

## Installation

This project consists of a single HTML file.

### Option 1 — Direct Usage

1. Download `LUDEX.html`
2. Open it in your browser

Example:

```bash
double-click LUDEX.html
```

No server required.

Note: When opening via `file://`, RAWG API calls may require the built-in CORS proxy fallback (already implemented in the application).

---

## RAWG API Configuration

Ludex requires a free RAWG API key to fetch review scores.

### Step 1 — Create a RAWG account

Visit:

```
https://rawg.io/apidocs
```

Create an account and generate your API key.

### Step 2 — Configure inside Ludex

Inside the app:

```
Settings → API Key
```

Paste your key and click:

```
Save & Verify
```

If valid, the status indicator will confirm successful verification.

---

## HowLongToBeat Database Setup

Ludex does not bundle HLTB data. You must import it manually.

### Step 1 — Download the dataset

Go to:

```
https://github.com/julianxhokaxhiu/hltb-scraper/releases/latest
```

Download one of the following files:

```
howlongtobeat_games.json
OR
howlongtobeat_games.csv
```

### Step 2 — Import into Ludex

Inside the app:

```
Settings → HLTB Database → Upload File
```

Once imported:
- Data is stored in IndexedDB
- It persists locally
- No re-import needed unless you want to update it

---

## Backup System

Ludex supports two types of backups.

### Manual Export

```
Settings → Export List
```

This generates a JSON file structured like:

```json
{
  "version": 1,
  "exported": "ISO date",
  "count": 42,
  "games": [...]
}
```

---

### Import

```
Settings → Import List
```

When importing, you can:
- Replace your entire list
- Merge with existing entries

---

### Automatic Backup (Overwrite Mode)

When enabled:

- You select a file once
- Ludex stores a FileSystemFileHandle
- Every automatic save overwrites that same file
- No duplicate versions are created

This uses the File System Access API and works best in Chromium-based browsers.

---

## Architecture

Data storage model:

```
HLTB Dataset   → IndexedDB
User Game List → localStorage
API Key        → localStorage
Backup Handle  → IndexedDB
```

Score retrieval logic:

```
RAWG API
   ↓
Metacritic (priority)
   ↓
RAWG rating × 20 (fallback)
```

If direct API requests fail under `file://`, the app retries using:

```
https://corsproxy.io/
```

---

## Technical Stack

```
• Vanilla JavaScript
• IndexedDB
• localStorage
• File System Access API
• RAWG REST API
```

No frameworks. No external dependencies.

---

## Notes

- HLTB data must be manually downloaded (not included for legal reasons).
- RAWG free tier has rate limits.
- File System Access API is not supported in all browsers.
- Best experience: Chromium-based browsers.

---

## License

MIT License

You are free to modify and redistribute this project.
