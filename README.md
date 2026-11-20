# Moveocado: Telepathically Play Tetris

![OpenCV](https://img.shields.io/badge/OpenCV-27338e?style=for-the-badge&logo=OpenCV&logoColor=white)
![Python](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=TensorFlow&logoColor=white)
![Keras](https://img.shields.io/badge/Keras-FF0000?style=for-the-badge&logo=keras&logoColor=white)

Ever wished you could play games without your clunky keyboard? Moveocado lets you play Tetris with nothing but your hands. Point, gesture, and watch the blocks fall.

It uses your webcam, [MediaPipe](https://developers.google.com/mediapipe) hand tracking, and a gesture-recognition model to turn hand movements into arrow-key presses. Any Tetris game that uses the arrow keys will work.

## Controls

| Action | How |
|---|---|
| ⬅️ Move left | Move your index fingertip to the **left edge** of the camera view |
| ➡️ Move right | Move your index fingertip to the **right edge** of the camera view |
| 🔄 Rotate | 👍 Thumbs up |
| ⬇️ Fast drop | 👎 Thumbs down |
| ❌ Quit | Press `q` in the Hand Tracking window |

## Requirements

- A webcam
- [uv](https://docs.astral.sh/uv/getting-started/installation/), which installs Python 3.9.13 for you

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Miller-Liu/Moveocado.git
cd Moveocado
```

### 2. Set up the environment

```bash
uv python pin 3.9.13
uv venv
uv pip install -r requirements.txt
```

uv downloads Python 3.9.13 automatically if you don't already have it.

### 3. Run it

```bash
uv run main.py
```

Then open a Tetris game that uses the arrow keys (for example [tetris.com](https://tetris.com/play-tetris)) and **click on the game window** so it has keyboard focus.

## Troubleshooting

**Nothing happens in the game**
- Make sure the game window has focus, not the Hand Tracking window. Moveocado sends ordinary key presses to whichever window is active.
- **macOS:** give your terminal app (Terminal, iTerm, VS Code, etc.) permission under *System Settings → Privacy & Security → Accessibility*. You may also need to grant *Input Monitoring*. Restart the terminal afterwards.

**Left/right doesn't trigger**
- The trigger zones are the outer 7.5% of the frame on each side. Your whole hand needs to stay visible for tracking to work, so try sitting farther from the camera.
- To make the zones bigger, edit the `0.075` and `0.925` thresholds in `main.py` (for example, `0.3` and `0.7`).

**Gestures aren't recognized**
- Use good, even lighting and a plain background.
- Keep one hand in view. Moveocado only tracks a single hand.

**Wrong camera opens**
- Change `cv2.VideoCapture(0)` in `main.py` to `1` or `2`.

## How It Works

1. OpenCV captures and mirrors each webcam frame.
2. MediaPipe detects 21 hand landmarks.
3. The index fingertip's horizontal position triggers left and right moves.
4. A Keras gesture model classifies the hand pose, and thumbs up or down triggers rotate or drop.
5. PyAutoGUI sends the matching arrow key to the active window.