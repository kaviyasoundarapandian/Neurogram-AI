# NeuroGram AI — Neuromorphic Edge-AI Smart Village Ecosystem

**Team:** NeuroGram AI  |  **Team ID:** HSC|TN|00977  |  **State:** Tamil Nadu
**Theme:** AI, GIS & Emerging Technologies  |  **Event:** Hack for Social Cause

An offline-first Android app with an on-device spiking neural network (SNN) "brain" that watches village sensors (soil, rain, tank, river, drains, bins, power) and automatically decides irrigation, pumping, sluice gates, aquifer recharge, bin pickup and disaster alerts. Voice alerts in English, हिन्दी, தமிழ், తెలుగు; large-text, dyslexia and high-contrast modes; grievance tickets for the Gram Panchayat; scheme finder (PM-KISAN, JJM, PMFBY).

## Key features
- 16 sensor neurons -> 14 decision neurons (leaky integrate-and-fire), runs on the device, no cloud, no API calls at run time
- 10 screens: AI Brain, Farm, Water, Harvest, Drainage, Energy, Waste, Grievance, Schemes, Alerts
- Spikes only on change, so early leak, flood and dry-spell alerts with low compute
- Accessibility: voice commands, read-screen, repeat alert, large text, dyslexia font, high contrast, vibration and chimes
- Built-in simulator with 1 s = 6 minutes of village time, and scenarios: Monsoon harvest, Pipe leak, Storm drain, Drain clog, Bins full, Heatwave
- Real hardware hook: `window.ingest({...})` (ESP32 / Raspberry Pi)

## Repository layout
```
src/index.html              Complete app (HTML5 + CSS3 + JavaScript, single file)
docs/TECHNICAL_DESIGN.md    Architecture, data flow, SNN design, diagrams
data/sample_sensor_data.json  Sample sensor payloads for ingest()
tests/TEST_CASES.md         Manual and console test cases with expected results
LICENSE                     MIT
```

## Setup

### Option A — Run in a browser (fastest)
1. Clone: `git clone https://github.com/kaviyasoundarapandian/Neurogram-AI.git`
2. Open `src/index.html` in Chrome or Edge (or serve it: `python3 -m http.server 8000` inside `src/`, then visit `http://localhost:8000`).
3. Use a Chromium browser for voice input (Web Speech API).

### Option B — Install the Android APK
1. Download the APK (prototype build `base_12`) and copy it to the phone.
2. Allow "Install unknown apps" for your file manager, then open the APK.
3. The app works with no internet connection.

### Option C — Build your own APK (WebView wrapper)
1. Create an Android Studio project (Kotlin, min SDK 24 recommended).
2. Copy `src/index.html` to `app/src/main/assets/index.html`.
3. Load it in a `WebView` with JavaScript and DOM storage enabled:
   `webView.loadUrl("file:///android_asset/index.html")`.
4. Optional native bridge: expose an `AndroidTTS` JavaScript interface with `speak(text, lang)`, `listen(lang)` and `vibrate(type)`. The app falls back to the browser Web Speech API and `navigator.vibrate` if the bridge is absent.

## Environment variables
**None required.** The app has no API keys, no cloud services and no server. Nothing needs configuring beyond the steps above. Do not commit signing keystores (`.gitignore` excludes them).

## Connecting real sensors
Replace the simulator by pushing readings into the app (for example from an ESP32 through a local bridge or the WebView):
```js
window.ingest({ soil: 31, temp: 34, tank: 28, river: 4.2, rain: 0.1 });
```
Once `ingest()` is called, the simulator stops (`live = true`). Accepted fields are listed in `docs/TECHNICAL_DESIGN.md` and examples are in `data/sample_sensor_data.json`.

## Testing
See `tests/TEST_CASES.md`. Tests are manual and console-driven scenarios, each with the expected AI decision.

## Documentation
Architecture, data flow and SNN design: `docs/TECHNICAL_DESIGN.md`.

## Limitations and roadmap
- Prototype thresholds are tuned on simulated data, not field data
- Roadmap: field pilot in one Tamil Nadu village, scale to more panchayats, add GIS village map and live ESP32 data

## Team
- **Kaviya S** — concept, development, testing, presentation (Final-year B.Tech Civil Engineering, Gandhigram Rural Institute)

## License
MIT — see `LICENSE`.
