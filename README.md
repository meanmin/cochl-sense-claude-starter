# cochl-sense-claude-starter

**Just let your apps listen it!** Cochl.Sense와 Claude가 만나면 단 몇 줄의 대화로 강력한 소리 인식 서비스를 만들 수 있습니다.

## 🛠️ 시작하기 전 준비 사항 (Getting Started)

Cochl.Sense Cloud API를 사용하기 위해 아래 단계를 먼저 진행해 주세요.

### Step 1. Python 환경 설정
Cochl 라이브러리는 **Python 3.9 이상** 버전이 필요합니다. 터미널에서 아래 명령어를 순서대로 입력하세요.


# 1. 가상 환경 생성 (권장)
`python3 -m venv venv`

# 2. 가상 환경 활성화 (Mac/Linux)
`source venv/bin/activate`

# 3. pip 및 cochl 라이브러리 설치/업데이트
`pip install --upgrade pip`
`pip install --upgrade cochl`

### Step 2. 
1. **Cochl Dashboard**(https://dashboard.cochl.ai)에 접속해 가입하세요.
2. **[+New project]**를 누르고 프로젝트 타입을 **Cloud API**로 선택하여 생성합니다.
3. 생성된 프로젝트의 **Project Key**를 복사해 두세요. 이 키가 있어야 Claude가 API를 호출할 수 있습ㄴ디ㅏ. 

## ✨ Magic Moment: 60초 만에 끝내는 설치
1. **가이드 장착**: 이 저장소의 `.claudeprompt` 파일을 만들고자 하는 프로젝트 폴더에 넣으세요.
2. **클로드 실행**: 터미널에서 `claude`를 입력합니다.
3. **명령**: *"방금 발급받은 API 키는 'YOUR_KEY'야. 이 폴더의 오디오 파일을 분석해서 '비명(Scream)'이 들리면 알려주는 파이썬 코드를 짜줘."*

## 🛠️ 주요 기능
- **고정밀 소리 인식**: 사이렌, 유리 깨지는 소리, 기침 등 113개 이상의 공식 태그 지원.
- **실시간 세션 관리**: 대용량 오디오 파일도 청크 단위로 나누어 안정적으로 분석.
- **사용자 지정 민감도**: 특정 소리에 더 민감하게 반응하도록 설정 가능.

---
© 2026 Cochl.ai | [Documentation](https://docs.cochl.ai)