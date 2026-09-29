# Feed iQ — final software stack

Team X · Smart India Hackathon 2026 · SIH26111

## App
- Installable web app (PWA): HTML, CSS, vanilla JavaScript, hosted on GitHub Pages, works offline
- On-phone AI: ONNX Runtime Web
- Probe → phone: Web Bluetooth (BLE)
- Voice input and read-aloud: Web Speech API (English, Tamil, Hindi)
- Storage on the phone: IndexedDB

## Probe firmware (Seeed XIAO ESP32-S3)
- Arduino C++
- NimBLE (Bluetooth), Adafruit_AS7341 (NIR light sensor), ModbusMaster (pH / moisture / temperature / EC probe)

## AI models — three models and one rule layer
| Model | Input | Output | Built with |
|---|---|---|---|
| M1 Photo → mould | Silage photo | Good / Mould / Spoiled + heat-map of mould | MobileNetV3-Small in PyTorch; Captum Grad-CAM |
| M2 Sensors → quality | pH, moisture, core temp, air temp, temp rise, EC, days open, crop | Good / Watch / Spoiled + score | Extra Trees classifier (scikit-learn); SHAP explains why |
| M3 Light → nutrients | 11 AS7341 channels | DM, CP, NDF, ADF, starch | PLS regression, one per nutrient |
| Decision rules | M1 + M2 + M3 | Final verdict, "use within N days" | Runs on the phone |

- Energy is calculated from ADF: NEL = 2.30 − 0.0262 × ADF (Mcal/kg DM), TDN = 88.9 − 0.779 × ADF (%).
- Final verdict = the worse of M1 and M2.

### Why Extra Trees for M2
- Random split points plus averaging over many trees means less overfitting on our small, synthetic sensor data.
- Works well with default settings, so there is little tuning.
- SHAP TreeExplainer supports it directly, and it exports to ONNX for the phone.

## Farmer advice
- Result screens: fixed advice sentences in English, Tamil and Hindi, picked by the top SHAP reasons. Works offline and cannot make things up.
- "Ask Feed iQ" chat (online only): Gemini `gemini-3.5-flash-lite`, given only that test's data.

## Cloud
- Firebase Firestore (records, silo stock) and phone-OTP login
- FastAPI server on Google Cloud Run (SHAP + chat)

## Datasets
| For | Dataset |
|---|---|
| M1 photos | Our own farm photos, plus open Wikimedia Commons silage/mould photos (the largest open set is only ~99 images), plus synthetic mould images blended onto clean silage. Test set is real photos only. |
| M2 sensors | Synthetic: 10,000 rows from published silage ranges, following the SIH26111 dummy-data columns |
| M3 nutrients | Synthetic, to prove the pipeline. Real calibration needs lab-tested samples read by our AS7341. |

Sensor and nutrient data are synthetic for now; nutrient numbers are screening estimates, not lab results.
