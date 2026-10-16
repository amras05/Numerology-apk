# 🔮 Numerology Calculator

A modern, cross-platform **Numerology Calculator** built with **Vue 3**, **Quasar Framework**, and **Capacitor**. Calculates accurate numerological insights using both the **Chaldean** and **Pythagorean** systems with an elegant glassmorphism user interface.

Available as a **Web App**, **Android APK**, and **Desktop Application** (Windows / Linux).

---

## ✨ Features

- **Dual Numerology Systems:**
  - **Chaldean System:** Ancient mystical vibration system (numbers 1–8).
  - **Pythagorean System:** Western numerology system (numbers 1–9).
- **Flexible Input Support:**
  - Accepts full names, words, and direct numerical digits (digits are seamlessly included in the calculation sum and reduction).
- **Date of Birth & Life Path Calculation:**
  - Date picker with `DD-MM-YYYY` formatting.
  - Computes **Birth Number** (Driver / Psychic) and **Destiny / Life Path Number** (Conductor).
- **In-Depth Breakdown:**
  - **Compound & Single Root Numbers:** Shows both master/compound vibration and reduced single root value.
  - **Vowels & Consonants Analysis:** Filter and inspect Soul Urge (Vowels) and Personality (Consonants) numbers.
  - **Interactive Letter Grid:** Real-time breakdown showing each character with its numerical value.
- **Glassmorphism Design & Responsive UI:**
  - Modern dark mode and light mode with one-click toggle.
  - Fully responsive across mobile phones, tablets, and wide desktop screens.
- **Cross-Platform Deployment:**
  - 📱 **Android:** Powered by Capacitor (supports debug & release APKs).
  - 🖥️ **Desktop:** Electron build support (Windows `.exe`, Linux `.AppImage`).
  - 🌐 **Web:** Blazing-fast Vite SPA.

---

## 🚀 Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18 or higher recommended)
- [npm](https://www.npmjs.com/) or [pnpm](https://pnpm.io/)
- For Android builds: [Android Studio](https://developer.android.com/studio) or Android SDK + JDK 17/21

### Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/amras05/Numerology-apk.git
cd Numerology-apk
npm install
```

---

## 🛠️ Development & Running

### Run the Web App locally

```bash
npx quasar dev
```
Open your browser at `http://localhost:9000`.

### Run / Sync Android (Capacitor)

```bash
# Build web assets and copy to Android project
npx quasar build -m capacitor -T android

# Open project in Android Studio
npx cap open android
```

### Build Android APK directly via Gradle

```bash
cd src-capacitor/android
./gradlew assembleDebug
```
The generated APK will be located at:
`src-capacitor/android/app/build/outputs/apk/debug/app-debug.apk`

---

## 📦 Project Structure

```
├── public/                 # Static assets & icons
├── src/
│   ├── assets/             # Brand assets and graphics
│   ├── css/                # Scss styling and Quasar design tokens
│   ├── layouts/            # Quasar layout wrappers
│   ├── pages/
│   │   └── IndexPage.vue   # Main Numerology calculation engine & UI
│   ├── router/             # Vue Router routes
│   └── App.vue             # Root component
├── src-capacitor/          # Android Capacitor project & native configurations
├── src-electron/           # Electron desktop wrapper scripts
├── quasar.config.js        # Quasar & Vite build configuration
└── package.json            # Node.js dependencies and scripts
```

---

## 🔢 Numerology Systems Reference

### Chaldean System
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|
| A, I, J, Q, Y | B, K, R | C, G, L, S | D, M, T | E, H, N, X | U, V, W | O, Z | F, P |

*(Number 9 is considered sacred in Chaldean numerology and is excluded from the alphabet mapping, but emerges in sums)*

### Pythagorean System
| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|
| A, J, S | B, K, T | C, L, U | D, M, V | E, N, W | F, O, X | G, P, Y | H, Q, Z | I, R |

---

## 📄 License

This project is licensed under the terms included in the [LICENSE](LICENSE) file.
