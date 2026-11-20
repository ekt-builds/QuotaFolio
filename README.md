# QuotaFolio

**Offline-first Android scheduled reading & quota telemetry app** built with **Kotlin**, **Jetpack Compose**, and **Room SQLite**.

Instead of passive to-do reminders that get swiped away and forgotten, QuotaFolio schedules exact reading slots that pop up directly to your bookmarked page, slide, or chapter—tracked locally with zero cloud accounts, zero ads, and full offline telemetry.

- **Live Website:** [ekt-builds.github.io/QuotaFolio](https://ekt-builds.github.io/QuotaFolio/)
- **APK Releases:** [GitHub Releases](https://github.com/ekt-builds/QuotaFolio/releases/latest)
- **Minimum Android Version:** Android 8.0 Oreo (`minSdk = 26`)

---

## Core Features

- **Doze-Proof Scheduled Pop-Ups (`AlarmManager.setAlarmClock`):**
  - Triggers scheduled reading slots on active weekdays and automatically reschedules future slots.
  - Launches `ReadingPopupActivity` over active apps or lock screens when **"Display over other apps"** is permitted, with high-priority full-screen intent notification fallback and automatic re-registration on device reboot or time/timezone changes.
  - Includes a built-in **10-Second Pop-Up Diagnostic** inside the `(i)` Guide menu.
- **5-Table Room SQLite Telemetry (`quotafolio.db`):**
  - `files` — Stores metadata, isolated internal file paths, unit type (`Pages`, `Slides`, or `Charts`), total units, and current bookmark.
  - `schedules` — Stores per-file active weekday bitmasks, session quota, normalized 24-hour slot times, and schedule versions.
  - `session_logs` — Immutable log of `COMPLETED` and `SKIPPED` slots with unit deltas, post-session bookmarks, and focus durations.
  - `daily_targets` — Snapshots daily targets and freezes past dates to preserve historical accuracy.
  - `bookmark_audit` — Records manual bookmark adjustments non-destructively.
- **Non-Destructive Schedule & Bookmark Editing:**
  - Update active days (`Mon–Sun`), quota per slot, 24-hour trigger times (auto-normalized, e.g., `8:5` → `08:05`), total units, or current bookmark without wiping historical logs.
- **Dashboard Telemetry & 47 Discipline Verses:**
  - Displays live local date, ISO-8601 week (`Wxx`), and timezone/UTC offset alongside Today's completion ring, Weekly vertical bars, Day Streak, Total Units Read, and Total Focus Time.
  - Includes **47 curated discipline verses** with daily rotation, `<` / `>` navigation, and instant search by book, theme, or keyword.

---

## Supported Offline File Formats

All parsing runs locally on the device with zero external cloud services:

| Extensions | Rendering & Reader Controls | Unit Counting | Notes |
| :--- | :--- | :--- | :--- |
| `.pdf` | Full visual page rendering + `1.0x–3.5x` pinch-to-zoom & pan | Exact PDF page count | Best for textbooks, diagrams, and exported slides |
| `.png`, `.jpg`, `.jpeg`, `.webp` | Full visual rendering + `1.0x–3.5x` pinch-to-zoom & pan | `1` Chart (OOM-safe decode) | Best for anatomy/biology charts and study sheets |
| `.epub` | Dark-mode text in OPF spine chapter order + `A-/A+` font size | ~1,400 chars / page | DRM-free EPUB files; embedded images are skipped |
| `.docx`, `.odt` | Dark-mode XML text extraction + `A-/A+` font size | ~1,400 chars / page | Export diagram-heavy docs to `.pdf` for visuals |
| `.pptx`, `.odp` | Slide/page text extraction + `A-/A+` font size | Exact slide / page count | Visual-only slides are noted; export to `.pdf` for graphics |
| `.rtf`, `.html`, `.htm` | Cleaned dark-mode text (strips markup/metadata) + `A-/A+` | ~1,400 chars / page | Good for offline articles and rich-text notes |
| `.txt`, `.md`, `.csv`, `.json` | Plain / structured dark-mode text + `A-/A+` font size | ~1,400 chars / page | Great for study notes, Markdown, and vocab lists |

> **Format Caveats:** Legacy pre-2007 binary `.doc`/`.ppt` files, DRM-protected e-books (`.mobi`/`.azw3`), and password-protected PDFs are not supported.

---

## Installation & Background Pop-Up Setup

1. Download the latest APK from [GitHub Releases](https://github.com/ekt-builds/QuotaFolio/releases/latest) and install it on your Android device (allow **Install unknown apps** for your browser/file manager if prompted).
2. Open **QuotaFolio** and grant **Notifications**, **Display over other apps**, and **Exact Alarms** when prompted.
3. **Prevent OEM Background Kills (Especially Xiaomi / MIUI / HyperOS, Samsung One UI, etc.):**
   - Open **App Info → QuotaFolio → Battery** and select **Unrestricted / No restrictions**.
   - On **Xiaomi / POCO / Redmi (MIUI / HyperOS)**, enable **Autostart / Background Autostart** and allow **Display pop-up windows while running in the background** under *Other permissions*.
4. Tap the top-right **`(i)`** button → **Test 10s Pop-Up**, exit or lock your screen, and confirm **POP-UP ENGINE VERIFIED** appears after 10 seconds.

---

## Building From Source (Android Studio)

1. Clone the repository:
   ```bash
   git clone [https://github.com/ekt-builds/QuotaFolio.git](https://github.com/ekt-builds/QuotaFolio.git)
