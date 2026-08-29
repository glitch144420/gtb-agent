# G.T.B - Gonna Take the Boredom

> An intelligent AI agent that transforms conversations into complete projects, running entirely in Google Colab.

![Version](https://img.shields.io/badge/version-2.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Platform](https://img.shields.io/badge/platform-Google%20Colab-orange)
![AI](https://img.shields.io/badge/AI-Gemini%202.5%20Flash-purple)
![Cloud](https://img.shields.io/badge/Cloud-Google%20Cloud-blue)

---

## 📖 Overview

**G.T.B (Gonna Take the Boredom)** is a multi-agent AI system that turns natural language conversations into complete software projects. Built with a modular architecture, it combines LLM-powered code generation, intelligent image creation, and a sophisticated web interface — all running free in Google Colab.

### Why G.T.B?
- **Conversational** — Discuss ideas naturally before building
- **Multi-Modal** — Generate code AND images
- **Self-Learning** — Remembers user preferences across sessions
- **Provider-Agnostic** — Works with multiple LLM and image providers
- **Zero Setup** — Runs instantly in Colab with free Gemini

### ☁️ Google Cloud Integration

G.T.B is built entirely on **Google Cloud infrastructure**:
- **Google Colab** — Powered by Google Cloud Compute
- **Google Gemini 2.5 Flash** — Google Cloud AI Model
- **Flask Server** — Deployable to Google Cloud Run
- **Google Drive** — Cloud storage for projects

---

## ✨ Features

### 🎭 Three Operating Modes

| Mode | Description | Use Case |
|------|-------------|----------|
| 💬 **Chat** | Free-flowing conversation | Discuss ideas, ask questions |
| 🏗️ **Build** | Generate complete projects | Create apps, games, websites |
| 🖼️ **Images** | Generate images only | Batch image creation |

### 🧠 Intelligent Agent

- **Intent Recognition** — Understands user requests naturally
- **Discovery Mode** — Asks clarifying questions when needed
- **Self-Learning Memory** — Stores preferences in `agent_memory.txt`
- **Project Type Detection** — Python, Web, React, Node.js, Data Science
- **Smart Image Prompts** — LLM crafts professional prompts from context

### 🎨 Image Generation

- **LLM-Enhanced Prompts** — Converts vague requests into professional prompts
- **Intelligent Naming** — LLM suggests logical filenames
- **Project Integration** — Images embedded into generated projects
- **Batch Generation** — Generate multiple images with rate-limit handling

### 🖥️ Full-Featured Interface

- **Code Editor** — View and edit generated files
- **HTML Preview** — Live preview of HTML files
- **Dark/Light Mode** — Theme toggle
- **History Sidebar** — Chats, projects, and files
- **6 Templates** — Portfolio, Snake Game, Blog, Data Analysis, E-commerce, Flask API
- **Export** — Download chat history as Markdown

### 🔌 Provider Support

**LLM Providers:**
- Google Colab AI (Gemini 2.5 Flash) — *Free, default*
- ClaudeStore (LLMsRelay) — Claude Sonnet 4.6
- Custom — Any OpenAI-compatible API

**Image Providers:**
- Replicate — Ideogram v3 Turbo / SDXL
- Custom — Any compatible API

---

## 🚀 Quick Start

### Prerequisites
- Google Colab account
- Files uploaded to Google Drive

### Installation

1. **Upload files** to `/content/drive/MyDrive/gtb-agent/`

2. **Run this single cell:**
```python
import os, subprocess, time, threading
subprocess.run(["pkill", "-f", "flask"], capture_output=True)
subprocess.run(["fuser", "-k", "5000/tcp"], capture_output=True)
time.sleep(2)
os.chdir("/content/drive/MyDrive/gtb-agent")
!pip install -q flask fpdf requests
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

3. **Open the interface** via the iframe popup

---

## ☁️ Deploy to Google Cloud Run

### Prerequisites
- Google Cloud account with billing enabled
- `gcloud` CLI installed
- Docker installed (optional for local testing)

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

### Step 2: Update requirements.txt

```
flask==3.0.0
gunicorn==21.2.0
fpdf==1.7.2
requests==2.31.0
python-dotenv==1.0.0
```

### Step 3: Deploy to Cloud Run

```bash
# Set project
gcloud config set project YOUR_PROJECT_ID

# Build and deploy
gcloud run deploy gtb-agent \
  --source . \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --memory 1Gi \
  --timeout 300
```

### Step 4: Access the app

```bash
gcloud run services describe gtb-agent --format='value(status.url)'
```

### Alternative: Deploy via Console

1. Go to [Cloud Run Console](https://console.cloud.google.com/run)
2. Click **Create Service**
3. Select **Deploy from source**
4. Upload the project files
5. Set port to `8080`
6. Click **Deploy**

---

## 📁 Project Structure

```
project-forge-agent/
├── app.py              # Flask server
├── agent_brain.py      # Intent analysis & planning
├── agent_core.py       # Execution engine
├── llm_handler.py      # LLM provider management
├── image_handler.py    # Image generation
├── pdf_generator.py    # Documentation generator
├── file_extractor.py   # File parsing
├── config_manager.py   # Settings management
├── index.html          # Web interface
├── config.json         # Configuration
├── agent_memory.txt    # Self-learning memory
├── Dockerfile          # For Cloud Run deployment
├── requirements.txt    # Dependencies
└── temp/               # Generated projects
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────┐
│              Flask Server (5000)            │
│  ┌───────────────────────────────────────┐  │
│  │         Agent Brain                   │  │
│  │  • Intent Analysis                    │  │
│  │  • Discovery Mode                     │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         Agent Core                    │  │
│  │  • Chat Mode                          │  │
│  │  • Build Mode                         │  │
│  │  • Image Mode                         │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         LLM Handler                   │  │
│  │  • Colab AI (Gemini)                  │  │
│  │  • ClaudeStore                        │  │
│  │  • Custom                             │  │
│  └───────────────────────────────────────┘  │
│  ┌───────────────────────────────────────┐  │
│  │         Image Handler                 │  │
│  │  • Replicate                          │  │
│  │  • Custom                             │  │
│  └───────────────────────────────────────┘  │
└─────────────────────────────────────────────┘
```

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
G.T.B: "What sections do you need?"
User: "Portfolio and contact"
G.T.B: "What colors do you prefer?"
User: "Blue and white"
G.T.B: "Ready to build?"
User: "Build it"
```

### Example 3: Generate Images
```
User: "3 cute cat images"
G.T.B: Enhances prompts → Generates → Names → Zips
```

---

## 🎬 Demo Video

[![G.T.B Demo](https://img.shields.io/badge/Watch-Demo-red)](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

**Watch G.T.B in action:** [Demo Video Link](https://www.youtube.com/watch?v=YOUR_VIDEO_ID)

---

## ⚙️ Configuration

### Settings Panel (⚙️)

**LLM Provider:**
- Provider selection
- Base URL (optional)
- Model name (optional)
- API Key (optional)

**Image Provider:**
- Provider selection
- Base URL (optional)
- Model name (optional)
- API Key (optional)

### config.json

```json
{
  "llm_provider": "colab_ai",
  "llm_base_url": "",
  "llm_api_key": "",
  "llm_model_name": "",
  "image_provider": "none",
  "image_base_url": "",
  "image_model_name": "",
  "image_api_key": ""
}
```

---

## 🧠 Self-Learning Memory

G.T.B learns from every interaction:

```
Conversation 1: "I prefer Python" → Stored
Conversation 2: "I like games" → Stored  
Conversation 3: "I use Arabic comments" → Stored

Next session: G.T.B remembers and adapts!
```

Memory file: `agent_memory.txt`

---

## 📊 Statistics

| Metric | Value |
|--------|-------|
| Operating Modes | 3 |
| LLM Providers | 3 |
| Image Providers | 2 |
| Templates | 6 |
| API Endpoints | 9 |
| Features | 15+ |

---

## 🔧 Troubleshooting

### Images not generating?
- Check API Key in Settings ⚙️
- Verify provider selected
- Rate limits: Replicate free = 6/min

### LLM not responding?
- Check provider selection
- Verify API Key format
- Colab AI works without keys

### Server not starting?
- Kill previous: `pkill -f flask`
- Free port: `fuser -k 5000/tcp`
- Reinstall: `pip install flask fpdf requests`

### Cloud Run deployment failing?
- Verify `gcloud` is authenticated
- Check billing is enabled
- Ensure Dockerfile is correct
- Port must be `8080`

---

## 📜 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgments

- **Google** — Colab AI (Gemini 2.5 Flash) + Cloud Platform
- **Replicate** — Image generation
- **LLMsRelay** — Claude API
- **Flask** — Web framework

---

## 🏆 Built With

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![Flask](https://img.shields.io/badge/Flask-3.0-green)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow)
![HTML5](https://img.shields.io/badge/HTML5-orange)
![CSS3](https://img.shields.io/badge/CSS3-blue)
![Google Cloud](https://img.shields.io/badge/Google%20Cloud-blue)

---

> **"Gonna Take the Boredom"** — Transform ideas into reality ⚡

---

**Built with ❤️ on Google Cloud Platform**
```

---
