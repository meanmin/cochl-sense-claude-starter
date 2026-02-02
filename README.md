# Cochl.Sense 소리 분석기

Cochl.Sense API를 활용하여 오디오 파일을 업로드하면 어떤 소리인지 AI가 자동으로 분석해주는 웹 애플리케이션입니다.

## 주요 기능

- 오디오 파일 업로드 (MP3, WAV, OGG)
- AI 기반 소리 이벤트 자동 감지
- 신뢰도 및 시간 정보 표시
- 깔끔한 웹 인터페이스

## 설치 방법

### 1. 시스템 요구사항

**Python 버전:**
- Python 3.9 이상 필수

**시스템 의존성 (macOS):**
```bash
brew install openssl portaudio ffmpeg sox
```

**시스템 의존성 (Ubuntu):**
```bash
sudo apt-get update
sudo apt-get install ffmpeg sox portaudio19-dev libssl-dev libcurl4-openssl-dev python3-dev
```

### 2. 가상환경 생성 및 활성화

```bash
python3 -m venv venv
source venv/bin/activate  # macOS/Linux
# 또는
venv\Scripts\activate  # Windows
```

### 3. 의존성 설치

```bash
# 기본 패키지 설치
pip install flask python-dotenv pydub

# cochl 패키지 설치 (의존성 없이)
pip install cochl --no-deps

# cochl 의존성 수동 설치
pip install soundfile requests numpy
```

### 4. API 키 설정

1. [Cochl Dashboard](https://dashboard.cochl.ai)에 접속
2. 회원가입 또는 로그인
3. 새 프로젝트 생성
4. 프로젝트 키 복사
5. `.env` 파일에 API 키 설정:

```bash
COCHL_API_KEY=your_api_key_here
```

현재 프로젝트에는 이미 API 키가 설정되어 있습니다.

## 사용 방법

### 1. 애플리케이션 실행

```bash
python app.py
```

### 2. 웹 브라우저에서 접속

```
http://localhost:5000
```

### 3. 소리 파일 분석하기

1. 웹 페이지에서 "클릭하거나 파일을 드래그하여 업로드" 영역을 클릭
2. 분석하고 싶은 오디오 파일 선택 (MP3, WAV, OGG 지원)
3. "분석하기" 버튼 클릭
4. AI가 분석한 결과 확인
   - 감지된 소리 종류
   - 신뢰도 (%)
   - 시작/종료 시간

## 지원 파일 형식

- MP3
- WAV
- OGG

최대 파일 크기: 16MB

## 프로젝트 구조

```
cochl-sense-claude-starter/
├── app.py              # Flask 애플리케이션
├── config.json         # Cochl API 설정
├── requirements.txt    # Python 의존성
├── .env               # API 키 (보안 파일, git에 커밋 안 됨)
├── .env.example       # 환경변수 예시
├── templates/
│   └── index.html     # 웹 인터페이스
└── uploads/           # 임시 업로드 폴더 (자동 생성)
```

## 문제 해결

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

### Python 버전 호환성 문제

Python 3.9 이상으로 업그레이드를 권장합니다:

```bash
python3 --version  # 버전 확인
```

### API 키 오류

1. `.env` 파일이 프로젝트 루트에 있는지 확인
2. `COCHL_API_KEY` 변수명이 정확한지 확인
3. API 키에 공백이나 따옴표가 없는지 확인
4. 애플리케이션 재시작

## 참고 자료

- [Cochl Dashboard](https://dashboard.cochl.ai) - API 키 관리
- [Cochl 공식 문서](https://docs.cochl.ai/sense/cochl.sense-cloud-api/gettingstarted/)
- [Cochl.Sense 기술 소개](https://www.cochl.ai/ko)

## 라이센스

이 프로젝트는 Cochl API 활용 샘플 프로젝트입니다.
