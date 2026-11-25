# AI 비서 통합 회의실 — 사용 설명서

파일 하나(`index.html`)로 끝나는 웹앱입니다. 서버도, 설치도 필요 없습니다.

## 1. 바로 열기
- `index.html`을 더블클릭하면 브라우저에서 열립니다(PC·스마트폰 모두).
- API 키가 없어도 **데모 모드**로 모든 기능(멘션·대시보드·요약·할 일 뽑기)이 그대로 작동합니다.

## 2. 실제 AI 연결하기 (무료 키)
회의실 오른쪽 위 **⚙️ → API 키 관리**에서 붙여넣고 저장하면 됩니다. 키는 그 브라우저(localStorage)에만 저장됩니다.

| 직원 | 제공사 | 키 발급 주소 | 기본 모델 |
|---|---|---|---|
| 리서처 | Google Gemini | https://aistudio.google.com/app/apikey | `gemini-3.8-flash` |
| 기획자·개발자 | Groq | https://console.groq.com/keys | `openai/gpt-oss-120b` |
| 마케터 | OpenRouter | https://openrouter.ai/keys | `google/gemma-4-31b-it:free` |

> 2026년 11월 기준: Gemini 2.0 Flash(2026-06-01)와 Groq `llama-3.3-70b-versatile`(2026-08-16)는 서비스가 끝나서, 현재 살아 있는 모델을 기본값으로 넣었습니다. 모델 이름은 ⚙️에서 직접 바꿀 수 있습니다.
> API 키가 맞지 않거나 한도를 넘으면 **자동으로 데모 답변**으로 넘어가고, 대화창에 그 이유가 안내됩니다.

## 3. 무료로 배포하기 (주소 만들기)
- **GitHub Pages**: 새 저장소에 `index.html`을 올리고 `Settings → Pages → Branch: main / (root)` 저장 → 1~2분 뒤 주소 생성.
- **Vercel**: https://vercel.com → Add New → Project → 저장소 선택 → Framework는 `Other`, Build 명령 없이 Deploy. `index.html`이 그대로 웹 주소가 됩니다.
- 배포 후에는 `https://…` 주소로 열기 때문에 휴대폰에서도 홈 화면에 추가해 앱처럼 쓸 수 있습니다.

## 4. 데이터 보관
- 대화·사용량·키는 모두 **그 브라우저의 localStorage**에만 있습니다. 서버로 모으지 않습니다.
- 다른 기기와 대화를 공유하려면 ☰ → **대화 내보내기(.md)** 로 파일을 저장해 옮기세요.
- 공용 PC에서는 키를 저장하지 마세요(⚙️ 맨 아래 “모두 지우기”로 삭제 가능).
