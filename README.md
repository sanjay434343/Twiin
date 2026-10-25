<div align="center">

  <img src="https://raw.githubusercontent.com/user/repository/main/assets/brand_logo.png" alt="Twiin Logo" width="120" onerror="this.src='https://img.icons8.com/fluency/144/credit-card-front.png'"/>

  # Twiin
  ### The Hardware-Secured, Real-Physics Digital Card Wallet

  [![Flutter](https://img.shields.io/badge/Flutter-3.24%2B-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
  [![Dart](https://img.shields.io/badge/Dart-3.5%2B-0175C2?logo=dart&logoColor=white)](https://dart.dev)
  [![Platform](https://img.shields.io/badge/Platform-Android%20%7C%20iOS-3DDC84?logo=android&logoColor=white)](https://android.com)
  [![Security](https://img.shields.io/badge/Security-256--bit%20AES%20%2B%20Offline%20Vault-darkgreen?logo=shield)](https://github.com)
  [![ISO Standard](https://img.shields.io/badge/Standard-ISO%2FIEC%207810%20ID--1-blue)](https://www.iso.org/standard/70483.html)
  [![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
  [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

  <p align="center">
    <b>Transform physical cards into hyper-realistic, 3D interactive, mathematically enhanced digital twins.</b><br/>
    <i>100% Offline • Hardware Sandboxed • Zero Telemetry • Instant 100% Screen Brightness</i>
  </p>

  <p align="center">
    <a href="#-why-twiin">Why Twiin</a> •
    <a href="#-scientific-digitalization-pipeline">Scientific Formulas</a> •
    <a href="#-key-features">Features</a> •
    <a href="#-security-architecture">Security Vault</a> •
    <a href="#-getting-started">Installation</a> •
    <a href="#-apk-size-optimization">Ultra-Compressed Build</a>
  </p>

</div>

---

## 💡 Why Twiin?

| The Problem with Physical & Digital Wallets | The Twiin Solution |
|:---|:---|
| ❌ **Bulky Wallets**: Losing physical cards, worn-out plastic, peeling magnetic stripes. | ✅ **Ultra-Lightweight Digital Twins**: Every ID, debit, license, loyalty, and business card in your pocket. |
| ❌ **Blurry Photo Gallery Scans**: Dark, crooked photos with harsh flash hotspots and bad angles. | ✅ **Multi-Pass Computer Vision**: Restores degraded plastic, strips ambient glare, and normalizes geometry. |
| ❌ **Cloud Surveillance**: Big tech wallet apps upload card numbers, dates, and spending habits to remote servers. | ✅ **Zero-Knowledge Kernel Sandbox**: 100% offline local storage with 256-bit AES cryptographic encryption. |
| ❌ **Barcode/POS Scanning Failures**: Dim screens cause cashier scanners and optical terminals to fail. | ✅ **Auto-100% HDR Brightness**: Instantly overrides display to 100% luminance when viewing card details. |
| ❌ **Lifeless Flat Images**: Conventional wallet apps show static 2D rectangles. | ✅ **Physical 3D Gyroscope Engine**: True card depth, specular holographic shine, and real physical flips. |

---

## 🔬 Scientific Digitalization Pipeline & Mathematical Formulas

Unlike ordinary card scanner apps that simply take a JPEG photo, **Twiin** executes a multi-pass, deterministic computer vision pipeline inside a dedicated Dart background Isolate. It mathematically extracts the intrinsic material properties of the card while stripping environmental noise.

```
Raw Camera Input (YCbCr) 
         │
         ▼
[ 1. Auto-Orientation & ISO 7810 Geometry Normalization ]
         │
         ▼
[ 2. Retinex Illumination-Reflectance Decomposition ] 
         │
         ▼
[ 3. CLAHE Local Contrast & Plastic Saturation Boost ]
         │
         ▼
[ 4. Bilateral Edge-Preserving Spatial Denoising ]
         │
         ▼
[ 5. Adaptive Shock & High-Boost Laplacian Sharpening ]
         │
         ▼
[ 6. Vector Chamfer Bevel & Subpixel Anti-Aliased Mask ]
         │
         ▼
Lossless SVG Container + GPU-Accelerated HD Artifact
```

---

### 1. ISO/IEC 7810 ID-1 Planar Geometry Normalization
All standard debit, credit, driver's licenses, and identity cards conform strictly to the international ISO/IEC 7810 ID-1 standard. Twiin enforces exact physical aspect ratio constraints:

$$\text{Aspect Ratio}_{\text{ISO}} = \frac{85.60\text{ mm}}{53.98\text{ mm}} \approx 1.5857725$$

$$\text{Corner Radius } r_{\text{corner}} = 3.18\text{ mm} \quad (\approx 3.75\% \text{ of card width})$$

If portrait orientation is detected ($\text{Height} > \text{Width}$), an orthographic coordinate transpose rotates the pixel buffer in $\mathcal{O}(N)$ time:

$$x_{\text{new}} = \text{Height} - 1 - y_{\text{old}}, \quad y_{\text{new}} = x_{\text{old}}$$

---

### 2. Retinex Illumination-Reflectance Decomposition
Photographs taken under indoor warm lighting, flash glare, or hand shadows suffer from uneven ambient radiance. Twiin models observed pixel intensity $S(x, y)$ as the product of intrinsic material reflectance $R(x, y)$ and environmental illuminance $L(x, y)$:

$$S(x, y) = R(x, y) \cdot L(x, y)$$

To recover the true, clean card surface $R(x, y)$, Twiin subdivides the card plane into a $16 \times 10$ sampling grid and calculates the 82nd percentile luminance ($P_{82}$) of local background plastic:

$$\text{Lum}(x, y) = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B$$

Continuous field luminance $L_{\text{local}}(x, y)$ is computed across the matrix using bilinear interpolation:

$$L_{\text{local}}(x, y) = \sum_{i=0}^{1}\sum_{j=0}^{1} L(x_i, y_j) \cdot (1 - |x - x_i|) \cdot (1 - |y - y_j|)$$

The Retinex normalization scalar $\Phi(x, y)$ strips shadows and flash hotspots:

$$\Phi(x, y) = \text{clamp}\left( \frac{L_{\text{target}}}{L_{\text{local}}(x, y)}, 0.80, 1.35 \right), \quad \text{where } L_{\text{target}} = 242.0$$

$$R_{\text{norm}}(x, y) = \text{clamp}\left( S(x, y) \cdot \Phi(x, y), 0, 255 \right)$$

---

### 3. PVC Saturation Restoration & CLAHE Dynamic Range Stretch
Scanners and digital cameras often desaturate printed plastic finishes. Twiin applies a chromatic saturation vector transformation around the local achromatic axis:

$$C_{\text{out}} = \text{Lum} + s \cdot (C_{\text{in}} - \text{Lum}), \quad \text{with } s = 1.15$$

To make stamped cardholder numbers and micro-text razor sharp, Twiin stretches the dynamic tonal range:

$$\text{Text Darkening: } \quad C_{\text{final}} = \lfloor 0.90 \cdot C_{\text{out}} \rfloor \quad \text{if } \text{Lum} < 115$$

$$\text{Highlight Cleaning: } \quad C_{\text{final}} = \lfloor (C_{\text{out}} - 210) \cdot 1.08 + 210 \rfloor \quad \text{if } \text{Lum} > 210$$

---

### 4. Bilateral Edge-Preserving Denoising
Gaussian blur removes camera sensor noise but damages crisp text edges. Twiin uses an edge-preserving bilateral filter that combines spatial geometric proximity with radiometric photometric difference:

$$I_{\text{denoised}}(p) = \frac{1}{W_p} \sum_{q \in \Omega} I(q) \cdot f(\|p - q\|) \cdot g(|I(p) - I(q)|)$$

Where photometric range weights are pre-computed in an $\mathcal{O}(1)$ lookup table:

$$g(\Delta I) = \exp\left( -\frac{(\Delta I)^2}{2\sigma_r^2} \right), \quad \sigma_r = 24.0$$

A gradient magnitude threshold protects all embossed letters, barcodes, and logos from blurring:

$$\nabla I = |\text{Lum}(x+1, y) - \text{Lum}(x, y)| + |\text{Lum}(x, y+1) - \text{Lum}(x, y)|$$

$$\text{If } \nabla I > 28, \quad I_{\text{denoised}}(p) = I(p) \quad \text{(100\% Stroke Preservation)}$$

---

### 5. Adaptive Shock & High-Boost Laplacian Sharpening
To restore physical ink boundaries, Twiin computes an adaptive high-boost difference filter:

$$I_{\text{sharp}}(x, y) = I(x, y) + \gamma(\nabla I) \cdot \left[ I(x, y) - I_{\text{box}}(x, y) \right]$$

Where the sharpening gain $\gamma$ dynamically scales:

$$\gamma(\nabla I) = \begin{cases} 0.50 & \text{if } \nabla I > 18 \quad \text{(Text strokes, CVV, numbers)} \\ 0.15 & \text{if } \nabla I \le 18 \quad \text{(Smooth plastic backgrounds)} \end{cases}$$

---

### 6. Subpixel Anti-Aliased Corner Masking & Chamfer Bevel
To eliminate aliasing (jagged edges) around the rounded corners, each pixel's distance from the four corner circle origins is computed:

$$d(x, y) = \sqrt{(x - x_0)^2 + (y - y_0)^2} - r_{\text{corner}}$$

$$\alpha(x, y) = \text{clamp}\left(1.0 - d(x, y), 0.0, 1.0\right)$$

The final result is enclosed within a scalable SVG container with a 3D specular light-reflecting gradient:

```xml
<linearGradient id="bevelSpecular" x1="0%" y1="0%" x2="100%" y2="100%">
  <stop offset="0%" stop-color="rgba(255,255,255,0.45)"/>
  <stop offset="60%" stop-color="rgba(255,255,255,0.12)"/>
  <stop offset="100%" stop-color="rgba(0,0,0,0.22)"/>
</linearGradient>
```

---

## 🎮 3D Real-Physics & Interaction Engine

When inspecting a card in the **Card Detail Screen**, Twiin runs a 60 FPS real-physics simulation:

### 1. Real-Time Accelerometer & Gyroscope 3D Tilt
Using device motion sensors, the card leans and catches ambient light according to real-world gravitational tilt:

$$\theta_y = \text{clamp}\left( -\frac{a_x}{9.8} \times 0.16, -0.16, 0.16 \right) \quad \text{(Yaw)}$$

$$\theta_x = \text{clamp}\left( \frac{a_y - 4.5}{9.8} \times 0.12, -0.12, 0.12 \right) \quad \text{(Pitch)}$$

An exponential low-pass filter ensures ultra-silky movement with zero jitter:

$$\theta_{\text{smooth}}(t) = \theta_{\text{smooth}}(t-1) + 0.15 \cdot \left( \theta_{\text{target}}(t) - \theta_{\text{smooth}}(t-1) \right)$$

### 2. Double-Sided 3D Card Flip
A physical $180^\circ$ Y-axis flip with a 3D perspective projection matrix ($M_{3,2} = 0.001$). At $90^\circ$, back-face culling swaps the front face for the back face (magnetic stripe, barcode, QR code, signature strip).

### 3. Automatic 100% HDR Display Brightness
The moment you open any card's detail page, Twiin automatically commands native window hardware to **100% maximum brightness**. Barcode scanners, optical turnstiles, and payment POS terminals read your screen instantly on the first try. Exiting the screen immediately restores your original system brightness.

---

## 🔒 Security Vault Architecture

```
┌────────────────────────────────────────────────────────┐
│                   Android / iOS OS                     │
├────────────────────────────────────────────────────────┤
│  App Layer: Biometrics (Fingerprint / Face ID / Keyguard) │
│  PIN Engine: 4 or 6 Digit Hash-Salted Entry            │
├────────────────────────────────────────────────────────┤
│  Kernel Sandbox Isolation: .twiin_vault (POSIX 0700)   │
│  Cryptographic Engine: 256-Bit Master AES / SHA-256    │
│  Zero Network Sockets: No HTTP, No Telemetry, No Cloud │
└────────────────────────────────────────────────────────┘
```

- **Zero Cloud Footprint**: Data never touches a remote server. Everything is stored strictly on-device.
- **Sandboxed Vault**: Files reside in an isolated `.twiin_vault` directory inaccessible to other installed applications.
- **Double-Layer Authentication**: Supports native device biometrics with graceful fallback to an in-app PIN.
- **Anti-Brute Force Protection**: Progressively increases lock durations upon repeated incorrect attempts.
- **Zero-Footprint Memory Scrubbing**: Sensitive encryption keys and card numbers are cleared from RAM when the app is backgrounded.

---

## 📦 APK Size Optimization & Compression

Twiin includes an aggressive optimization pipeline in `build.gradle.kts` and `proguard-rules.pro` that reduces release binary size from **100MB+ down to ~25MB**:

1. **Native Library ZIP Compression (`useLegacyPackaging = true`)**: Deflates `.so` binaries inside the APK.
2. **Resource Shrinking (`isShrinkResources = true`)**: Strips unused XML layouts and assets.
3. **R8 Full Mode (`android.enableR8.fullMode=true`)**: Aggressive cross-class inlining and dead code pruning.
4. **Localization Pruning (`resourceConfigurations += listOf("en")`)**: Removes strings for 80+ foreign languages bundled by third-party SDKs.
5. **Per-ABI Splitting**: Generates independent APKs for each CPU architecture.

### Ultra-Compressed Build Command:

```powershell
flutter build apk --release --split-per-abi --obfuscate --split-debug-info=build/app/outputs/symbols --tree-shake-icons
```

---

## 🚀 Getting Started

### Prerequisites
- [Flutter SDK](https://docs.flutter.dev/get-started/install) (v3.24.0 or higher)
- Android SDK (API Level 26 or higher)
- Java 17+

### Installation & Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/twiin.git
   cd twiin
   ```

2. **Install Flutter packages:**
   ```bash
   flutter pub get
   ```

3. **Run on a connected device:**
   ```bash
   flutter run
   ```

4. **Build a production release:**
   ```bash
   flutter build apk --release --split-per-abi
   ```

---

## 🤝 Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

Distributed under the **MIT License**. See [LICENSE](LICENSE) for more information.

<div align="center">
  <sub>Engineered with precision for absolute privacy and digital elegance.</sub>
</div>
