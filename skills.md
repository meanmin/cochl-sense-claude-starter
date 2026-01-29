# Cochl Sound Analysis Skill

## Description
비언어적 소리(기침, 비명, 유리 깨짐, 사이렌 등)를 실시간 또는 파일 단위로 분석하여 감지하는 기능입니다.

## Triggers
- "소리 분석해줘", "소리 들려?", "특정 소리 감지하면 알려줘"
- "Sound detection", "Audio analysis", "Emergency sound alert"

## Capabilities
1. **환경 인식**: `.claudeprompt`의 지침에 따라 Cochl SDK 환경을 자동 구축합니다.
2. **소리 분석**: 오디오 파일을 Cochl.Sense Cloud API에 전송하고 결과를 파싱합니다.

## Best Practices
- 항상 `cochl.sense` 공식 라이브러리 사용을 우선합니다.
- API Key는 환경 변수(`COCHL_API_KEY`)에서 먼저 찾도록 권장합니다.