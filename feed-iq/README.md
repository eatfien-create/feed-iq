# Feed iQ

Silage quality check for dairy farmers — Team X, Smart India Hackathon 2026 (SIH26111).

The app reads the Feed iQ probe (pH, moisture, temperature, EC and the AS7341 light sensor), checks silage photos for mould, estimates nutrients, and gives simple advice in **English, தமிழ் and हिन्दी**. It also has voice commands and silo stock tracking.

> **Demo build:** probe readings, nutrient values and photo results are hardcoded for the showcase. The trained models are listed in [TECH_STACK.md](TECH_STACK.md).

## Install on a phone

1. Open the GitHub Pages link in **Chrome on Android**.
2. Tap **⋮ → Install app** (or **Add to Home screen**). You can also use **Settings → Install Feed iQ** inside the app.
3. Feed iQ opens full screen from its own icon and works offline after the first visit.

On iPhone, open the link in Safari and tap **Share → Add to Home Screen**.

## What's in the demo

| Screen | What it does |
|---|---|
| Probe test | Live-looking sensor readings, nutrients (DM, protein, NDF, ADF, starch, ash, fat, sugars), energy from ADF, and advice |
| Upload photo | Recognises the two demo photos (clean and mouldy): score, mould areas, and what to do |
| Scan live | Back camera with a scan frame; Settings → Demo can force the result |
| Speak to Feed iQ | Voice commands in English, Tamil and Hindi (“probe test”, “scan”, “add stock”, “can I feed today?”) |
| Silos | Stock list with total tonnes; **Add stock** to enter Bunker-1, bags and pits by hand |

Voice needs Chrome with microphone permission. It uses Google's speech service, so it needs internet.

## Files

```
index.html      the whole app (HTML, CSS, JS in one file)
manifest.json   app name, icons, full-screen mode
sw.js           offline cache
icons/          app icons (192, 512, maskable, Apple)
TECH_STACK.md   final software stack, models and datasets
```

## Deploy

The site is served by **GitHub Pages** from the `main` branch root (Settings → Pages → Deploy from a branch → `main` / root). Any push to `main` updates the live app. Bump `CACHE` in `sw.js` when you change the app, so installed phones pick up the new version.
