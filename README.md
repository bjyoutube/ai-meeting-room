# 🏢 1인 기업 사이버 오피스

파일 하나(`index.html`)로 끝나는 사내 오피스 웹앱입니다. 서버도, 설치도 필요 없습니다.

- 주소: **https://bjyoutube.github.io/ai-meeting-room/**
- 저장소: **bjyoutube/ai-meeting-room** (공개)

## 1. 직원 6명 (두 팀)

### 피클 팀 — GitHub 공용 파일(`office-messages.json`)로 움직임
| 멘션 | 역할 | 움직이는 방식 |
|---|---|---|
| `@방카르1` | 팀장 · 실행/총괄 | 여기서 지시 → 공용 파일에 커밋 → 피클이 보고를 남기면 화면에 합쳐짐 |
| `@방카르2` | 리서처 · 시장조사 | 위와 같음 |
| `@방카르3` | 마케터 · 홍보/지원 | 위와 같음 |

### 외부 AI 팀 — 이 브라우저에서 실시간 API 호출
| 멘션 | 역할 | 기본 연결 |
|---|---|---|
| `@GPT` | 기획 · 사업성 분석 | OpenAI 직통(`api.openai.com`) 또는 OpenRouter(`openai/gpt-4o-mini`, `openai/gpt-oss-120b`) |
| `@클로드` | 카피 · 전략/문서 | OpenRouter(`anthropic/claude-3.5-sonnet`) 우선, Anthropic 직통은 CORS 때문에 실패할 수 있음 |
| `@제미나이` | 빠른 초안 · 브레인스토밍 | Google AI Studio(`gemini-2.5-flash` 등, ⚙️에서 모델 변경 가능) |

- `@전체`: 연결된 직원이 각자 관점으로 차례로 답합니다. 연결이 없는 직원은 건너뛰고 한 번만 안내합니다.
- 멘션이 없으면: 공용창(PAT)이 연결돼 있으면 `@방카르1`, 아니면 `@GPT`에게 갑니다.
- 키가 없으면 **데모 모드**로 모든 기능이 그대로 동작합니다(답변마다 `데모`/`실제 API`/`공용창` 라벨).

## 2. 연결하기 (오른쪽 위 ⚙️)

모든 값은 **그 브라우저의 localStorage**에만 저장되고, 답변을 만들 때 해당 제공사로만 전송됩니다.

| 항목 | 발급 | 권한 |
|---|---|---|
| OpenAI API 키 | https://platform.openai.com/api-keys | — |
| OpenRouter API 키 | https://openrouter.ai/keys | — |
| Anthropic API 키 | https://console.anthropic.com/settings/keys | — |
| Google AI Studio 키 | https://aistudio.google.com/app/apikey | — |
| GitHub PAT | https://github.com/settings/tokens | Fine-grained → Repository access: `bjyoutube/ai-meeting-room`, Permissions → **Contents: Read and write** |

PAT를 넣고 **저장하기** → ⚙️의 **공용창 연결 확인** 버튼으로 쓰기 권한을 확인할 수 있습니다.

> OpenRouter 요청 헤더(`HTTP-Referer`, `X-Title`)에는 ASCII만 사용합니다(`AI Cyber Office`). 한글이 들어가면 브라우저가 요청 자체를 거부합니다.

## 3. 공용 소통창 양방향 동기화

- **읽기 (토큰 불필요)**: `https://raw.githubusercontent.com/bjyoutube/ai-meeting-room/main/office-messages.json`
  - ⟳ 버튼으로 새로고침, 🔄 버튼으로 **20초 자동 새로고침** 켜기/끄기.
- **쓰기 (PAT 필요)**: GitHub Contents API `PUT /repos/bjyoutube/ai-meeting-room/contents/office-messages.json`
  - `@방카르N`에게 지시하면 파일에 `type: "instruction"` 항목이 커밋됩니다.
  - 피클이 `type: "report"` 항목을 `replyTo`로 남기면 화면에 `🐙 공용창` 라벨로 합쳐집니다.

### 파일 형식
```json
{
  "updatedAt": "2026-11-26T09:22:00.000Z",
  "messages": [
    {
      "id": "msg_20261126_1822_a1",
      "ts": "2026-11-26T09:22:00.000Z",
      "from": "owner",
      "to": "bangkar1",
      "type": "instruction",
      "text": "@방카르1 몽골 중고차 수출 손익 계산해줘",
      "replyTo": null
    },
    {
      "id": "msg_20261126_1835_b2",
      "ts": "2026-11-26T09:35:00.000Z",
      "from": "bangkar1",
      "to": "owner",
      "type": "report",
      "text": "보고: 손익분기 월 39건 ...",
      "replyTo": "msg_20261126_1822_a1"
    }
  ]
}
```

- `from`: `owner` | `bangkar1` | `bangkar2` | `bangkar3`
- `type`: `instruction`(지시) | `report`(보고) | `note`(메모)
- 피클이 보고를 추가할 때: 기존 `messages`를 읽어 **뒤에 덧붙이고**, `updatedAt`을 갱신해 같은 경로에 커밋합니다. `to`는 `owner`, `replyTo`는 응답하는 지시의 `id`를 넣으면 화면에서 “보고 도착”으로 표시됩니다.

## 4. 화면 구성

- **사이버 오피스(상단)**: 직원 6명 카드 — 역할 · 상태 배지(🟢 대기 중 / 🟡 작업 중 / ✅ 보고 완료) · 연결(API/GitHub) · 마지막 활동 · 남은 한도(외부 AI) 또는 공용 파일 연결(피클 팀).
  - 작업 중에는 카드 테두리·아바타 펄스와 상단 진행 바 애니메이션이 돕니다.
  - PC 3열 / 모바일 2열 반응형, 오피스 영역은 접기(🏢·✕) 지원, 모바일에서는 내부 스크롤.
- **대화**: 메시지마다 `데모` / `실제 API` / `🐙 공용창` 라벨, 멘션 강조, 마크다운 최소 지원.
- **퀵 액션**: 🗒️ 회의 정리 · ✅ 할 일 뽑기 · ✍️ 초안 쓰기.
- **저장**: 대화·키·사용량은 localStorage(☰ → 대화 내보내기 .md 가능).

## 5. 배포

`main` 브랜치 루트에 `index.html`이 있으면 GitHub Pages가 `https://bjyoutube.github.io/ai-meeting-room/`로 서빙합니다(`.nojekyll` 포함).

```
git add index.html README.md .nojekyll office-messages.json
git commit -m "사이버 오피스 업그레이드"
git push origin main
```

## 6. 문제 해결

- **“PAT 권한이 부족합니다 (403)”** → 토큰 Fine-grained 권한에서 `Contents: Read and write`가 켜져 있는지, 저장소가 `bjyoutube/ai-meeting-room`인지 확인.
- **“커밋이 겹쳤습니다”** → 다른 곳에서 동시에 고친 경우입니다. 잠시 뒤 다시 지시하면 자동으로 다시 읽고 커밋합니다.
- **모델을 찾을 수 없어요** → 제공사가 모델을 교체했을 수 있습니다. ⚙️에서 모델 이름을 바꾸세요.
- **클로드 직통이 연결되지 않음** → 브라우저 CORS 제한입니다. OpenRouter 키를 넣으면 그 경로로 답합니다.
- **공용창이 안 바뀜** → ⟳를 누르거나 20초 자동 새로고침이 켜져 있는지 확인. 커밋 직후에는 CDN 캐시 때문에 몇 초 늦을 수 있습니다.

## 7. 보안

- 저장소가 **공개**이므로 `office-messages.json`은 누구나 읽습니다. 민감한 정보(주민번호·계좌·비밀번호)는 지시에 적지 마세요.
- PAT는 localStorage에 남습니다. 공용 PC에서는 저장하지 말고, 쓰고 나면 GitHub에서 토큰을 폐기하세요. ⚙️ 맨 아래 **모두 지우기**로 이 브라우저의 값을 지울 수 있습니다.
