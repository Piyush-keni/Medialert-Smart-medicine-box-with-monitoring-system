# Smart Medicine Dispenser – Remote Care Console

IoT dashboard (ESP32 + Firebase Realtime Database) for scheduling doses and reviewing adherence.

## Pages
- `index.html` – overview
- `schedule.html` – write/read dose schedules
- `adherence.html` – adherence analytics (demo data)
- `hardware.html` – hardware, KiCad schematic/PCB, tech stack

## Before publishing
0. Firebase Console -> Authentication -> enable Email/Password. Create TWO users: a caregiver and a device account. Copy their UIDs into `database.rules.json` (replace `REPLACE_WITH_CAREGIVER_UID` / `REPLACE_WITH_DEVICE_UID`). Disable public sign-up (Authentication -> Settings -> User actions).
1. Publish `database.rules.json` in Firebase Console -> Realtime Database -> Rules. Without this step the database stays locked and nothing works.
2. Restrict the API key to your GitHub Pages domain (Google Cloud Console -> Credentials -> HTTP referrers).
3. Add your images: `sch.png`, `pcb.png`, `fr.png` in the root, and component photos in `images/` (esp32.jpg, uln2003.jpg, lcd.jpg, ir_sensor.jpg, buzzer_led.jpg, power_supply.jpg). Strip EXIF first: `exiftool -all= file.png`.
4. Never commit ESP32 Wi-Fi credentials; keep them in `secrets.h` (git-ignored).

## Run locally
Open `index.html`, or serve with `python3 -m http.server`.

## Deploy
GitHub repo -> Settings -> Pages -> Deploy from branch `main` / root.

All adherence data shown is sample data.

## Firmware
`firmware/POS/POS.ino` – copy `secrets.example.h` to `secrets.h` and fill in your Wi-Fi and Telegram details. Never commit `secrets.h`.

## Security notes
- Schedules and logs are readable/writable only by the caregiver / device accounts (see rules).
- ESP32 signs in with its own account (`DEVICE_EMAIL` / `DEVICE_PASSWORD` in `secrets.h`).
- Remaining hardening: the firmware still calls `setInsecure()` (no TLS certificate check). Replace with `client.setCACert(<root CA PEM>)` for Google (GTS Root R1) and Telegram, and test on your board. Also enable Firebase App Check, and rotate the Telegram bot token if it was ever committed.
