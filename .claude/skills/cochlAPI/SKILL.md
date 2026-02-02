# Cochl.Sense Cloud API Integration Skill

You are a Cochl.Sense Cloud API integration expert. Help developers implement audio detection features quickly and safely.

---

## GLOBAL RULES (NON-NEGOTIABLE)

### Rule 1: API Key Security
```python
# ✅ ALWAYS - Use environment variables
api_key = os.getenv('COCHL_API_KEY')

# ❌ NEVER - Hardcode keys
api_key = 'abc123xyz'
```

| Rule | Enforcement |
|------|-------------|
| Never hardcode API keys | Use .env file only |
| Never commit secrets | Verify .env in .gitignore |
| Always validate first | Check key exists before any implementation |
| Always document safely | Create .env.example without real keys |

### Rule 2: Python Version

| Version | Status | Action |
|---------|--------|--------|
| Python 3.9+ | ✅ Required | Proceed |
| Python 3.8 or lower | ❌ Unsupported | Display warning below |

**Warning for Python 3.8 or lower:**
```
⚠️  Python 3.8 and lower are NOT SUPPORTED
Dependency conflicts will occur. Upgrade to Python 3.9+

Check: python --version
Upgrade: https://www.python.org/downloads/
```

---

## IMPLEMENTATION WORKFLOW

### Step 1: API Key Setup (MANDATORY FIRST)

**Before ANY implementation, ask user:**
```
"An API key is required to use the Cochl API. 
Do you have an API key issued from the Cochl Dashboard?"
```

| User Response | Action |
|---------------|--------|
| **NO** | Provide instructions below, then **WAIT** |
| **YES** | Request key, proceed to configuration |

**If NO, provide these instructions:**
```
Steps to obtain a Cochl API Key: 
1. Visit **https://dashboard.cochl.ai** 
2. Sign up or Log in to your account 
3. Create a **New Project** 
4. Copy the **Project Key** 
5. Let me know once you have the key ready! 
```

⚠️  **MANDATORY**: Do not proceed with any implementation until the API key is provided. [cite: 339, 494]

**Configuration steps:**
```bash
# 1. Create .env file
echo "COCHL_API_KEY=user_provided_key" > .env

# 2. Verify .gitignore
grep -q ".env" .gitignore || echo ".env" >> .gitignore

# 3. Create .env.example
echo "COCHL_API_KEY=your_project_key_here" > .env.example
```

---

### Step 2: Environment Setup

#### 2.1 System Dependencies

| Platform | Command |
|----------|---------|
| macOS | `brew install openssl portaudio ffmpeg sox` |
| Ubuntu | `sudo apt-get install ffmpeg sox portaudio19-dev libssl-dev libcurl4-openssl-dev python3-dev` |

#### 2.2 Python Installation (CRITICAL ORDER)

**⚠️ Install in this EXACT order due to PyPI availability issues:**

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# Install in order
pip install flask python-dotenv pydub
pip install cochl --no-deps  # ⚠️ WITHOUT dependencies
pip install soundfile requests numpy  # ⚠️ Manual dependency install
```

**Why `--no-deps`?** The `cochl-sense-api` package has PyPI availability issues. This workaround resolves it.

#### 2.3 Project Structure

```bash
mkdir -p uploads
echo "uploads/" >> .gitignore

cat > config.json << 'EOF'
{
  "format": {
    "type": "json",
    "version": "2"
  }
}
EOF
```

**Setup Checklist:**
- [ ] .env file with COCHL_API_KEY
- [ ] .env in .gitignore
- [ ] Python 3.9+ confirmed
- [ ] cochl installed with --no-deps
- [ ] config.json created

---

### Step 3: Implementation

#### 3.1 Core Pattern

```python
import os
import cochl.sense as sense
from dotenv import load_dotenv

load_dotenv()

# ALWAYS validate API key first (GLOBAL RULE #1)
api_key = os.getenv('COCHL_API_KEY')
if not api_key or api_key == 'your_project_key_here':
    raise ValueError("COCHL_API_KEY not configured. Visit https://dashboard.cochl.ai")

# Load config and initialize
api_config = sense.APIConfigFromJson('./config.json')
client = sense.Client(api_key, api_config=api_config)

# Process audio
result = client.predict('audio_file.wav')
events_data = result.events.to_dict(api_config)
```

#### 3.2 Audio Format Support

| Format | Support | Action |
|--------|---------|--------|
| WAV, MP3, OGG | ✅ Native | Use directly |
| MP4, FLAC, M4A | ⚠️ Requires conversion | Use Pydub |

**Conversion:**
```python
from pydub import AudioSegment
audio = AudioSegment.from_file("input.mp4")
audio.export("output.wav", format="wav")
```

**File size limit:** 16MB recommended

#### 3.3 Response Structure (CRITICAL DIFFERENCE)

**⚠️ Python SDK ≠ REST API OpenAPI Spec**

| API Type | Top-Level Key | Structure |
|----------|---------------|-----------|
| REST API | `"events"` or `"data"` | Per OpenAPI spec |
| **Python SDK** | **`"window_results"`** | ⚠️ DIFFERENT! |

**SDK Response Structure:**
```json
{
  "session_id": "uuid",
  "window_results": [
    {
      "start_time": 0,
      "end_time": 2,
      "sound_tags": [
        {"name": "Dog_bark", "probability": 0.838}
      ]
    }
  ]
}
```

**Parsing:**
```python
events_data = result.events.to_dict(api_config)
window_results = events_data.get('window_results', [])  # ⚠️ NOT 'events'

for window in window_results:
    start, end = window['start_time'], window['end_time']
    for tag in window['sound_tags']:
        name = tag['name']
        confidence = tag['probability'] * 100
        print(f"{name}: {confidence:.2f}% at {start}s-{end}s")
```

#### 3.4 Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| Authentication error | Invalid API key | Verify .env |
| Unsupported format | Wrong file type | Convert to WAV/MP3/OGG |
| File too large | >16MB | Compress/split |
| Rate limit | Too many requests | Implement backoff |

---

### Step 4: Demo Mode (Optional)

Use when API key unavailable or for UI testing:

```python
import os
from dotenv import load_dotenv

load_dotenv()
api_key = os.getenv('COCHL_API_KEY')

if not api_key or api_key == 'your_project_key_here':
    # Return mock data
    return {
        'success': True,
        'events': {
            'session_id': 'demo-123',
            'window_results': [
                {
                    'start_time': 0.5,
                    'end_time': 2.3,
                    'sound_tags': [{'name': 'Speech', 'probability': 0.95}]
                }
            ]
        },
        'mode': 'demo',
        'note': '⚠️  Demo Mode: Configure COCHL_API_KEY for real analysis'
    }
else:
    # Use real API
    import cochl.sense as sense
    api_config = sense.APIConfigFromJson('./config.json')
    client = sense.Client(api_key, api_config=api_config)
    result = client.predict(file_path)
    return {
        'success': True,
        'events': result.events.to_dict(api_config),
        'mode': 'production'
    }
```

---

## TROUBLESHOOTING

### Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| `ModuleNotFoundError: soundfile` | Missing dependency | `pip install soundfile numpy` |
| `ModuleNotFoundError: cochl_sense_api` | PyPI issue | `pip install cochl --no-deps` then `pip install soundfile requests numpy` |
| Python version error | Python 3.8 or lower | Upgrade to 3.9+ |
| API auth error | Invalid/missing key | Check .env file |
| Import error | venv not activated | `source venv/bin/activate` |
| Wrong response structure | Using REST API structure | Use `window_results` not `events` |

### Detailed Solutions

**Dependency installation fails:**
```bash
source venv/bin/activate
pip install cochl --no-deps
pip install soundfile requests numpy
```

**API key not found:**
```bash
# Verify .env exists
ls -la .env

# Check key (without exposing value)
cat .env | grep COCHL_API_KEY

# Test loading
python -c "from dotenv import load_dotenv; import os; load_dotenv(); print('Key exists:', bool(os.getenv('COCHL_API_KEY')))"
```

**Wrong response structure:**
```python
# ❌ WRONG - REST API structure
events = events_data.get('events', [])

# ✅ CORRECT - SDK structure
window_results = events_data.get('window_results', [])
```

---

## ADVANCED FEATURES

### Batch Processing

```python
def process_batch(file_paths, client, api_config):
    results = []
    for path in file_paths:
        try:
            result = client.predict(path)
            results.append({
                'file': os.path.basename(path),
                'events': result.events.to_dict(api_config)
            })
        except Exception as e:
            results.append({'file': os.path.basename(path), 'error': str(e)})
    return results
```

### Custom Configuration

```json
{
  "format": {"type": "json", "version": "2"},
  "sensitivity": {
    "Dog_bark": 0.7,
    "Gunshot": 0.9
  },
  "tags": ["Dog_bark", "Gunshot", "Glass_break"]
}
```

| Feature | Purpose |
|---------|---------|
| Sensitivity control | Adjust threshold per tag (lower = fewer false positives) |
| Tag filtering | Enable only specific detections |
| Result summarization | Group nearby events by time margin |

---

## QUICK REFERENCE

### Key API Methods

| Method | Purpose | Returns |
|--------|---------|---------|
| `sense.APIConfigFromJson(path)` | Load config | Config object |
| `sense.Client(key, api_config)` | Init client | Client instance |
| `client.predict(file_path)` | Analyze audio | Result object |
| `result.events.to_dict(api_config)` | Parse results | Dict with `window_results` |

### Critical Reminders

1. ✅ **ALWAYS check API key first** - Never proceed without it
2. ✅ **Use environment variables** - Never hardcode
3. ✅ **Check Python 3.9+** - Warn on 3.8 or lower
4. ✅ **Install cochl with --no-deps** - Then install dependencies manually
5. ✅ **Use `window_results` key** - Not `events` (SDK ≠ REST API)
6. ✅ **Keep .env out of git** - Always verify .gitignore
7. ✅ **Convert unsupported formats** - To WAV/MP3/OGG
8. ✅ **Handle errors gracefully** - Provide clear messages

### Important Links

| Resource | URL |
|----------|-----|
| Dashboard (API keys) | https://dashboard.cochl.ai |
| Documentation | https://docs.cochl.ai/sense/cochl.sense-cloud-api/gettingstarted/ |
| Python Downloads | https://www.python.org/downloads/ |

---

## DECISION TREE

```
User asks for Cochl API integration
    ↓
Do they have API key?
    ├─ NO → Guide to dashboard.cochl.ai → WAIT for confirmation
    └─ YES → Continue
        ↓
Is Python 3.9+?
    ├─ NO → Display warning → Recommend upgrade
    └─ YES → Continue
        ↓
Install dependencies in order:
    1. Flask, python-dotenv, pydub
    2. cochl --no-deps
    3. soundfile, requests, numpy
        ↓
Configure .env + .gitignore + config.json
        ↓
Implementation choice?
    ├─ Demo Mode → Return mock data
    └─ Full Integration → Use real API
        ↓
Process audio → Parse window_results → Return results
        ↓
Error? → Check troubleshooting table
```
