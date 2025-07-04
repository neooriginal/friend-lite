# Own your Friend DevKit, DevKit 2, OMI

## Overview
This repository provides everything needed to build your own audio-first AI system using OMI/Friend devices. Choose from multiple backend options based on your needs, from simple audio processing to full AI-powered conversation management.

**Core Components:**
- 📱 **Mobile App** - React Native app for Bluetooth streaming
- 🖥️ **Backends** - Multiple backend options for different use cases  
- 🔧 **Services** - Additional ASR and speaker recognition services

## Architecture
![Architecture Diagram](.assets/plan.png)

Your OMI device streams audio via Bluetooth (OPUS codec) → Backend processes and transcribes → AI extracts memories/actions → Storage (MongoDB, Qdrant)

## Quick Start

### 1. Choose Your Backend

#### 🚀 **Advanced Backend** (Recommended)
**Location:** `backends/advanced-backend/`  
**Best for:** Production use, full AI features

**Setup:**
```bash
cd backends/advanced-backend
cp .env.template .env
# Edit .env with your settings
docker compose up --build -d
```

**Services included:**
- Backend API (port 8000)
- Web UI (port 8501) 
- MongoDB (port 27017)
- Qdrant vector DB (port 6333)
- Optional: Ollama, Speaker Recognition, Ngrok

**Features:**
- ✅ Memory system with vector storage
- ✅ Speaker recognition & enrollment  
- ✅ Action items extraction
- ✅ Conversation management
- ✅ Web UI for monitoring
- ✅ Multiple ASR options (Deepgram + offline)
- ✅ Audio cropping & optimization

<details>
<summary><strong>Advanced Setup Details</strong></summary>

**Required Environment Variables:**
```bash
# ASR Configuration (choose one)
DEEPGRAM_API_KEY=your_key_here           # For Deepgram API
OFFLINE_ASR_TCP_URI=tcp://192.168.0.110:8765/  # For offline ASR

# AI Configuration  
OLLAMA_BASE_URL=http://localhost:11434   # For local Ollama
HF_TOKEN=your_huggingface_token         # For speaker recognition

# Optional Services
SPEAKER_SERVICE_URL=http://localhost:8001  # If using speaker recognition
NGROK_AUTHTOKEN=your_ngrok_token          # For public access
```

**First Time Setup:**
1. `cd backends/advanced-backend`
2. `cp .env.template .env` 
3. Edit `.env` with your configuration
4. `docker compose up --build -d`
5. Wait 2-3 minutes for all services to start
6. Access Web UI at `http://localhost:8501`

**Optional: Enable Speaker Recognition**
Uncomment the `speaker-recognition` service in `docker-compose.yml` and set `SPEAKER_SERVICE_URL` in `.env`

**Optional: Enable Ollama**  
Uncomment the `ollama` service in `docker-compose.yml` and ensure `OLLAMA_BASE_URL` points to it
</details>

---

#### Other Backend Options

<details>
<summary><strong>Simple Backend</strong> - `backends/simple-backend/`</summary>

**Best for:** Learning, basic audio processing

```bash
cd backends/simple-backend  
cp .env.template .env
docker compose up --build -d
```

**Services:** Backend API (8000), Ngrok  
**Features:** Basic OPUS→WAV conversion, file storage  
**Output:** Audio chunks saved to `./audio_chunks/`
</details>

<details>
<summary><strong>OMI Webhook Compatible</strong> - `backends/others/omi-webhook-compatible/`</summary>

**Best for:** Existing OMI users, easy migration

```bash
cd backends/others/omi-webhook-compatible
cp .env.template .env  
# Add NGROK_TOKEN to .env
docker compose up --build -d
```

**Setup:** Point OMI app to `<ngrok-url>/webhook/audio_bytes`  
**Features:** Drop-in OMI replacement, webhook system
</details>

<details>
<summary><strong>Example Satellite</strong> - `backends/others/example-satellite/`</summary>

**Best for:** Wyoming protocol, Home Assistant integration

```bash  
cd backends/others/example-satellite
docker compose up --build -d
# Or run directly:
uv run python main.py --omi-mac <mac> --wake-uri <uri>
```

**Features:** Wyoming satellite, streams to remote ASR servers  
**Use case:** Distributed setups, HA integration
</details>

---

### 2. Setup Mobile App

**Location:** `friend-lite/`

```bash
cd friend-lite
npm install
# For iOS: npx expo run:ios  
# For Android: npx expo run:android
```

Configure app to point to your backend's IP address and port.

### 3. Additional Services (Optional)

<details>
<summary><strong>ASR Services</strong> - `extras/asr-services/`</summary>

Self-hosted transcription options:

```bash
cd extras/asr-services
# Moonshine (fast):
docker compose up moonshine --build -d
# Parakeet (alternative):  
docker compose up parakeet --build -d
```
</details>

<details>
<summary><strong>Speaker Recognition</strong> - `extras/speaker-recognition/`</summary>

Standalone speaker identification:

```bash
cd extras/speaker-recognition
docker compose up --build -d
```
Port: 8001, Features: REST API for speaker operations
</details>

<details>
<summary><strong>HAVPE Relay</strong> - `extras/havpe-relay/`</summary>

Audio relay service:

```bash
cd extras/havpe-relay  
docker compose up --build -d
```
</details>

## Usage Recommendations

**Beginners:** Start with Simple Backend → understand audio flow → upgrade to Advanced  
**Production:** Use Advanced Backend with full stack (MongoDB + Qdrant + optional Ollama)  
**OMI Users:** Use OMI Webhook Compatible for seamless migration  
**Home Assistant:** Use Example Satellite + ASR Services for Wyoming integration

## Architecture Details

**Audio Flow:** OMI Device → Bluetooth → Mobile App → Backend → ASR → AI Processing → Storage  
**Storage:** MongoDB (conversations), Qdrant (memory vectors), File system (audio)  
**Protocols:** Wyoming protocol for ASR compatibility, REST APIs for all services

## Repository Structure

```
friend-lite/
├── friend-lite/          # React Native mobile app
├── backends/
│   ├── advanced-backend/     # Full-featured AI backend ⭐  
│   ├── simple-backend/       # Basic audio processing
│   └── others/
│       ├── example-satellite/        # Wyoming satellite
│       └── omi-webhook-compatible/   # OMI migration
└── extras/               # Optional services
    ├── asr-services/         # Offline transcription
    ├── speaker-recognition/  # Speaker identification  
    └── havpe-relay/         # Audio relay
```

Each directory contains its own `docker-compose.yml` for easy deployment.