<div align="center">

# 🖐️ Hand Gesture Controlled Mouse

**Control your computer cursor entirely with hand gestures — no physical mouse needed.**  
Built with Python · MediaPipe · OpenCV · PyAutoGUI

[![Python](https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-0.10%2B-FF6F00?style=for-the-badge&logo=google&logoColor=white)](https://mediapipe.dev)
[![OpenCV](https://img.shields.io/badge/OpenCV-4.8%2B-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white)](https://opencv.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)

</div>

---

## 📌 Overview

**Hand Gesture Controlled Mouse** is a computer vision project that replaces your physical mouse with real-time hand gesture recognition. Using your webcam, the application tracks 21 hand landmarks via **MediaPipe Hands**, interprets finger positions into mouse commands, and executes them through **PyAutoGUI** — all at near-real-time speed.

This project demonstrates the intersection of **computer vision**, **human-computer interaction (HCI)**, and **gesture-based control systems**.

---

## ✨ Features

| Gesture | Action |
|---|---|
| ☝️ Index finger only | Move cursor |
| ✌️ Index + Middle (close together) | Left click |
| 🤟 Index + Middle + Ring | Right click |
| 🤏 Thumb + Index pinch | Double click |
| ✊ Fist (all fingers down) | Scroll up/down |

- 🎯 **Smooth cursor movement** — exponential moving average to eliminate jitter  
- ⏱️ **Click cooldown system** — prevents accidental multi-firing  
- 🖥️ **On-screen HUD** — live gesture legend and status text overlay  
- ⚙️ **Configurable** — tweak sensitivity, smoothing, thresholds in one place  

---

## 🛠️ Tech Stack

| Library | Purpose |
|---|---|
| [MediaPipe](https://developers.google.com/mediapipe) | Real-time hand landmark detection (21 points) |
| [OpenCV](https://opencv.org) | Webcam capture, frame processing, UI overlay |
| [PyAutoGUI](https://pyautogui.readthedocs.io) | Cross-platform mouse & keyboard control |
| [NumPy](https://numpy.org) | Coordinate smoothing & distance calculations |

---

## 📁 Project Structure

```
hand-gesture-mouse/
│
├── gesture_mouse.py      # Main application — run this
├── requirements.txt      # Python dependencies
├── .gitignore            # Files excluded from git
├── LICENSE               # MIT License
└── README.md             # You are here
```

---

## 🚀 Getting Started

### Prerequisites

- Python **3.8 or higher**
- A working **webcam**
- Windows / macOS / Linux

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/hand-gesture-mouse.git
cd hand-gesture-mouse
```

### 2. Create a Virtual Environment (Recommended)

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run the Application

```bash
python gesture_mouse.py
```

Press **`q`** to quit the application.

---

## 🎮 How to Use

1. Run the script — a webcam window will open.
2. Hold your hand in front of the camera (30–60 cm works best).
3. Use the gestures from the table above.
4. The **status bar** at the bottom of the window shows the detected action in real time.
5. The **legend** in the top-left corner is always visible for quick reference.

### Tips for Best Performance

- 💡 Ensure **good lighting** — avoid strong backlighting
- 🤚 Keep your hand **fully visible** in the frame
- 📏 Maintain a distance of roughly **30–60 cm** from the camera
- 🎨 A **plain background** improves detection accuracy

---

## ⚙️ Configuration

Open `gesture_mouse.py` and edit the constants near the top:

```python
SMOOTHING          = 5    # Higher = smoother but more lag (range: 3–10)
CLICK_THRESHOLD    = 40   # Lower = need fingers closer to click (pixels)
SCROLL_SENSITIVITY = 30   # Lower = scroll triggers more easily (pixels)
CAM_INDEX          = 0    # Change to 1, 2... if using an external webcam
```

---

## 🔍 How It Works

```
Webcam Frame
     │
     ▼
Flip & Convert to RGB
     │
     ▼
MediaPipe Hands
  → Detect 21 landmarks per hand
     │
     ▼
fingers_up() — classify which fingers are extended
     │
     ▼
Gesture Logic
  ├─ Index only       → pyautogui.moveTo()
  ├─ Index+Middle     → pyautogui.click()
  ├─ Index+Middle+Ring→ pyautogui.rightClick()
  ├─ Thumb+Index pinch→ pyautogui.doubleClick()
  └─ Fist             → pyautogui.scroll()
     │
     ▼
smooth_position() — moving average to reduce jitter
     │
     ▼
Draw HUD overlay & display frame
```

MediaPipe returns normalized landmark coordinates `(x, y)` in `[0.0, 1.0]`. These are mapped to screen pixels using `pyautogui.size()`.

---

## 🧩 Landmark Reference

MediaPipe tracks **21 hand landmarks**:

```
                 8   12  16  20
                 |   |   |   |
             7   |   |   |   |
         6   |   11  |   |   19
     5   |   10  |   15  |   |
 4   |   9   |   14  |   18  |
 |   |   |   13  |   17  |  
 3   6   |   |   |   |   |  
 |   |   |   |   |   |   |  
 2   5   |   |   |   |   |  
 |   |   |   |   |   |   |  
 1   |   |   |   |   |   |  
  \  4   |   |   |   |   |  
   \ |   3   2   3   2   3  
    \|   |   |   |   |   |  
     0───────────────────── (WRIST)
```

Key landmarks used:
- `4` — Thumb tip
- `8` — Index tip
- `12` — Middle tip  
- `16` — Ring tip
- `20` — Pinky tip
- `6, 10, 14, 18` — PIP joints (used to check if finger is up)

---

## 🐛 Known Limitations

- Performance may dip in **low-light** conditions
- **Multi-monitor** setups may need coordinate mapping adjustments
- Accuracy reduces if the hand is **at extreme angles** (>60° rotation)
- Not tested with **Linux Wayland** (X11 recommended on Linux)

---

## 🔮 Future Improvements

- [ ] Add drag-and-drop gesture support
- [ ] Support two-hand detection (one hand for actions, one for control)
- [ ] GUI settings panel for real-time threshold tuning
- [ ] Gesture recording / custom gesture binding
- [ ] System tray icon with toggle on/off
- [ ] Performance profiling & GPU acceleration via MediaPipe GPU delegate

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create your feature branch: `git checkout -b feature/my-feature`
3. Commit your changes: `git commit -m 'Add my feature'`
4. Push to the branch: `git push origin feature/my-feature`
5. Open a Pull Request

Please make sure your code follows clean Python conventions and is well-commented.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgements

- [Google MediaPipe Team](https://developers.google.com/mediapipe) — for the incredible hand tracking pipeline
- [OpenCV](https://opencv.org) — for robust computer vision tooling
- [PyAutoGUI](https://pyautogui.readthedocs.io) — for seamless GUI automation

---

<div align="center">

Made with ❤️ using Python, MediaPipe & OpenCV

⭐ **Star this repo if you found it useful!** ⭐

</div>
