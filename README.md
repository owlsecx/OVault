# 🦉 OVault

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Linux%20%2F%20Windows-informational?style=flat-square&logo=linux&logoColor=white&color=0a0c10"/>
  <img src="https://img.shields.io/badge/Category-OForensics%20%2F%20Cloud%20Evidence-cyan?style=flat-square"/>
  <img src="https://img.shields.io/badge/No%20Core%20Dependencies-Stdlib%20Only-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/License-MIT-green?style=flat-square"/>
  <img src="https://img.shields.io/badge/Part%20of-OwlSec%20Toolkit-7b5ea7?style=flat-square"/>
  <img src="https://img.shields.io/badge/Version-1.0-cyan?style=flat-square"/>
</p>

> **OVault** is a cloud evidence collector — analyse Google Takeout ZIP exports and iCloud backups, extract EXIF and GPS coordinates from image folders, build a unified chronological evidence timeline, and profile device identifiers from any evidence source.

---

## 📌 Overview

OVault collects and analyses digital evidence from cloud export archives. Each module feeds into a shared global timeline that aggregates all timestamped events across sources. Results are exportable to JSON, CSV, and TXT reports. OVault also supports CLI flags for direct non-interactive analysis.

> **Use only on data you own or are legally authorised to access.**

---

## 🖥️ Modules

| # | Module | Description |
|---|--------|-------------|
| **[1] Google Takeout** | Analyse a Google Takeout `.zip` export — extract location history, searches, YouTube, Chrome, Drive files, and account email |
| **[2] iCloud Backup** | Analyse an iCloud / iTunes backup folder — extract contacts, call log, SMS count, Safari history, app list, and device info |
| **[3] Photo Evidence** | Scan an image folder — extract EXIF metadata, GPS coordinates, camera model, and detect stripped metadata |
| **[4] Timeline Builder** | Aggregate all collected events into a unified chronological timeline — exportable to JSON or CSV |
| **[5] Device Profiler** | Scan any folder or ZIP for device identifiers using regex patterns — IMEI, MACs, emails, UUIDs, and more |
| **[E] Export Session** | Export all results from the current session to JSON or TXT |

---

## 📦 Google Takeout Analyser

Accepts `.zip` files exported from `takeout.google.com`.

| Extracted Data | Detail |
|----------------|--------|
| **Location history** | GPS coordinates (lat/lon) from `latitudeE7/longitudeE7` — up to 5,000 points |
| **Search history** | Query text and timestamps from My Activity |
| **YouTube watch history** | Video title, channel name, and timestamp |
| **Chrome history** | URL, page title, and visit timestamp — up to 300 entries |
| **Drive file list** | File name, creation date, and size — up to 100 entries |
| **Account email** | Extracted from `profile.json` or `subscriber.json` |
| **Date range** | Earliest and latest timestamps across all collected data |
| **Archive integrity** | SHA-256 hash and file size in MB |
| **Device profile** | Auto-runs Device Profiler on the ZIP after analysis |

---

## 📱 iCloud Backup Analyser

Accepts iCloud backup folders from iTunes / Finder or OMOBILESCAN extractions.

| Extracted Data | Detail |
|----------------|--------|
| **Device info** | Device name, product type, iOS version, serial number, IMEI, last backup date, iTunes version — from `Info.plist` / `Manifest.plist` |
| **Contacts** | Name and phone number — from `AddressBook.sqlitedb` |
| **Call log** | Caller, duration, date, call type — from `CallHistory.storedata` |
| **SMS count** | Total message count — from `sms.db` |
| **Safari history** | URL, page title, visit timestamp — up to 100 entries from `History.db` |
| **App list** | Installed applications — from backup metadata |
| **Location data** | Location entries where available |

---

## 📷 Photo Evidence (EXIF)

Scans a folder recursively for image files and extracts EXIF metadata.

| Supported Formats | `.jpg` `.jpeg` `.tiff` `.tif` `.heic` `.heif` `.png` |
|-------------------|------------------------------------------------------|
| **Max images** | Up to 500 per scan |

| Extracted Field | Detail |
|-----------------|--------|
| **Datetime** | `DateTimeOriginal` or `DateTime` EXIF tag |
| **Camera make** | Manufacturer name |
| **Camera model** | Device model |
| **Software** | Processing software tag |
| **GPS latitude / longitude** | Decimal degrees — converted from DMS with hemisphere reference |
| **GPS altitude** | Where available |
| **Stripped metadata** | Flagged for `.jpg`/`.jpeg` files with no EXIF — may indicate evidence tampering |
| **Google Maps link** | Auto-generated for each GPS-tagged photo |

**EXIF parsing:** uses `exifread` if installed, falls back to raw JPEG binary parser (`FF E1` APP1 marker) — no Pillow required for core functionality.

---

## 🕒 Timeline Builder

Aggregates all timestamped events from every module into a single sorted timeline.

| Timeline Sources | Events Added Automatically |
|-----------------|---------------------------|
| Google Location | `Location recorded` with lat/lon |
| Google Search | Search query text |
| YouTube | `Watched: <title>` |
| Chrome | `Visited: <title or URL>` |
| Safari | `Visited: <title or URL>` |
| Photo GPS | `Photo at <lat, lon>` with filename |
| Photo | `Photo taken — <camera model>` |
| File System | File modification timestamps from optional extra folder |

| Export Format | Filename |
|---------------|----------|
| **JSON** | `ovault_timeline_YYYYMMDD_HHMMSS.json` |
| **CSV** | `ovault_timeline_YYYYMMDD_HHMMSS.csv` — columns: `timestamp`, `source`, `event`, `detail` |

---

## 🔍 Device Profiler

Scans text-based files inside a folder or ZIP for device identifiers using regex patterns.

| Identifier | Pattern |
|------------|---------|
| **IMEI** | 15-digit number |
| **Serial Number** | 8–14 character alphanumeric |
| **MAC Address** | `XX:XX:XX:XX:XX:XX` or `XX-XX-XX-XX-XX-XX` |
| **IPv4 Address** | Standard dotted-quad |
| **Email** | Standard email format |
| **Phone Number** | 10–15 digit international format |
| **Google ID** | 21-digit numeric `"id"` field |
| **Apple ID** | `@icloud.com`, `@me.com`, `@apple.com` addresses |
| **UUID** | Standard 8-4-4-4-12 hex format |
| **Android ID** | 16-character hex `"androidId"` field |

Scans: `.json` `.txt` `.csv` `.xml` `.plist` `.log` `.html` `.js` — up to 20 unique values per identifier type.

---

## 📤 Export Formats

| Report | Filename | Format |
|--------|----------|--------|
| **Google Takeout** | `ovault_report_YYYYMMDD_HHMMSS.json / .txt` | Full analysis with summary |
| **iCloud Backup** | `ovault_report_YYYYMMDD_HHMMSS.json / .txt` | Full analysis with summary |
| **Photo GPS** | `ovault_photos_YYYYMMDD_HHMMSS.json / .csv` | Per-image EXIF and GPS data |
| **Timeline** | `ovault_timeline_YYYYMMDD_HHMMSS.json / .csv` | All collected events sorted by timestamp |
| **Session** | `ovault_report_YYYYMMDD_HHMMSS.json / .txt` | All module results from current session |

---

## 🖥️ CLI Mode

OVault supports direct non-interactive analysis via command-line flags:

```bash
./OVault -g takeout.zip              # Analyse Google Takeout ZIP
./OVault -i /path/to/icloud_backup  # Analyse iCloud backup folder
./OVault -p /path/to/photos         # Extract EXIF from image folder
./OVault -g takeout.zip -o report.json  # Save output to JSON file
```

| Flag | Description |
|------|-------------|
| `-g / --google` | Google Takeout ZIP path |
| `-i / --icloud` | iCloud backup folder path |
| `-p / --photos` | Image folder path |
| `-o / --output` | Save JSON report to specified file |

---

## ⚙️ Requirements

- **Linux or Windows**
- **No Python installation needed** — runs as a standalone executable
- **No core dependencies** — stdlib only (`json`, `csv`, `zipfile`, `sqlite3`, `hashlib`, `struct`, `xml`, `re`)

**Optional — enhance EXIF extraction:**
```bash
pip install Pillow exifread
```

---

## 🚀 Usage

```bash
./OVault
```

---

## 📦 Part of OwlSec Toolkit

This tool is part of the **OwlSec** suite — a collection of 300+ security and privacy tools.

🔗 [owlsec.org](https://owlsec.org)

---

## ©️ License

MIT License — © Khaled S. Haddad

*Tools are distributed as pre-built executables. Source code is proprietary.*
