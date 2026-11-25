# MeterCheck Pro

Offline-first utility reading and bill-estimation starter for Android. Built with Vite, JavaScript, Capacitor and Tesseract.js.

## Current starter features
- Electricity and water meter reading records
- Manual reading entry and consumption calculation
- Editable provider names, location, rates and fixed charges
- Check-meter camera capture / image selection, OCR suggestions, and saved meter photos
- Provider-bill camera capture / image selection, OCR suggestions for amount due and consumption, bill-photo history, and manual verification
- On-device OCR attempt with Tesseract.js
- Local browser storage, history, CSV export and JSON backup/restore
- PWA shell caching

## Important limitations
- Camera capture works through the device file/camera picker in supported browsers and Capacitor builds. OCR uses Tesseract.js; its worker/language data may need internet on first use. This starter does not yet guarantee first-launch fully offline OCR. To guarantee it, bundle the worker, WASM and English traineddata with the app and test with airplane mode enabled.
- This starter uses localStorage, not SQLite yet. Compressed photos are stored with records, but large photo collections can fill browser storage. Export a backup regularly and do not clear app storage before backing up.
- Provider list is a selectable starting list, not a definitive service-area map. Verify the provider on the household's latest bill. Rates are editable estimates and do not represent official live tariffs.
- First reading for each utility establishes a baseline; subsequent readings calculate the difference from the latest saved reading.
- Bill OCR is a suggestion tool only. It does not authenticate a provider bill, confirm payment, or guarantee the correct total. Always verify amount due and usage against the original statement. Tiered tariffs, taxes, lifeline discounts, demand charges, water minimum charges and other provider-specific billing rules need implementation from current official tariff schedules before treating estimates as accurate.

## Termux setup
1. Install Termux from a trusted source and open it.
2. Run:

```sh
pkg update -y && pkg upgrade -y
pkg install nodejs-lts git unzip -y
```

3. Extract this ZIP into your Termux home directory (or clone your GitHub repository).
4. In the project folder run:

```sh
npm install
npm run dev -- --host 0.0.0.0
```

5. Open the local URL printed by Vite in your phone browser.

## Android APK build
Capacitor Android builds normally require Android Studio / Android SDK and a compatible JDK. Termux-only Android builds can be difficult. The recommended route is GitHub Actions with Java 21 and Android SDK to build the APK, then download it from the Actions artifact or GitHub Release.

```sh
npm run build
npx cap add android
npx cap sync android
```

## GitHub
Create an empty repository named `MeterCheck-Pro`, then:

```sh
git init
git add .
git commit -m "Initial MeterCheck Pro app"
git branch -M main
git remote add origin https://github.com/xev28/MeterCheck-Pro.git
git push -u origin main
```

## Automatic APK build
The included GitHub Actions workflow (`.github/workflows/android.yml`) builds a debug APK on every push to `main` and when manually started. After a successful run, open the repository's **Actions** tab, select the latest **Build MeterCheck Pro APK** run, and download the `MeterCheck-Pro-debug-apk` artifact.


## Updated camera workflows
- **Capture check meter:** choose/capture a meter photo, run OCR for a suggested reading, verify the digits, then save the reading with its compressed photo.
- **Capture provider bill:** choose/capture an electricity or water bill photo, run OCR for suggested total amount and consumption, manually verify/correct the fields, then save the bill image and bill details locally.
- **Compare bill:** enter the provider's total amount to compare it with the latest saved app estimate for that utility. The difference is informational only; it does not validate the provider's calculation.

## Publish to GitHub from Windows PowerShell
Create an empty public repository named `MeterCheck-Pro` under the GitHub account `xev28` (do not initialize it with a README, license, or .gitignore). In the extracted project folder, run:

```powershell
git init
git add .
git commit -m "Initial MeterCheck Pro with meter and bill camera capture"
git branch -M main
git remote add origin https://github.com/xev28/MeterCheck-Pro.git
git push -u origin main
```

If Git asks you to sign in, complete the browser authentication prompt. After pushing, open the repository's Actions tab to monitor the APK workflow.

To publish a downloadable GitHub Release after the first successful push, create and push a version tag:

```powershell
git tag v1.0.0
git push origin v1.0.0
```

The workflow builds the APK and attaches it to the GitHub Release for that tag. A normal push to `main` also creates a downloadable artifact under **Actions**.
