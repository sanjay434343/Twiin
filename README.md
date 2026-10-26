<div align="center">

  <img src="logo.png" alt="Twiin app logo – offline digital card wallet for ID, Aadhaar, ATM and smart cards" width="120"/>

  # Twiin – Offline Digital Card Wallet for ID, Aadhaar, ATM & Smart Cards

  ### Scan any card. Get a perfect digital twin. 100% offline, encrypted, and yours.

  [![Flutter](https://img.shields.io/badge/Flutter-3.24%2B-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
  [![Dart](https://img.shields.io/badge/Dart-3.5%2B-0175C2?logo=dart&logoColor=white)](https://dart.dev)
  [![Platform](https://img.shields.io/badge/Platform-Android%20%7C%20iOS-3DDC84?logo=android&logoColor=white)](https://github.com/sanjay434343/twiin/releases)
  [![Encryption](https://img.shields.io/badge/Encryption-AES--256-darkgreen?logo=letsencrypt&logoColor=white)](#-security--privacy)
  [![Offline](https://img.shields.io/badge/Works-100%25%20Offline-orange)](#-why-twiin)
  [![ISO 7810 ID-1](https://img.shields.io/badge/Standard-ISO%2FIEC%207810%20ID--1-blue)](https://www.iso.org/standard/70483.html)
  [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
  [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

  <p>
    <a href="https://github.com/sanjay434343/twiin/releases"><b>⬇️ Download the APK</b></a> •
    <a href="#-key-features">Features</a> •
    <a href="#-how-twiin-digitizes-your-cards">How It Works</a> •
    <a href="#-security--privacy">Security</a> •
    <a href="#-faq">FAQ</a> •
    <a href="#-getting-started-build-from-source">Build From Source</a>
  </p>

</div>

---

**Twiin** is a free, open-source **digital card wallet** built with **Flutter** for Android and iOS. It turns your physical **ID cards, Aadhaar cards, ATM/debit cards, driver's licenses, loyalty cards, business cards, and smart cards** into clean, realistic, 3D-interactive **digital duplicates**, stored **offline** on your phone with **AES-256 encryption**. No account. No cloud. No tracking.

> **Why "Twiin"?** Two i's, on purpose: every card you scan gets an exact **twin**.

---

## 💡 Why Twiin?

| The problem | The Twiin solution |
|:---|:---|
| ❌ **Bulky wallets** and worn-out, easily lost plastic cards | ✅ **Every card in your pocket**: ID, Aadhaar, ATM, license, loyalty, business and smart cards |
| ❌ **Blurry gallery photos** with glare, shadows and crooked angles | ✅ **Computer-vision cleanup** that removes glare, fixes lighting and straightens geometry |
| ❌ **Cloud wallet apps** that upload your card data to remote servers | ✅ **100% offline, on-device storage** with AES-256 encryption and no cloud sync |
| ❌ **Barcodes that won't scan** on a dim phone screen | ✅ **Auto max-brightness** on the card detail screen, restored when you leave |
| ❌ **Flat, lifeless 2D images** | ✅ **Real 3D card physics**: tilt, specular shine and a true double-sided flip |

---

## ✨ Key Features

- 📷 **Smart card scanner**: capture any card and get a clean, print-quality digital copy
- 🪪 **Supports all card types**: Aadhaar and ID cards, ATM/debit/credit cards, driving licenses, smart cards, loyalty and business cards
- 🧠 **Multi-pass computer-vision pipeline**: auto-orientation, glare removal, contrast and saturation restoration, denoising and sharpening
- 📐 **ISO/IEC 7810 ID-1 accurate**: exact real-world card proportions and rounded corners
- 🎴 **3D interactive cards**: motion-sensor tilt, holographic-style shine, double-sided flip
- 🔆 **Scanner-friendly display**: automatic 100% brightness for barcodes and QR codes
- 🖨️ **Print & export**: arrange card position and size in the built-in editor and export to **PDF**
- 🔒 **Security locks**: biometrics (fingerprint / Face ID) with in-app PIN fallback
- 🚫 **Private by design**: no ads, no analytics, no accounts, no cloud

---

## 📸 Screenshots

<!-- Add real screenshots to /assets/screenshots and uncomment: -->
<!--
<p align="center">
  <img src="assets/screenshots/home.png" alt="Twiin home screen showing a wallet of digital cards" width="200"/>
  <img src="assets/screenshots/scan.png" alt="Twiin card scanner detecting and cleaning an ID card" width="200"/>
  <img src="assets/screenshots/card-3d.png" alt="Twiin 3D card detail with tilt and shine" width="200"/>
  <img src="assets/screenshots/export-pdf.png" alt="Twiin editor exporting cards to PDF" width="200"/>
</p>
-->

---

## 🔬 How Twiin Digitizes Your Cards

Most card-scanner apps just save a JPEG. Twiin runs a **deterministic, multi-pass computer-vision pipeline** inside a background Dart Isolate (so the UI stays smooth) to recover what your card *really* looks like, and strip away glare, shadows and noise.

```
Camera image
     │
     ▼
1. Auto-orientation + ISO 7810 geometry normalization
     ▼
2. Retinex illumination–reflectance decomposition (glare & shadow removal)
     ▼
3. CLAHE-style contrast + plastic saturation restoration
     ▼
4. Bilateral edge-preserving denoising
     ▼
5. Adaptive high-boost sharpening
     ▼
6. Anti-aliased rounded-corner mask + specular bevel
     ▼
Clean digital twin (SVG container + HD raster artifact)
```

<details>
<summary><b>📐 Show the math behind each step</b></summary>

### 1. ISO/IEC 7810 ID-1 geometry normalization

Standard ID, debit and credit cards follow the ID-1 format:

$$\text{Aspect Ratio} = \frac{85.60\ \text{mm}}{53.98\ \text{mm}} \approx 1.5858$$

$$r_{\text{corner}} = 3.18\ \text{mm} \quad (\approx 3.7\%\ \text{of card width})$$

If a portrait image is detected (height > width), the pixel buffer is rotated in $\mathcal{O}(N)$:

$$x_{\text{new}} = \text{Height} - 1 - y_{\text{old}}, \qquad y_{\text{new}} = x_{\text{old}}$$

### 2. Retinex illumination–reflectance decomposition

Observed intensity is modeled as surface reflectance times ambient light:

$$S(x, y) = R(x, y) \cdot L(x, y)$$

The card is divided into a $16 \times 10$ grid; the 82nd-percentile luminance of each cell estimates the local lighting, using

$$\text{Lum} = 0.299\,R + 0.587\,G + 0.114\,B$$

Lighting is interpolated bilinearly between grid points (coordinates normalized to grid-cell units):

$$L_{\text{local}}(x, y) = \sum_{i=0}^{1}\sum_{j=0}^{1} L(x_i, y_j)\,(1 - |x - x_i|)\,(1 - |y - y_j|)$$

A correction scalar then flattens shadows and flash hotspots:

$$\Phi(x, y) = \text{clamp}\!\left(\frac{L_{\text{target}}}{L_{\text{local}}(x, y)},\ 0.80,\ 1.35\right), \quad L_{\text{target}} = 242$$

$$R_{\text{norm}}(x, y) = \text{clamp}\big(S(x, y)\cdot\Phi(x, y),\ 0,\ 255\big)$$

### 3. Saturation restoration and tonal stretch

$$C_{\text{out}} = \text{Lum} + s\,(C_{\text{in}} - \text{Lum}), \quad s = 1.15$$

$$C_{\text{final}} = \begin{cases} \lfloor 0.90\,C_{\text{out}} \rfloor & \text{if } \text{Lum} < 115 \quad \text{(darker, crisper text)} \\ \lfloor (C_{\text{out}} - 210)\cdot 1.08 + 210 \rfloor & \text{if } \text{Lum} > 210 \quad \text{(cleaner highlights)} \end{cases}$$

### 4. Bilateral edge-preserving denoising

$$I_{\text{denoised}}(p) = \frac{1}{W_p}\sum_{q \in \Omega} I(q)\, f(\lVert p - q \rVert)\, g(|I(p) - I(q)|)$$

$$g(\Delta I) = \exp\!\left(-\frac{(\Delta I)^2}{2\sigma_r^2}\right), \quad \sigma_r = 24$$

Range weights are precomputed in a lookup table for $\mathcal{O}(1)$ access. A gradient guard protects text, barcodes and logos:

$$\nabla I = |\text{Lum}(x{+}1, y) - \text{Lum}(x, y)| + |\text{Lum}(x, y{+}1) - \text{Lum}(x, y)|$$

$$\text{if } \nabla I > 28:\quad I_{\text{denoised}}(p) = I(p)$$

### 5. Adaptive high-boost sharpening

$$I_{\text{sharp}} = I + \gamma(\nabla I)\,\big[I - I_{\text{box}}\big], \qquad \gamma = \begin{cases} 0.50 & \nabla I > 18 \ \text{(text, numbers)} \\ 0.15 & \nabla I \le 18 \ \text{(smooth plastic)} \end{cases}$$

### 6. Anti-aliased corner mask and specular bevel

With $d(x, y)$ as the signed distance from the corner arc (negative inside the card):

$$d(x, y) = \sqrt{(x - x_0)^2 + (y - y_0)^2} - r_{\text{corner}}$$

$$\alpha(x, y) = \text{clamp}\big(0.5 - d(x, y),\ 0,\ 1\big)$$

The card is wrapped in a scalable SVG container with a light-reflecting gradient:

```xml
<linearGradient id="bevelSpecular" x1="0%" y1="0%" x2="100%" y2="100%">
  <stop offset="0%"   stop-color="#FFFFFF" stop-opacity="0.45"/>
  <stop offset="60%"  stop-color="#FFFFFF" stop-opacity="0.12"/>
  <stop offset="100%" stop-color="#000000" stop-opacity="0.22"/>
</linearGradient>
```

</details>

---

## 🎮 3D Card Physics Engine

Open any card and Twiin runs a smooth, 60 FPS interactive 3D view:

- **Motion-sensor tilt**: the card leans and catches light as you move your phone
- **Silky smoothing**: an exponential low-pass filter removes jitter
- **True double-sided flip**: a 180° Y-axis flip with perspective, showing the back (magnetic stripe, barcode, QR code, signature)
- **Auto max-brightness**: barcode scanners, turnstiles and POS terminals read your screen on the first try; your original brightness is restored when you exit

<details>
<summary><b>📐 Show the tilt math</b></summary>

$$\theta_y = \text{clamp}\!\left(-\frac{a_x}{9.8}\times 0.16,\ -0.16,\ 0.16\right) \quad \text{(yaw)}$$

$$\theta_x = \text{clamp}\!\left(\frac{a_y - 4.5}{9.8}\times 0.12,\ -0.12,\ 0.12\right) \quad \text{(pitch)}$$

$$\theta_{\text{smooth}}(t) = \theta_{\text{smooth}}(t{-}1) + 0.15\,\big(\theta_{\text{target}}(t) - \theta_{\text{smooth}}(t{-}1)\big)$$

The flip uses a perspective projection with $M_{3,2} = 0.001$; at 90° the front face swaps to the back face.

</details>

---

## 🔒 Security & Privacy

```
┌──────────────────────────────────────────────────────┐
│                  Android / iOS                       │
├──────────────────────────────────────────────────────┤
│  Lock layer:  Biometrics (Fingerprint / Face ID)     │
│               + in-app PIN fallback (4 or 6 digits)  │
├──────────────────────────────────────────────────────┤
│  App-private sandboxed vault  (.twiin_vault)         │
│  AES-256 encryption  •  SHA-256 hashed + salted PIN  │
│  No ads  •  No analytics  •  No cloud sync           │
└──────────────────────────────────────────────────────┘
```

- **Offline-first**: your cards never leave your device
- **Sandboxed vault**: files live in the app's private storage, inaccessible to other apps
- **Double-layer lock**: native biometrics with PIN fallback
- **Brute-force protection**: lock time increases after repeated wrong attempts
- **Memory hygiene**: sensitive keys and card numbers are cleared from RAM when the app goes to the background

> 🛡️ Found a vulnerability? Please open a private security advisory on GitHub instead of a public issue.

---

## 📦 Small APK, Fast Install

Twiin ships with an aggressive release-optimization setup that brings the binary from **100 MB+ down to roughly 25 MB**:

1. **Compressed native libraries** (`useLegacyPackaging = true`)
2. **Resource shrinking** (`isShrinkResources = true`)
3. **R8 code shrinking and optimization**
4. **Locale pruning** to remove unused translations from third-party SDKs
5. **Per-ABI split APKs** for each CPU architecture

```bash
flutter build apk --release --split-per-abi --obfuscate --split-debug-info=build/app/outputs/symbols
```

---

## 🚀 Getting Started (Build From Source)

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install) 3.24.0 or higher
- Android SDK (API level 26+)
- Java 17+

### Run it

```bash
# 1. Clone
git clone https://github.com/sanjay434343/twiin.git
cd twiin

# 2. Install dependencies
flutter pub get

# 3. Run on a connected device or emulator
flutter run

# 4. Build a release APK
flutter build apk --release --split-per-abi
```

---

## ❓ FAQ

**What is Twiin?**
Twiin is a free, open-source Flutter app that scans physical cards and stores them as clean, encrypted digital copies on your phone, fully offline.

**Does Twiin work without internet?**
Yes. Scanning, storage, viewing and PDF export all work offline.

**Which cards can I store?**
ID cards, Aadhaar cards, ATM/debit/credit cards, driving licenses, smart cards, loyalty cards and business cards.

**Is my card data uploaded anywhere?**
No. Twiin has no cloud sync, no accounts and no analytics. Everything stays in the app's encrypted private storage.

**Can I print my cards?**
Yes. Use the built-in editor to position and size your cards, then export a PDF to print.

**Does it work on iPhone?**
Twiin is built with Flutter for both Android and iOS.

**Is Twiin free?**
Yes, it's open source under the MIT license.

---

## 🤝 Contributing

Contributions are welcome and appreciated!

1. Fork the project
2. Create a feature branch: `git checkout -b feature/AmazingFeature`
3. Commit your changes: `git commit -m 'Add AmazingFeature'`
4. Push the branch: `git push origin feature/AmazingFeature`
5. Open a Pull Request

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## ⭐ Support the Project

If Twiin is useful to you, **give it a star** ⭐ and share it. It helps more people discover a private, offline alternative to cloud wallet apps.

---

## 📄 License

Distributed under the **MIT License**. See [LICENSE](LICENSE).

## 👨‍💻 Author

Built by **Sanjay** · [GitHub](https://github.com/sanjay434343) · [Portfolio](https://sanjay-m.netlify.app)

---

<div align="center">
  <sub>Twiin: the private, offline digital card wallet. Engineered with precision for absolute privacy.</sub>
</div>

<!--
Search keywords: digital card wallet, offline wallet app, ID card scanner, Aadhaar card holder app, ATM card storage app, smart card digital copy, document scanner Flutter, secure card vault, AES-256 encrypted wallet, 3D card viewer, open source wallet app, Flutter wallet app, privacy-first wallet, card to PDF
-->
