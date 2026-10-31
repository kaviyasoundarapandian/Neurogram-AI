# Test Cases — NeuroGram AI

Environment: Chrome/Edge (desktop or Android) opening `src/index.html`, or the APK build `base_12`.
How to run a console test: open DevTools (F12) > Console, run the `ingest(...)` line from `data/sample_sensor_data.json`, wait about 5-10 seconds (firing rates are averaged over a 20-step window), then check the expected result. Reload the page to reset.
Status column: fill in Pass/Fail when you run each case.

| ID | Scenario | Input / Action | Expected result | Status |
|---|---|---|---|---|
| T01 | App starts offline | Disable network, open app | App loads, AI Brain shows spike raster, activity log shows start message | |
| T02 | Dry soil triggers irrigation | `ingest({soil:22,temp:37,rain:0})` | Irrigate neuron fires, log says soil is dry and irrigation started, Farm valve shows ON | |
| T03 | Rain inhibits irrigation | `ingest({soil:22,temp:30,rain:0.9})` | Irrigate does not stay ON (rain inhibits) | |
| T04 | Low tank starts pump | `ingest({tank:15})` | Fill-tank neuron fires, log says tank is low and pump started | |
| T05 | Flood warning | `ingest({river:5.5,rain:0.9,drain:70})` | Flood alert fires, voice and vibration alert, log shows flood warning | |
| T06 | Pipe leak | `ingest({leak:true,drop:0.3})` | Leak alert fires and appears in Alerts | |
| T07 | Drain clog | `ingest({drain:75,clog:85})` | Open-drain / clog alert fires, Drainage screen updates | |
| T08 | Bins full | `ingest({bin:95})` | Bin pickup neuron fires, Waste screen shows pickup | |
| T09 | Night lighting | `ingest({hour:21})` | Lights-on neuron fires | |
| T10 | Low battery | `ingest({bat:12})` | Save-energy neuron fires | |
| T11 | Hysteresis | After T02 set `ingest({soil:70})` | Irrigation turns OFF and log says soil is wet enough; no rapid on/off flicker | |
| T12 | Live mode | Call any `ingest(...)` | Simulator stops updating values (live mode) | |
| T13 | Scenario buttons | Tap Monsoon harvest, Pipe leak, Storm drain, Drain clog, Bins full, Heatwave | Each triggers the matching AI reaction on the AI Brain and relevant screen | |
| T14 | Manual override | Farm > set valve to On, then Off, then Auto | Valve follows override, Auto returns control to AI | |
| T15 | Voice language | Switch language to English, Hindi, Tamil, Telugu and trigger an alert | Alert spoken in the selected language (device TTS voice must be installed) | |
| T16 | Voice command | Tap Voice Command and speak a command | Command recognised and screen/action responds; unsupported browser says voice recognition is not supported | |
| T17 | Accessibility | Toggle Large Text, Dyslexia, High Contrast, Easy mode | UI changes immediately for each mode | |
| T18 | Read screen / repeat | Tap Read Screen, then Repeat | Current screen is read aloud, last alert is repeated | |
| T19 | Grievance ticket | Grievance screen: choose category, enter text, submit | New ticket appears in the list with an incremented ticket number | |
| T20 | Scheme finder | Schemes screen: set profile and filter | PM-KISAN, JJM, PMFBY results filter accordingly | |
| T21 | SOS | Tap Emergency SOS | Confirmation shown, then disaster alert, siren action and vibration | |
| T22 | Crop target | Farm: select Tomato, Rice, Wheat, Millet | Target moisture updates (55, 70, 45, 35) | |

## Test data
Use `data/sample_sensor_data.json` for inputs.

## Notes
Tests are manual because the app is a single-file client with no build step. Expected results are derived from the SNN weights and thresholds in `src/index.html`.
