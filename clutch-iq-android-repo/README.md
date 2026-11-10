# Clutch IQ: The Game Sense Trainer

A game-sense trainer by **AFSR Games**. Players make real in-match decisions against a timer, then see the coach pick, why the other options are weaker, and how the crowd voted.

- Web app: https://clutch-iq.apper.so
- Android package: `com.afsr.clutchiq`
- Website: https://afsrpublishing.store
- Contact: afsrpublishing@gmail.com

This repository holds the files used to package the web app as an Android app (Trusted Web Activity) for Google Play, plus the store listing text.

| File | What it is |
|---|---|
| `manifest.json` | Web app manifest that Bubblewrap reads (name, colours, icons, start URL) |
| `assets/` | Play Store icon (512), maskable icon, 192 icon, feature graphic (1024x500) |
| `well-known/assetlinks.json` | Digital Asset Links template (the live copy is served by the app) |
| `store-listing.md` | Play Console listing text, Data safety and content rating answers |
| `privacy-policy.md` | Privacy policy text (live at https://clutch-iq.apper.so/privacy) |

---

## Build the Android app (Windows, Bubblewrap)

Same tools as before: Node v22 via nvm-windows and the Bubblewrap CLI.

### 1. Check the verification file is live
Open https://clutch-iq.apper.so/.well-known/assetlinks.json in a browser. You should see JSON with `com.afsr.clutchiq`. If you see the game's page or a 404 instead, stop: the domain needs the Netlify fallback before building.

### 2. Create the project
```powershell
mkdir C:\Users\princ\Desktop\clutch-iq
cd C:\Users\princ\Desktop\clutch-iq
nvm use 22
bubblewrap init --manifest https://raw.githubusercontent.com/princeasiff-glitch/CLUTCH-IQ-THE-GAME-SENSE-TRAINER/main/manifest.json
```

Answer the prompts like this:

| Prompt | Answer |
|---|---|
| Domain | `clutch-iq.apper.so` |
| URL path | `/play` |
| Application name | `Clutch IQ: The Game Sense Trainer` |
| Short name | `Clutch IQ` |
| Application ID | `com.afsr.clutchiq` |
| Starting version code | `1` |
| Display mode | `standalone` |
| Orientation | `portrait` |
| Status bar colour | `#0B0C10` |
| Splash screen colour | `#000000` |
| Icon URL / Maskable icon URL | keep the defaults it shows (from this repo) |
| Monochrome icon | leave blank |
| Include support for Play Billing | `No` |
| Request geolocation permission | `No` |
| Key store location | `C:\Users\princ\Desktop\clutch-iq\clutchiq.keystore` |
| Key name (alias) | `clutchiq-key` |
| First and last names | `AFSR Publishing` |
| Organisation / unit | `AFSR Games` |
| Country | `IN` |

Write the keystore password in a safe place. **Never upload the keystore or its password to GitHub.** Lose it and you cannot update the app.

### 3. Build
```powershell
bubblewrap build
```
This creates `app-release-bundle.aab` (upload this to Play) and `app-release-signed.apk` (install on your phone to test).

### 4. Get your upload key fingerprint
```powershell
bubblewrap fingerprint list
```
Or:
```powershell
keytool -list -v -keystore clutchiq.keystore -alias clutchiq-key
```
Copy the **SHA-256** value.

### 5. Put the fingerprint into the live app
Send the SHA-256 to Claude to update `/.well-known/assetlinks.json` in the Apper project, then **republish in Apper**.

### 6. Upload to Play Console
Create the app in Play Console as "Clutch IQ: The Game Sense Trainer", then go to Testing > Closed testing and upload the `.aab`. Fill in the listing from `store-listing.md`.

### 7. Add Google's signing fingerprint
In Play Console go to **Test and release > App integrity > App signing** and copy the **App signing key certificate SHA-256**. Add it as a second fingerprint in assetlinks and republish in Apper. Without it, the Play-installed app shows a browser address bar.

### 8. Closed testing
You need 12 testers opted in for 14 continuous days, then you can apply for production access.

---

Clutch IQ is an independent training app and is not affiliated with, endorsed by or sponsored by any game publisher. Game names are used only to identify which game a scenario relates to.

© 2026 AFSR Publishing. All rights reserved.
