# ESP32 Visual Theta Entrainment System

### Research-Grade, Safety-Enhanced, Hardware-Timer-Accurate Light Stimulation Engine

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform: ESP32](https://img.shields.io/badge/Platform-ESP32-blue.svg)](https://www.espressif.com/en/products/socs/esp32)
[![Framework: Arduino](https://img.shields.io/badge/Framework-Arduino-green.svg)](https://www.arduino.cc/)

> **📌 Original Repository:** This project is based on and enhanced from the original work by **AdmDC**:  
> 🔗 **[https://github.com/admdc2000/esp32_theta_entrainment](https://github.com/admdc2000/esp32_theta_entrainment)**

This project is an **ESP32-based visual entrainment engine** designed for **experimental neuroscience research**, artistic installations, and investigation into **low-frequency visual rhythmic stimulation** (theta-range flicker patterns around 4–8 Hz).

⚠️ **CRITICAL WARNING: This project is NOT a medical device. It is not intended to treat, diagnose, or cure any medical condition. Use only for research, art, and experimentation under appropriate ethical guidelines.**

---

## 📋 Table of Contents

- [Features](#-features)
- [Scientific Background](#-scientific-background)
- [Hardware Requirements](#-hardware-requirements)
- [Installation](#-installation)
- [Building & Flashing](#-building--flashing)
- [Usage](#-usage)
- [Configuration](#-configuration)
- [Safety Warnings](#-safety-warnings)
- [Technical Details](#-technical-details)
- [Research Applications](#-research-applications)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)
- [Credits](#-credits)

---

## ✨ Features

### 🎯 Accurate Theta-Range Flicker Generation

- **Hardware timer-based timing** with <1μs jitter (ESP32 hardware timer)
- **Independent left and right** frequency channels (5.8 Hz and 6.2 Hz default)
- **Microsecond-accurate phase calculations** for precise entrainment
- **Sinusoidal modulation** (research-recommended, reduces harmonic distortion)
- **Phase synchronization enhancement** for improved entrainment effectiveness
- **Frequency range**: 4–8 Hz (configurable, optimal for theta entrainment)

### 🎨 3 Mandala Visualization Modes

- **Radial petals** — rotating petal patterns synchronized to frequency
- **Rotating spiral** — dynamic spiral patterns
- **Interference waves** — derived from L/R frequency interplay
- **Automatic mode switching** every 30 seconds (prevents visual adaptation)

### 🛡️ Safety-Enhanced Runtime

- **Panic-stop hardware button** — immediate LED blackout (GPIO 14)
- **Auto fade-in** — 3-minute gradual ramp-up (prevents abrupt bright-flash onset)
- **Hard session time-limit** — 30 minutes maximum with smooth 15-second fade-out
- **Reduced harmonic content** — optimized for safer low-frequency use
- **Brightness clamping** — maximum brightness limited to 70/255
- **Smoothstep transitions** — exponential curves for natural onset/offset

### ⚙️ Highly Customizable

- Frequency selection (4–8 Hz range)
- Visual mode selection
- Color palette customization
- Breathing envelope (0.12 Hz slow modulation)
- Optional micro-texture shimmer (disabled by default)
- Spiral ordering for physical LED layout
- Frame rate control (~100 FPS)

### 📊 Research-Grade Features

- **Hardware timer precision** — ESP32 hardware timer for ultra-low jitter
- **Serial monitoring** — diagnostic output at 115200 baud
- **Phase enhancement algorithms** — improved synchronization
- **Smooth sinusoidal modulation** — research-proven effectiveness
- **Optimized color schemes** — warm/cool separation for hemispheric studies

---

## 🧠 Scientific Background

### Theta Entrainment Research

Theta brainwaves (4–8 Hz) are associated with:
- Deep relaxation and meditation
- REM sleep
- Creative states
- Memory consolidation
- Hypnagogic states

**Visual flicker entrainment** (also called photic driving) is a phenomenon where external rhythmic light stimulation can influence brainwave frequencies through neural synchronization.

### Research-Based Improvements (2020–2024)

This implementation incorporates findings from recent neuroscience research:

1. **Sinusoidal modulation** is more effective than square waves for entrainment
2. **Hardware timer precision** reduces jitter and improves phase accuracy
3. **Gradual onset** (3+ minutes) reduces discomfort and improves effectiveness
4. **Frequency range 5.5–6.5 Hz** shows optimal theta entrainment results
5. **Reduced harmonics** improve signal purity and safety

### Important Research Notes

- This device is suitable for **experimental research only**
- **Not validated** for clinical or therapeutic use
- Requires **proper ethical approval** (IRB) for human studies
- **Eye isolation** (separated left/right channels) is required for hemispheric studies
- **Pre-screening** for photosensitivity is mandatory
- **Photodiode verification** recommended to confirm actual output frequencies

---

## 🔧 Hardware Requirements

### Minimum Requirements

- **ESP32 Dev Module** (or compatible ESP32 board)
- **20× WS2812B (NeoPixel) LEDs** (or compatible addressable RGB LEDs)
- **5V / 2A power supply** (minimum, 3A recommended for stability)
- **330–470 Ω resistor** on data line (between ESP32 and LED strip)
- **Momentary push button** for panic stop
- **Jumper wires** for connections
- **Breadboard** (optional, for prototyping)

### Recommended Additional Components

- **1000µF capacitor** across LED power rails (reduces power supply noise)
- **Separate power supply** for LED strip (prevents ESP32 brownouts)
- **Level shifter** (3.3V to 5V) if using long data lines
- **Proper LED mounting** — diffusers, goggles, or enclosure

### Pin Configuration

| Component | ESP32 Pin | Notes |
|-----------|-----------|-------|
| LED Data | GPIO 12 | Via 330Ω resistor |
| Panic Button | GPIO 14 | Connect to GND when pressed |

---

## 📦 Installation

### Prerequisites

1. **PlatformIO** (recommended) or **Arduino IDE**
2. **USB cable** for ESP32 programming
3. **Driver** for ESP32 USB-to-Serial chip (CP2102 or CH340)

### Step 1: Clone or Download Repository

```bash
git clone https://github.com/admdc2000/esp32_theta_entrainment.git
cd esp32_theta_entrainment
```

Or download as ZIP and extract.

### Step 2: Install PlatformIO (Recommended)

**Option A: PlatformIO Core (CLI)**
```bash
pip install platformio
```

**Option B: PlatformIO IDE**
- Install [PlatformIO IDE](https://platformio.org/install/ide?install=vscode) extension for VS Code

**Option C: Arduino IDE**
- Install [ESP32 Board Support](https://github.com/espressif/arduino-esp32)
- Install FastLED library via Library Manager

### Step 3: Install Dependencies

Dependencies are automatically managed by PlatformIO via `platformio.ini`:

```ini
lib_deps = 
    fastled/FastLED@^3.6.0
```

For Arduino IDE, install FastLED via:
```
Sketch → Include Library → Manage Libraries → Search "FastLED"
```

---

## 🔨 Building & Flashing

### Using PlatformIO

1. **Connect ESP32** via USB cable
2. **Identify COM port** (Windows: Device Manager, Linux/Mac: `ls /dev/tty*`)
3. **Build and upload**:
   ```bash
   platformio run --target upload
   ```
4. **Monitor serial output**:
   ```bash
   platformio device monitor
   ```

### Using Arduino IDE

1. **Select board**: Tools → Board → ESP32 Dev Module
2. **Select port**: Tools → Port → (your COM port)
3. **Upload**: Sketch → Upload
4. **Open Serial Monitor**: Tools → Serial Monitor (115200 baud)

### Expected Serial Output

After successful upload, you should see:
```
========================================
ESP32 Theta Entrainment System
Research-Grade Version
========================================
Hardware timer initialized
Panic button configured on pin 14
LED strip initialized: 20 LEDs on pin 12
Left frequency: 5.80 Hz
Right frequency: 6.20 Hz
Max session time: 1800 seconds (30.0 minutes)
Ramp-in time: 180 seconds (3.0 minutes)
System ready. Session started.
========================================
```

---

## 🚀 Usage

### Basic Operation

1. **Power on** the ESP32 (via USB or external power)
2. **Wait for initialization** (LEDs will be off initially)
3. **Session starts automatically** — LEDs will begin gradual fade-in over 3 minutes
4. **Visual modes switch** automatically every 30 seconds
5. **Session ends** after 30 minutes with automatic fade-out

### Panic Stop

- **Press and hold** the panic button (GPIO 14 → GND)
- **All LEDs immediately turn off**
- **System halts** until reset

### Serial Monitoring

Connect to serial monitor (115200 baud) to view:
- System initialization messages
- Frequency settings
- Session timing information
- Panic stop activation

### Safety Checklist

Before each use:
- [ ] Verify panic button is accessible
- [ ] Check LED brightness is appropriate
- [ ] Ensure proper eye isolation if using L/R separation
- [ ] Confirm user has no photosensitivity issues
- [ ] Set appropriate session duration
- [ ] Have emergency stop plan ready

---

## ⚙️ Configuration

### Frequency Settings

Edit `src/main.cpp` to modify frequencies:

```cpp
// Theta range: 4-8 Hz optimal
constexpr float LEFT_FREQ_HZ    = 5.8f;   // Left hemisphere
constexpr float RIGHT_FREQ_HZ   = 6.2f;   // Right hemisphere
```

### Modulation Type

```cpp
// Sinusoidal (recommended) or exponential pulse
constexpr bool USE_SINUSOIDAL_MODULATION = true;
```

### Safety Parameters

```cpp
constexpr float RAMP_IN_SECONDS = 180.0f;      // 3 minutes fade-in
constexpr float MAX_SESSION_SECONDS = 1800.0f; // 30 minutes max
constexpr uint8_t GLOBAL_BRIGHTNESS = 70;      // 0-255 (safety limit)
```

### Visual Modes

```cpp
constexpr float MODE_DURATION = 30.0f;  // Seconds between mode switches
```

### Advanced Parameters

```cpp
// Phase synchronization
constexpr bool USE_PHASE_ENHANCEMENT = true;
constexpr float PHASE_SYNC_STRENGTH = 0.15f;

// Breathing envelope
constexpr float BREATH_FREQ_HZ = 0.12f;

// Micro-texture (disabled by default)
constexpr bool MICRO_ENABLED = false;
```

### LED Layout

Modify `spiralOrder[]` array to match your physical LED arrangement:

```cpp
const uint8_t spiralOrder[NUM_LEDS] = {
    9,10,8,11,7,12,6,13,5,14,4,15,3,16,2,17,1,18,0,19
};
```

---

## ⚠️ Safety Warnings

### CRITICAL: Read Before Use

This project uses **low-frequency blinking lights** that may pose serious risks:

### ⛔ DO NOT USE IF:

- You have **photosensitive epilepsy** or history of seizures
- You are prone to **migraines**, **headaches**, or **dizziness**
- You are sensitive to **visual flicker** or **strobing lights**
- You have **neurological conditions** without medical supervision
- You are **pregnant** or have **cardiovascular issues** (consult doctor first)

### ✅ Safe Use Guidelines:

1. **Start with short sessions** (5–10 minutes)
2. **Use in well-lit room** (not complete darkness)
3. **Maintain safe distance** from LEDs (not directly in eyes)
4. **Have panic button accessible** at all times
5. **Never use while driving** or operating machinery
6. **Stop immediately** if experiencing discomfort, nausea, or visual disturbances
7. **Consult medical professional** before extended use

### Research Ethics

For human research studies:
- **IRB approval required** for institutional research
- **Informed consent** mandatory
- **Pre-screening** for photosensitivity
- **Medical supervision** recommended
- **Data privacy** compliance (GDPR, HIPAA, etc.)

### Legal Disclaimer

**BY USING THIS SOFTWARE AND HARDWARE, YOU ACKNOWLEDGE THAT:**
- This is **experimental research equipment**, not a medical device
- You are **solely responsible** for your use and any consequences
- The authors and contributors **assume no liability** for any harm
- This device is **not FDA approved** or certified for medical use
- **Use at your own risk**

---

## 🔬 Technical Details

### Hardware Timer Implementation

The system uses ESP32 hardware timer for ultra-precise timing:

```cpp
// Timer 0, 80MHz APB clock, divider 80 = 1MHz (1μs resolution)
timer = timerBegin(0, 80, true);
timerAttachInterrupt(timer, &onTimer, true);
timerAlarmWrite(timer, 100, true);  // 100μs ticks
```

This provides **<1μs jitter** compared to software timing methods.

### Phase Calculation

```cpp
float getTimeSeconds() {
    // Hardware timer-based, atomic access
    return (float)timerMicros * 0.000001f;
}

float phaseOf(float t, float f) {
    return fmodf(t * f, 1.0f);  // Phase in 0..1 range
}
```

### Sinusoidal Modulation

Research-recommended smooth modulation:

```cpp
float sinModSmooth(float t, float freq) {
    float phase = t * freq;
    float raw = sinf(TWO_PI * phase);
    return 0.5f * (1.0f + raw);  // 0..1 range
}
```

### Frame Rate Control

```cpp
constexpr unsigned long FRAME_MS = 10;  // ~100 FPS
// Independent of phase timing for smooth animation
```

### Memory Usage

Typical build statistics:
- **RAM**: ~6.8% (22,380 bytes / 327,680 bytes)
- **Flash**: ~23.2% (303,989 bytes / 1,310,720 bytes)

---

## 🧪 Research Applications

### Suitable For:

- **Neuroscience research** — visual entrainment studies
- **Cognitive science** — attention, memory, relaxation research
- **Art installations** — interactive light art
- **Meditation aids** — personal experimentation (with caution)
- **EEG studies** — photic driving research
- **Bilateral stimulation** — with proper eye isolation

### Not Suitable For:

- **Medical treatment** — not a therapeutic device
- **Clinical diagnosis** — not a diagnostic tool
- **Commercial medical products** — requires regulatory approval
- **Unsupervised use** by vulnerable populations

### Research Methodology

If conducting formal research:

1. **Calibrate output** using photodiode + oscilloscope
2. **Measure actual frequencies** at LED output
3. **Verify eye isolation** if using L/R separation
4. **Record environmental conditions** (lighting, ambient noise)
5. **Document participant screening** (photosensitivity, medical history)
6. **Use appropriate controls** (sham stimulation, baseline measurements)

---

## 🔍 Troubleshooting

### LEDs Not Lighting

- **Check power supply** — ensure 5V, 2A+ capacity
- **Verify data line** — GPIO 12 connected, resistor present
- **Check ground connection** — common ground required
- **Test with simple FastLED example** — verify hardware

### Flickering or Unstable

- **Add capacitor** — 1000µF across LED power rails
- **Separate power supplies** — use dedicated supply for LEDs
- **Check wiring** — ensure solid connections
- **Reduce brightness** — lower `GLOBAL_BRIGHTNESS` value

### Panic Button Not Working

- **Check wiring** — GPIO 14 to button, button to GND
- **Verify pull-up** — internal pull-up enabled in code
- **Test continuity** — button should short GPIO 14 to GND when pressed

### Serial Monitor Issues

- **Check baud rate** — must be 115200
- **Verify COM port** — correct port selected
- **Check drivers** — ESP32 USB-to-Serial drivers installed
- **Try different USB cable** — some cables are power-only

### Compilation Errors

- **Update PlatformIO** — `pio upgrade`
- **Update ESP32 platform** — `pio platform update espressif32`
- **Clear build cache** — `pio run --target clean`
- **Check library versions** — FastLED compatibility

### Frequency Accuracy

- **Verify hardware timer** — check serial output for initialization
- **Measure with oscilloscope** — confirm actual output frequencies
- **Check for interference** — WiFi/Bluetooth can affect timing
- **Disable WiFi** — add `WiFi.mode(WIFI_OFF);` in setup() if needed

---

## 🤝 Contributing

Contributions are welcome! Areas for improvement:

- **Enhanced mandala algorithms** — new visualization patterns
- **Advanced modulation engines** — alternative waveform types
- **Photodiode calibration** — automatic frequency verification
- **Safety enhancements** — additional safety protocols
- **ESP32-S3 support** — parallel LED drivers
- **Web interface** — WiFi-based configuration
- **Data logging** — session recording capabilities
- **Multi-frequency support** — dynamic frequency adjustment

### Contribution Guidelines

1. **Fork the repository**
2. **Create feature branch** — `git checkout -b feature/amazing-feature`
3. **Follow code style** — match existing formatting
4. **Add documentation** — update README if needed
5. **Test thoroughly** — verify safety features work
6. **Submit pull request** — with clear description

---

## 📄 License

This project is licensed under the **MIT License with Patent Grant** — see [LICENSE](LICENSE) file for details.

**Key Points:**
- ✅ **Free to use, modify, and distribute**
- ✅ **Commercial use permitted**
- ✅ **Patent grant** — contributors grant patent rights
- ❌ **Patent prohibition** — cannot patent this technology
- 📝 **Attribution required** — credit original author
- 🔗 **Link to original** — reference original repository

**Original Repository:** https://github.com/admdc2000/esp32_theta_entrainment

---

## 🙏 Credits

### Original Author

**AdmDC** — Original ESP32 Theta Entrainment implementation
- Repository: https://github.com/admdc2000/esp32_theta_entrainment

### Enhancements

This version includes research-grade improvements:
- Hardware timer implementation
- Enhanced safety protocols
- Optimized frequency ranges
- Improved modulation algorithms

### Acknowledgments

- **FastLED library** — excellent WS2812B support
- **ESP32 community** — hardware timer documentation
- **Neuroscience researchers** — entrainment research findings
- **Open source community** — continuous improvements

### References

- Visual flicker entrainment research (2020–2024)
- Theta brainwave studies
- Photodiode driving research
- ESP32 hardware timer documentation
- FastLED library documentation

---

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/admdc2000/esp32_theta_entrainment/issues)
- **Discussions**: [GitHub Discussions](https://github.com/admdc2000/esp32_theta_entrainment/discussions)
- **Original Repository**: https://github.com/admdc2000/esp32_theta_entrainment

---

## ⭐ Star History

If you find this project useful for your research or art, please consider starring the repository! ⭐

---

**Remember: This is experimental research equipment. Use responsibly and ethically.**

---

*Last updated: 2025*

