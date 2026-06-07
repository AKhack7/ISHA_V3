## https://akhack7.github.io/ISHA_V3/



# 🤖 ISHA — Intelligent System for Human Assistance

<p align="center">
  <img src="https://img.shields.io/badge/version-10.0-cyan?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Python-3.9%2B-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/PyQt5-GUI-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Ollama-Local%20AI-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Edge--TTS-Voice-purple?style=for-the-badge" />
  <img src="https://img.shields.io/badge/license-MIT-lightgrey?style=for-the-badge" />
</p>

> **ISHA** is a single-file, fully offline-capable AI desktop assistant with a **Cyberpunk UI**, triple local model routing via Ollama, real-time voice synthesis via Edge-TTS, hand gesture control, weather, news, system info, and a full API mode panel — all running locally on your machine.

---

## 📸 Screenshots

> The UI features a frameless, translucent Cyberpunk-themed window with neon cyan/pink accents, animated particles, a communication dock, and a splash screen.

---

## ✨ Features

### 🧠 AI Engine — Triple Local Model Routing
| Model | Role |
|-------|------|
| `phi3:mini` | Code generation, debugging, analysis |
| `gemma2` | Deep knowledge, explanations, essays |
| `llama3.2` | General chat fallback |

- Smart keyword-based routing automatically selects the best model for each query
- Full chat history context (last 16 messages sent to model)
- Cloud API override via the API Mode panel (supports 12 vendors)
- Per-model ON/OFF toggle, persisted across sessions in `isha_model_toggles.json`

### 🗣️ Edge-TTS Voice Engine
- Powered by **Microsoft Edge TTS** (`en-IN-NeerjaNeural` — premium Indian-English female voice)
- Runs entirely in a background `QThread` with an `asyncio` event loop — **never blocks the GUI**
- Thread-safe queue with stale-utterance drain (interrupts old speech on new input)
- Silent audio playback chain: `pygame.mixer` → `playsound` → `winmm` (ctypes) → `afplay` (macOS) → `mpg123/ffplay` (Linux)
- Text sanitisation strips Markdown, HTML tags, emojis, and bullet symbols before speaking

### 🎨 Cyberpunk UI (PyQt5)
- Frameless, translucent, always-on-screen floating window
- Animated neon particle system background
- Colour palette: Neon Cyan (`#00F0FF`), Hot Pink (`#FF007F`), Deep Space BG (`#070A14`)
- Draggable window (no title bar)
- `QGraphicsDropShadowEffect` glow on all major widgets
- Splash screen with animated progress bar on startup

### 📡 Communication Dock
- Real-time log streaming
- Signal-slot based, freeze-free async design
- Toggle from main window or system tray

### ⚙️ Quick Settings Panel
| Setting | Function |
|---------|----------|
| Mute Toggle | Enable/disable TTS voice output |
| Hand Control | Toggle MediaPipe gesture recognition |
| Display Toggle | Show/hide main window |
| API Mode | Open full CRUD API manager dialog |

### 🔑 API Mode Panel
- CyberpunkDialog popup with full Create / Read / Update / Delete
- Max **3 API slots**, max **2 active simultaneously**
- Per-slot ON/OFF toggle with Edit button
- Creation timestamps, 12-vendor dropdown (OpenAI, Anthropic, Gemini, Mistral, etc.)

### ✋ Hand Gesture Control
- Powered by **MediaPipe** + **OpenCV**
- Control the assistant with hand gestures (no keyboard needed)
- Runs in a separate daemon `QThread`

### ⌨️ Global Hotkey
- `Ctrl + Alt + Space` — focus ISHA from anywhere on the desktop
- Managed by a dedicated `QThread` using the `keyboard` library

### 🌐 Built-in Commands
| Command | What it does |
|---------|-------------|
| Weather | Fetches current weather via API |
| News | Retrieves latest headlines |
| System Info | CPU, RAM, disk, OS via `psutil` |
| Network Info | IP, speed test via `speedtest-cli` |
| Screenshot | Captures screen via `pyautogui` |
| Web Search | Opens browser search |
| Math | Evaluates expressions via `sympy` |
| WhatsApp | Sends messages via `pywhatkit` |

### 📜 Chat History Manager
- Full conversation history with live scroll
- Exportable session logs
- Opens as a separate resizable window

### 🖥️ System Tray
- Minimises to system tray (app stays alive, `setQuitOnLastWindowClosed(False)`)
- Tray menu: Show, Search, History, Comm Dock, Screenshot, Exit
- Double-click tray icon to restore window

### 🔧 Self-Install Engine
- Runs **before** `QApplication` initialises
- Auto-installs all missing pip packages silently
- Auto-pulls missing Ollama models (`phi3:mini`, `gemma2`, `llama3.2`)
- Zero manual setup required on first run

---

## 🚀 Getting Started

### Prerequisites
- Python **3.9+**
- [Ollama](https://ollama.com/) installed and running locally
- Internet connection on first run (for package install and model pull)

### Installation & Run

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/isha-assistant.git
cd isha-assistant

# Run directly — ISHA self-installs all dependencies automatically
python V3_localmodel_Isha.py
```

> ⚠️ On first launch, ISHA will automatically install all pip packages and pull the three Ollama models. This may take several minutes depending on your internet speed and hardware.

### Manual Dependency Install (optional)

```bash
pip install PyQt5 edge-tts SpeechRecognition pyautogui requests \
            beautifulsoup4 sympy psutil ollama mediapipe \
            opencv-python keyboard speedtest-cli pywhatkit \
            pygame "playsound==1.2.2"
```

### Pull Ollama Models Manually

```bash
ollama pull phi3:mini
ollama pull gemma2
ollama pull llama3.2
```

---

## 🗂️ Project Structure

```
isha-assistant/
│
├── V3_localmodel_Isha.py       # Single-file application (all ~4800 lines)
├── isha_model_toggles.json     # Auto-generated: per-model ON/OFF states
├── isha_api_configs.json       # Auto-generated: API Mode slot configurations
└── README.md                   # This file
```

### Internal Code Sections (§)

| Section | Description |
|---------|-------------|
| §0 | Self-Install Engine |
| §1 | PyQt5 Imports & Colour Palette |
| §2 | Edge-TTS Engine (QThread + asyncio) |
| §3 | LocalAI — Triple-Model Ollama Pipeline |
| §4–§10 | Command Handlers (weather, news, sysinfo, math, etc.) |
| §11–§15 | UI Widgets (chat bubbles, input bar, particles) |
| §16–§20 | Quick Settings, API Mode Panel, Communication Dock |
| §21–§23 | Hand Gesture Engine, Global Hotkey, Model Settings |
| §24–§25 | Main Window (`ISHAWindow`) |
| §26 | Splash Screen |
| §27 | System Tray |
| §28 | Entry Point (`main()`) |

---

## 🧩 Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                   ISHAWindow (Main Qt Thread)        │
│  ┌─────────────┐  ┌────────────┐  ┌──────────────┐  │
│  │  Chat UI    │  │  Settings  │  │  Comm Dock   │  │
│  └─────────────┘  └────────────┘  └──────────────┘  │
└────────────────────────┬────────────────────────────┘
                         │ pyqtSignal (thread-safe)
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌──────▼───────┐  ┌────▼──────────┐
│ EdgeTTSWorker│  │  LocalAI     │  │ GestureEngine │
│  (QThread)   │  │  (Ollama)    │  │  (QThread)    │
│  asyncio loop│  │  phi3:mini   │  │  MediaPipe    │
│  edge-tts    │  │  gemma2      │  │  OpenCV       │
│  pygame      │  │  llama3.2    │  └───────────────┘
└──────────────┘  └──────────────┘
                         │
                  ┌──────▼──────┐
                  │  Cloud API  │
                  │  (optional) │
                  │  12 vendors │
                  └─────────────┘
```

---

## ⚙️ Configuration

### API Mode (Cloud Providers)
1. Open ISHA → Quick Settings → **API Mode**
2. Click **Add** to create a new API slot
3. Select your vendor (OpenAI, Anthropic, Gemini, Cohere, Mistral, etc.)
4. Paste your API key and toggle it ON
5. ISHA will route queries through the cloud API when active

### Model Toggles
- Access via the **Model Settings** button in the main window
- Each model (`phi3:mini`, `gemma2`, `llama3.2`) can be individually enabled/disabled
- State is saved to `isha_model_toggles.json`

### TTS Voice
Change the voice by editing the `VOICE` constant in `EdgeTTSWorker`:
```python
VOICE = "en-IN-NeerjaNeural"   # Default: Indian English female
# Other options: "en-US-JennyNeural", "en-GB-SoniaNeural", etc.
```

---

## 🖐️ Hand Gesture Controls

ISHA supports gesture-based control via your webcam using MediaPipe:

| Gesture | Action |
|---------|--------|
| ✋ Open Palm | Pause / Stop TTS |
| 👊 Fist | (configurable) |
| 👆 Point Up | Scroll up |
| 👇 Point Down | Scroll down |

Enable via Quick Settings → **Hand Control** toggle.

---

## ⌨️ Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl + Alt + Space` | Global hotkey — focus ISHA from anywhere |
| `Enter` | Send message |
| `Esc` | Minimise to tray |

---

## 🛠️ Requirements

```
Python          >= 3.9
PyQt5           >= 5.15
edge-tts        >= 6.1
SpeechRecognition
pyautogui
requests
beautifulsoup4
sympy
psutil
ollama          >= 0.3
mediapipe
opencv-python
keyboard
speedtest-cli
pywhatkit
pygame
playsound       == 1.2.2
```

---

## 🔄 Changelog

### v10.0 (Current)
- **Edge-TTS Migration**: Removed `pyttsx3`. Full async TTS via `QThread` + thread-safe queue. GUI never blocks.
- **Triple Local Model Routing**: `phi3:mini` (code), `gemma2` (knowledge), `llama3.2` (chat fallback)
- **Self-Install Engine**: Pre-GUI auto-installer for all pip packages + Ollama model puller
- **API Mode Panel**: Full CRUD, max 3 slots, max 2 active, ON/OFF toggle, 12 vendor dropdown, timestamps
- **Communication Dock Fix**: Fixed signal slots, real-time log streaming, no freeze
- **Quick Settings Cleanup**: Removed Wi-Fi, Bluetooth, Airplane, Dark Mode, Focus, Night Light, Network Type, Battery cards; kept Mute, Hand Control, Display Toggle

### v9.0
- Previous major release baseline

---

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/your-feature`
3. Commit your changes: `git commit -m 'Add your feature'`
4. Push to the branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 👤 Author

**ISHA** — Intelligent System for Human Assistance  
Built with ❤️ using Python, PyQt5, Ollama, and Edge-TTS.

---

<p align="center">
  <i>"Your local AI companion — no cloud required."</i>
</p>
