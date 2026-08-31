# G.T.B - Gonna Take the Boredom

> **Transform conversations into complete projects.** An intelligent AI agent that eliminates the boredom of repetitive coding tasks.

[![Watch Demo](https://img.shields.io/badge/🎬-Watch_Demo-red?style=for-the-badge)](https://youtu.be/s2BT-Lfh25k)

![Version](https://img.shields.io/badge/version-2.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Google%20Colab-orange)
![AI](https://img.shields.io/badge/AI-Gemini%202.5%20Flash-purple)
![Cloud](https://img.shields.io/badge/Cloud-Google%20Cloud-blue)
![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Flask](https://img.shields.io/badge/Flask-3.0-green)

---

## 🎯 Why G.T.B?

**The Problem:** Developers waste hours on repetitive tasks — setting up file structures, writing boilerplate code, creating placeholders, and formatting documentation.

**The Solution:** G.T.B transforms natural conversations into complete, working projects automatically. Describe what you want, discuss the details, and let the agent handle the rest.

**The Value:** Free your creativity. Remove boredom from programming. Focus on ideas, not implementation.

---

## ✨ Features

### 🎭 Three Operating Modes

| Mode | Description | Example |
|------|-------------|---------|
| 💬 **Chat** | Natural conversation to discuss and refine ideas | *"I want a portfolio site"* |
| 🏗️ **Build** | Generate complete projects from discussions | *"Build what we discussed"* |
| 🖼️ **Images** | Generate images with AI-enhanced prompts | *"3 cute cat logos"* |

### 🧠 Self-Learning Memory
- Stores **user preferences** in `agent_memory.txt`
- **Editable** from the Settings panel
- Learns: preferred language, project types, coding style
- Adapts responses based on past interactions

### 📝 6 Ready Templates
- 🎨 Portfolio Website
- 🐍 Python Snake Game
- 📝 Blog
- 📊 Data Analysis
- 🛒 E-commerce Store
- 🔌 Flask API

### 🎨 Image Generation
- **Providers:** Replicate, Hugging Face, Custom API
- **LLM-Enhanced Prompts:** Converts vague requests into professional prompts
- **Intelligent Naming:** LLM suggests logical filenames
- **Project Integration:** Images embedded into generated projects

### 💻 Code Editor
- View and edit generated files
- Save modifications
- HTML live preview
- Dark/Light theme toggle

### 🔌 Provider Support

**LLM Providers:**
| Provider | Model | Status |
|----------|-------|--------|
| Google Colab AI | Gemini 2.5 Flash | ✅ Free, Default |
| ClaudeStore (LLMsRelay) | Claude Sonnet 4.6 | ✅ Works |
| Custom | Any OpenAI-compatible | ✅ Works |

**Image Providers:**
| Provider | Model | Status |
|----------|-------|--------|
| Replicate | Ideogram v3 Turbo | ✅ Works |
| Hugging Face | SDXL Base 1.0 | ✅ Works |
| Custom | Any API | ✅ Works |

---

## ☁️ Google Cloud Integration

G.T.B is **fully built on Google Cloud infrastructure**:

- **Google Colab** — Compute environment (Google Cloud)
- **Google Gemini 2.5 Flash** — AI model (Google Cloud AI)
- **Google Drive** — Cloud storage for projects
- **Google Cloud Run** — Deployment target (optional)

---

## 🚀 Quick Start

### Prerequisites
- Google Colab account
- Files uploaded to Google Drive at `/content/drive/MyDrive/project-forge-agent/`

### Run (Single Cell)

```python
import os, subprocess, time, threading
subprocess.run(["pkill", "-f", "flask"], capture_output=True)
subprocess.run(["fuser", "-k", "5000/tcp"], capture_output=True)
time.sleep(2)
os.chdir("/content/drive/MyDrive/project-forge-agent")
!pip install -q flask fpdf requests huggingface_hub
def run_flask():
    from app import app
    app.run(host='0.0.0.0', port=5000, debug=False, use_reloader=False)
threading.Thread(target=run_flask, daemon=True).start()
time.sleep(3)
from google.colab import output
output.serve_kernel_port_as_iframe(5000, height=700)
while True:
    time.sleep(1)
```

---

## ☁️ Deploy to Google Cloud Run

### Step 1: Create Dockerfile

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PORT=8080
ENV PYTHONUNBUFFERED=1

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "app:app"]
```

### Step 2: Deploy

```bash
# Set project
gcloud config set project YOUR_PROJECT_ID

# Deploy
gcloud run deploy gtb-agent \
  --source . \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 1Gi \
  --timeout 300
```

### Step 3: Get URL

```bash
gcloud run services describe gtb-agent --format='value(status.url)'
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Flask Server (5000)                  │
│  ┌───────────────────────────────────────────────────┐  │
│  │              Agent Brain                          │  │
│  │  • Intent Analysis                                │  │
│  │  • Discovery Mode                                 │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │              Agent Core                           │  │
│  │  • Chat Mode                                      │  │
│  │  • Build Mode                                     │  │
│  │  • Image Mode                                     │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │              LLM Handler                          │  │
│  │  • Colab AI (Gemini 2.5 Flash)                   │  │
│  │  • ClaudeStore                                    │  │
│  │  • Custom                                         │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │              Image Handler                        │  │
│  │  • Replicate                                      │  │
│  │  • Hugging Face                                   │  │
│  │  • Custom                                         │  │
│  └───────────────────────────────────────────────────┘  │
│  ┌───────────────────────────────────────────────────┐  │
│  │              File Extractor                       │  │
│  │  • Parse LLM output                               │  │
│  │  • Create files                                   │  │
│  └───────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

---

## 📁 Project Structure

| File | Description |
|------|-------------|
| `app.py` | Flask server with API endpoints |
| `agent_brain.py` | Intent analysis and planning |
| `agent_core.py` | Execution engine (3 modes) |
| `llm_handler.py` | LLM provider management |
| `image_handler.py` | Image generation (3 providers) |
| `pdf_generator.py` | Documentation generator |
| `file_extractor.py` | Parse and create files |
| `config_manager.py` | Settings management |
| `index.html` | Web interface |
| `agent_memory.txt` | Self-learning memory |
| `config.json` | Configuration |

---

## 📡 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Web interface |
| POST | `/api/chat` | Chat mode |
| POST | `/api/build` | Build project |
| POST | `/api/generate_image` | Generate single image |
| POST | `/api/generate_images_batch` | Generate multiple images |
| GET | `/api/download/<id>` | Download project ZIP |
| GET | `/api/image/<id>/<file>` | Serve image |
| GET | `/api/file/<id>/<file>` | View file |
| POST | `/api/save_file` | Save edited file |
| GET | `/api/memory` | Get agent memory |
| POST | `/api/memory` | Save agent memory |

---

## 🎯 Usage Examples

### Example 1: Build a Game
```
User: "Create a Python snake game"
G.T.B: Generates main.py, game.py, player.py, constants.py...
```

### Example 2: Discuss then Build
```
User: "I want a personal website"
G.T.B: "Great! A portfolio or blog?"
User: "Portfolio with contact section"
G.T.B: "I'll use a clean design with blue accents. Ready to build?"
User: "Build it"
```

### Example 3: Generate Images
```
User: "3 logo designs for a tech startup"
G.T.B: Enhances prompts → Generates via Replicate/HF → Names files → Zips
```

---

## 🔧 Troubleshooting

### Images not generating?
- Check API Key in Settings ⚙️
- Verify provider selected
- Hugging Face: use `stabilityai/stable-diffusion-xl-base-1.0`
- Replicate free: 6 requests/minute

### LLM not responding?
- Colab AI works without keys (default)
- ClaudeStore: verify API Key format
- Custom: check Base URL ends with `/v1`

### Server not starting?
```bash
pkill -f flask
fuser -k 5000/tcp
pip install flask fpdf requests huggingface_hub
```

### Cloud Run deployment failing?
- Verify `gcloud` authenticated
- Check billing enabled
- Port must be `8080`

---

## 📊 Statistics

| Metric | Value |
|--------|-------|
| Operating Modes | 3 |
| LLM Providers | 3 |
| Image Providers | 3 |
| Templates | 6 |
| API Endpoints | 11 |
| Features | 15+ |

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgments

- **Google** — Colab, Gemini 2.5 Flash, Cloud Platform
- **Replicate** — Image generation
- **Hugging Face** — Image generation
- **LLMsRelay** — Claude API
- **Flask** — Web framework

---

> **"Gonna Take the Boredom"** — Transform ideas into reality ⚡

**Built with ❤️ on Google Cloud Platform**
```

---
