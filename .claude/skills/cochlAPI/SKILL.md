---
name: cochlAPI
description: Cochl.Sense 소리 인식 AI를 사용하여 비언어적 소리(기침, 비명, 유리 깨짐, 사이렌 등)를 분석합니다. "소리 분석해줘", "Sound detection", "오디오 분석" 등의 요청에 사용하세요.
---

# Cochl.Sense API Expert (v1.7.12)

당신은 Cochl.Sense(소리 인식 AI) 전문가입니다.

## 1. 환경 자동 구축 (Auto-Setup)

프로젝트에 `venv` 폴더나 `cochl` 라이브러리가 없다면 즉시 다음을 수행하세요:

```bash
python3 -m venv venv
. venv/bin/activate
pip install --upgrade pip
pip install --upgrade cochl
git clone https://github.com/cochlearai/cochl-sense-py.git
cd cochl-sense-py/samples
```

이 과정에서 python3나 pip가 없다면 추가로 설치해주세요

## 2. API 기술 규격

- **Endpoint**: `POST https://api.cochl.ai/sense/api/v1/audio_sessions/`
- **인증**: `Authorization: Bearer {COCHL_API_KEY}` 헤더
- **Session Body**:
```json
{
  "type": "file",
  "content_type": "audio/x-raw; rate=44100; format=s24le",
  "tags_sensitivity": { "Siren": -2, "Emergency_vehicle_siren": -1 },
  "default_sensitivity": 0,
  "window_hop": "0.5s"
}
```

## 3. 기본 사용법

```python
from cochl.sense import Sense

# API Key는 환경변수 COCHL_API_KEY에서 우선 로드
sense = Sense(api_key="YOUR_API_KEY")

# 파일 분석
result = sense.file("audio.wav")
for event in result.events:
    print(f"[{event.start_time}s] {event.tag}: {event.probability:.2%}")
```

## 4. 지원 태그

- **긴급**: Siren, Emergency_vehicle_siren, Scream, Gunshot
- **생활**: Baby_cry, Dog_bark, Glass_breaking, Cough
- **기타**: Knock, Alarm, Speech, Music

## 5. Best Practices

- API Key는 환경변수(`COCHL_API_KEY`)에서 우선 로드
- `cochl.sense` 공식 라이브러리 사용 우선
- 오디오 포맷: WAV, MP3, FLAC (44.1kHz 권장)
