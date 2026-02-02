# Cochl.Sense Sound Analyzer

A web application that automatically analyzes audio files using the Cochl.Sense API to identify sound events with AI.

## Key Features

- Audio file upload (MP3, WAV, OGG)
- AI-based automatic sound event detection
- Confidence scores and time information display
- Clean web interface

## Installation

### 1. System Requirements

**Python Version:**
- Python 3.9 or higher required

**System Dependencies (macOS):**
```bash
brew install openssl portaudio ffmpeg sox
```

**System Dependencies (Ubuntu):**
```bash
sudo apt-get update
sudo apt-get install ffmpeg sox portaudio19-dev libssl-dev libcurl4-openssl-dev python3-dev
```

### 2. Create and Activate Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate  # macOS/Linux
# or
venv\Scripts\activate  # Windows
```

### 3. Install Dependencies

```bash
# Install base packages
pip install flask python-dotenv pydub

# Install cochl package (without dependencies)
pip install cochl --no-deps

# Manually install cochl dependencies
pip install soundfile requests numpy
```

### 4. API Key Setup

1. Visit [Cochl Dashboard](https://dashboard.cochl.ai)
2. Sign up or log in
3. Create a new project
4. Copy the project key
5. Set the API key in `.env` file:

```bash
COCHL_API_KEY=your_api_key_here
```

An API key is already configured in the current project.

## Usage

### 1. Run the Application

```bash
python app.py
```

### 2. Access via Web Browser

```
http://localhost:5000
```

### 3. Analyze Audio Files

1. Click the "Click or drag files to upload" area on the web page
2. Select the audio file you want to analyze (MP3, WAV, OGG supported)
3. Click the "Analyze" button
4. View the AI analysis results:
   - Detected sound types
   - Confidence score (%)
   - Start/end times

## Supported File Formats

- MP3
- WAV
- OGG

Maximum file size: 16MB

## Project Structure

```
cochl-sense-claude-starter/
├── app.py              # Flask application
├── config.json         # Cochl API configuration
├── requirements.txt    # Python dependencies
├── .env               # API key (secure file, not committed to git)
├── .env.example       # Environment variable example
├── templates/
│   └── index.html     # Web interface
└── uploads/           # Temporary upload folder (auto-created)
```

## Troubleshooting

### ModuleNotFoundError: No module named 'soundfile'

```bash
source venv/bin/activate
pip install soundfile numpy
```

### ModuleNotFoundError: No module named 'cochl_sense_api'

```bash
pip install cochl --no-deps
pip install soundfile requests numpy
```

### Python Version Compatibility Issues

We recommend upgrading to Python 3.9 or higher:

```bash
python3 --version  # Check version
```

### API Key Errors

1. Verify that the `.env` file is in the project root
2. Confirm that the variable name `COCHL_API_KEY` is correct
3. Ensure there are no spaces or quotes in the API key
4. Restart the application

## References

- [Cochl Dashboard](https://dashboard.cochl.ai) - API key management
- [Cochl Official Documentation](https://docs.cochl.ai/sense/cochl.sense-cloud-api/gettingstarted/)
- [Cochl.Sense Technology Overview](https://www.cochl.ai/ko)

## License

This project is a sample project utilizing the Cochl API.
