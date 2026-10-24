# Brahma Muhurtha Calculator

A lightweight, single-page web app that tells you **when to wake up for Brahma Muhurtha** (96 minutes before sunrise) and **when to go to bed** to get the amount of sleep you want — for any place in the world, on any day in the next 16 days.

**Live app:** https://fellowshipofhogwarts.github.io/brahma-muhurtha/

No sign-up, no installation, no tracking. It is one HTML file that runs entirely in your browser.

---

## Table of contents

1. [Key concepts](#key-concepts)
2. [Features](#features)
3. [Using the app](#using-the-app)
4. [How it works](#how-it-works)
5. [Run locally](#run-locally)
6. [Deploy to GitHub Pages](#deploy-to-github-pages)
7. [Privacy](#privacy)
8. [Limitations](#limitations)
9. [Project structure](#project-structure)
10. [Credits](#credits)
11. [License](#license)

---

## Key concepts

### Brahma Muhurtha

In the Vedic tradition, a day is divided into 30 *muhurthas*. **Brahma Muhurtha** is the second-to-last muhurtha of the night, traditionally regarded as the most favourable time for meditation, study and spiritual practice.

### The 96 → 48 minute rule

This app uses the widely followed fixed rule that one muhurtha lasts **48 minutes**:

| Moment | Time | Meaning |
|---|---|---|
| **Muhurtha starts** | Sunrise − 96 min | Brahma Muhurtha begins. This is your **wake-up time**. |
| **Muhurtha ends** | Sunrise − 48 min | Brahma Muhurtha ends. |
| **Sunrise** | From live data | The upper edge of the sun appears on the horizon. |

Brahma Muhurtha does **not** last until sunrise. It ends 48 minutes before sunrise, and the next muhurtha follows.

### Ideal bedtime

```
Bedtime = Muhurtha start − chosen sleep duration (7 to 9 hours)
```

**Example:** sunrise at 07:00 → muhurtha starts 05:24 → with 8 hours of sleep, bedtime is 21:24 the evening before.

---

## Features

| Feature | Description |
|---|---|
| **City search** | Search any city, town or village worldwide. |
| **GPS location** | Use your device's current position (requires an `https://` page). |
| **Quick cities** | One-click presets: Erlangen, Munich, Bengaluru, Stockholm, Trollhättan. |
| **Today / Tomorrow** | Switch days with one tap or the arrow keys. |
| **Custom date** | Pick any date within the next 16 days. |
| **Sleep slider** | Choose 7 to 9 hours of sleep in 30-minute steps. |
| **Timeline** | Bedtime → Muhurtha starts → Muhurtha ends → Sunrise at a glance. |
| **Live countdown** | Time left until wake-up, with days for later dates. Shows **Now** during the muhurtha and **Passed** afterwards. |
| **Time zone badge** | Shows the location's own time zone, e.g. `CET (UTC+1)` or `UTC+5:30`. |
| **Daylight-saving notice** | Warns when the clocks change overnight and adjusts the times. |
| **Smart day switching** | Late in the evening, tomorrow's times are shown automatically. |
| **Remembers your settings** | Last location and sleep duration are restored on your next visit. |
| **Mobile friendly** | Responsive layout; can be added to your phone's home screen. |
| **Accessible** | Keyboard navigation, screen-reader labels and live announcements. |

---

## Using the app

1. **Choose a location:** tap a quick city, type a city and press **Enter**, or tap the **GPS** button.
2. **Choose a day:** **Today**, **Tomorrow**, or a date from the date picker.
3. **Set your sleep target** with the slider (7 to 9 hours).
4. **Read your plan:**
   - **Wake-up time**: when to wake up (Brahma Muhurtha begins)
   - **Muhurtha**: the full start–end period
   - **Ideal bedtime**: when to go to sleep, and on which evening
   - **Time until wake-up**: a live countdown

> **Tip:** If you open the app after today's Brahma Muhurtha has ended, it automatically shows tomorrow. Tap **Today** to see today's times anyway.

---

## How it works

### Data sources

| Purpose | Service | Notes |
|---|---|---|
| Sunrise times | [Open-Meteo Forecast API](https://open-meteo.com/) | 16-day daily sunrise and sunset, with the location's time zone. No API key needed. |
| City search and reverse lookup | [Nominatim (OpenStreetMap)](https://nominatim.openstreetmap.org/) | Converts a city name to coordinates, and GPS coordinates to a place name. |

### Time zone handling

All times are shown in the **time zone of the selected location**, not the time zone of your device. If you are in Germany and select Bengaluru, you see Bengaluru local times. Sunrise times are retrieved as absolute timestamps and formatted with the browser's built-in `Intl` API.

### Daylight-saving changes

Bedtime is calculated on absolute time, not on clock time. On a night when the clocks change, you still get exactly the sleep duration you chose. A notice explains the change, for example: *"Clocks go back 1 hour overnight (CEST → CET)."*

### Midnight behaviour

If the page stays open past midnight, the app keeps showing **the same morning** you were planning for:

| Before midnight | After midnight |
|---|---|
| Tomorrow (chosen by you or by the app) | Same date, now shown as **Today** |
| Custom date | Stays on that date |
| Today (chosen by you) | Moves forward to the new today |

### Places without a sunrise

During polar night or midnight sun (far north or south), there is no sunrise to calculate from. The app shows a clear note instead of times.

### Performance and reliability

- Forecasts are cached for 30 minutes per location; changing the day or slider needs no new request.
- City searches are limited to one per second, following the Nominatim usage policy.
- Requests time out after 12 seconds, and the newest request always wins if you click quickly.
- Errors (offline, service unavailable, too many requests, unknown city) are shown as plain-language messages.

---

## Run locally

1. Download `index.html`.
2. Double-click it to open it in any modern browser (Chrome, Edge, Firefox, Safari).

An internet connection is needed for sunrise data and city search.

> **GPS note:** Browsers only allow location access on secure pages. When the file is opened directly from disk (`file://`), use the search or quick cities instead. GPS works on the hosted version, or locally via a server:
>
> ```bash
> python -m http.server 8000
> # then open http://localhost:8000
> ```

---

## Deploy to GitHub Pages

1. Create a **public** repository (e.g. `brahma-muhurtha`).
2. Upload `index.html` (plus `README.md` and `LICENSE` if you like).
3. Go to **Settings → Pages**, choose **Deploy from a branch**, select **main** and **/(root)**, then **Save**.
4. After one or two minutes, the app is live at `https://<username>.github.io/<repository>/`.

To update the app, upload a new `index.html`. The site refreshes within a few minutes.

---

## Privacy

- **Stored on your device only:** your last location (name and coordinates) and sleep duration, in the browser's `localStorage`. Clear your browser data to remove them.
- **Sent to third parties:** coordinates are sent to Open-Meteo to get sunrise times; search text (or GPS coordinates) is sent to OpenStreetMap Nominatim to find the place.
- **Not collected:** no accounts, cookies, analytics or tracking.

---

## Limitations

- **Fixed 48-minute muhurtha.** Traditional systems sometimes divide the actual length of the night into 15 parts, so the muhurtha varies with the season. This app uses the common fixed rule.
- **16-day range.** Dates are limited to the Open-Meteo forecast period.
- **Internet required.** Times cannot be calculated offline.
- **Fair use of free services.** Open-Meteo's free tier is for non-commercial use, and Nominatim is intended for light use. For heavy or commercial use, consider their paid or self-hosted options.
- **Sunrise accuracy.** Sunrise is calculated for sea level and a flat horizon. Mountains, buildings and altitude can shift the visible sunrise by a few minutes.

---

## Project structure

```
brahma-muhurtha/
├── index.html   # The complete app: HTML, CSS and JavaScript in one file
├── README.md    # This document
└── LICENSE      # MIT License
```

No build step, framework or dependencies are required.

---

## Credits

- Sunrise data: [Open-Meteo.com](https://open-meteo.com/) (CC BY 4.0)
- Place search: © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright), via Nominatim
- Developed by **Shashank** with the help of **Claude**

---

## License

Released under the [MIT License](LICENSE). Copyright © 2026 Shashank Koundinya Banavathi Rajashekara.
