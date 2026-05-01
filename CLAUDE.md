# CLAUDE.md

이 프로젝트는 **AI Literacy Club** 용 슬라이드 덱을 제작하기 위한 작업 공간입니다.
모든 슬라이드는 첨부된 `sample template.pdf` 와 `design.md` 에 정의된 **다크 모드 프레젠테이션 디자인 시스템**을 엄격하게 따라야 합니다.

---

## 1. 프로젝트 목적

사용자가 제공한 샘플 템플릿(`uploads/sample template.pdf`, 30페이지, 1920×1080, 다크 배경 기반)을
재사용 가능한 **디자인 시스템**으로 정리하고, 이 시스템을 바탕으로 다양한 주제의 AI 리터러시 강의용
슬라이드를 일관된 톤 & 무드로 생산하는 것이 목표입니다.

- 출력 단위: **단일 HTML 파일 한 개 (self-contained)**
- 캔버스 크기: **1920 × 1080 (16:9)**
- 스케일링/네비게이션: `deck_stage.js` starter component 사용 (직접 구현 금지)
- 언어: 한국어 기본, 영문 병기 가능

---

## 2. 작업 시작 전 체크리스트

새 덱을 만들 때는 **반드시** 아래 순서를 따르세요.

1. `design.md` 전체를 읽고 **TYPE_SCALE**, **SPACING**, **COLORS** 토큰을 머릿속에 올려놓는다.
2. 사용자에게 다음 4가지를 확인한다 (이미 주어졌으면 스킵):
   - 발표 길이 (분) / 예상 슬라이드 수
   - 청중 (예: 사내 스터디, 외부 강연, 경영진 보고)
   - 톤 (교육형 / 설득형 / 요약 보고형)
   - 꼭 들어가야 하는 섹션·키 메시지
3. **제목 시퀀스(title sequence)** 를 먼저 쓴다. 한 가지 문법 스타일로 통일 (짧은 명사구 OR 짧은 서술문). 제목만 읽어도 흐름이 이해되어야 한다.
4. 각 슬라이드에 사용할 **레이아웃 블루프린트** (`design.md §6`) 를 미리 매핑한다.
5. `copy_starter_component({ kind: "deck_stage.js" })` 를 호출해 스캐폴드를 깐다.
6. 슬라이드를 작성한다. 한 번에 한 섹션씩, 항상 디자인 시스템 토큰만 사용.

---

## 3. 절대 규칙 (Hard Rules)

> 이 규칙들은 `lesson_learned.md` 의 누적 피드백을 반영합니다. 새 슬라이드 작성 전 lesson_learned.md 를 먼저 읽으세요.

### ⚡ 최우선 룰 — Prompter ↔ index.html 동기화 (Rule R-12)

**`index.html` 의 슬라이드 콘텐츠가 변경되면, 같은 작업 단위 안에서 반드시 `Prompter.html` 의 `scripts` 배열도 갱신한다.** 예외 없음.

| 변경 유형 | Prompter 갱신 의무 |
|---|---|
| 슬라이드 추가 | 같은 인덱스 위치에 `{ title, text }` 객체 삽입 |
| 슬라이드 삭제 | 해당 객체 제거 |
| 슬라이드 재배치 | 객체 순서 동일하게 재정렬 |
| 슬라이드 본문 수정 (도구 키워드, 톤, 핵심 메시지 변경) | `text` 필드 같이 갱신 |
| 슬라이드 `data-label` 변경 | `title` 필드 같이 갱신 |
| 슬라이드 디자인만 변경 (대본 영향 없음) | 갱신 불필요 — 단, 판단 후 명시적으로 "Prompter 변경 없음" 보고 |

**왜 강제인가**: Prompter 의 `goSlide(idx)` 는 인덱스 기반으로 BroadcastChannel + iframe postMessage 로 sync. 인덱스가 1만 어긋나도 강사가 보는 대본과 청중 슬라이드가 도미노로 어긋나 강의가 망함.

**검증 명령** (작업 종료 전 반드시 실행):
```powershell
$ix = (Select-String -Path index.html -Pattern 'data-label="').Count
$pr = (Select-String -Path Prompter.html -Pattern '^        \{ title: "').Count
"index: $ix / prompter: $pr — 같아야 함 (Cover 등 의도된 title 차이는 OK)"
```

신규 대본 톤은 `lesson_learned.md` 의 "톤 작성 가이드" 참조 (위트 + 시니컬 + `<b>` 1–2회 + 3–5문장).

### 해도 되는 것
- `design.md` 에 정의된 컬러 토큰, 타입 스케일, 스페이싱, 컴포넌트만 사용
- 강조가 필요할 때 **accent 3색** (`purple / cyan / amber`) 중 **슬라이드당 1색만** — eyebrow / 라벨 / 강조어 / 박스 모두 같은 한 색
- 실제 이미지가 없으면 **#1C1C1E 배경 + 모노톤 placeholder** 사용
- 한 덱 안에서 레이아웃 블루프린트는 **4~6개 내에서 반복** (리듬 확보)
- 보조/부속 텍스트는 `text-secondary (#8E8E93)` / `text-tertiary (#5A5A60)` 회색만 사용 (다른 accent 색으로 보조 텍스트 만들지 말 것)

### 하면 안 되는 것
- ❌ 새로운 색상 발명 (임의 hex 금지). 필요하면 `oklch()` 로 기존 팔레트 변형
- ❌ **한 슬라이드에 accent 색 2종 이상** (purple+cyan, amber+cyan 등 모두 금지)
- ❌ **이모지 / 유니코드 dingbats / emoji-rendering 문자** — 예: ✏ ✓ ✗ ★ ⚡ 등 모두 금지. ASCII (`*`, `+`, `-`, `>`) 와 화살표 텍스트(`→`, `↓`, `↑`) 만 허용
- ❌ **TYPE_SCALE 외 임의 폰트 크기.** 인라인 `font-size:` 는 `{24, 28, 40, 56, 80, 96, 160}` 6개 값만 허용. 22/26/30/32/34/36/44/48 등 금지. 가능하면 클래스 사용 (`.body`, `.caption`, `.subsection`, `.section-h`, `.title`, `.title-lg`)
- ❌ **`border-left:` 으로 좌측 accent 띠 추가** — 키워드 자체를 검색해 0건이어야 함. 강조는 (a) full border, (b) 배경 그라디언트, (c) 상단/하단 4px 띠 중 하나
- ❌ **eyebrow 에 시간 / 진행 단계 / 길이 표기** — `00:00–02:00`, `Act 1`, `Step 03` 같은 강사용 진행표 정보 금지. eyebrow 는 의미 라벨만 (예: `HANDS-ON PRACTICE`, `워크플로우 설계도`)
- ❌ **horizontal flow 에 항목 5개 이상** — `.steps` 는 3~4개 한정. 5개 이상은 수직 리스트 또는 2분할로
- ❌ `<section>` 에 직접 `position / inset / width / height` 지정 (deck-stage 가 처리)
- ❌ 그라디언트 남발, "It's not X. It's Y." 식 카피
- ❌ 슬라이드 하나에 글머리표 5개 초과
- ❌ **도구 컨텍스트 미스매치** — 직전 슬라이드가 Antigravity 라면 이 슬라이드도 Antigravity 컨텍스트(GEMINI.md 등). Claude Code 의 `.claude/` 폴더, `CLAUDE.md` 같은 도구 전용 경로/파일을 컨텍스트 확인 없이 쓰지 않는다
- ❌ **한국어 텍스트에 `var(--font-mono)` 적용** — JetBrains Mono 는 Latin 전용. 한국어가 시스템 fallback 으로 떨어져 흉한 혼합 렌더가 나온다. mono 는 파일명·경로·명령·영문 라벨에만. 한국어가 섞이면 `<span>` 으로 mono 영역만 분리 (예: `<span style="font-family:var(--font-mono)">AGENT.md</span> 생성`)
- ❌ **다이어그램을 mono 텍스트 줄로 표현** — `검색<br>↓<br>요약<br>↓<br>디자인` 식의 모노 박스는 코드 블록이지 다이어그램이 아님. 다이어그램은 **pill/박스 노드 + 화살표 connector** 로 표현
- ❌ `styles` 라는 전역 이름 사용 (컴포넌트마다 고유 이름: `titleSlideStyles`, `agendaStyles` …)
- ❌ **외부 도구·제품명·가격을 기억으로 작성** — 예: "Anthropic Claude Cowork" (존재 안 함, 실제는 Claude for Work), "Max $200/월" (실제는 $100/$200 두 단계). 제품 라인 / 가격 / 용어 정의는 슬라이드 작성 시 web search 로 1차 검증. 도구는 1년 안에 빠르게 변함. (R-17)
- ❌ **개념 슬라이드에 코드 파일명·확장자·기술 어휘 직접 노출** — `skill.md`, `agents/*.md`, "인스턴스", "페르소나에 빙의" 등은 비개발자 청중에게 막힌다. 개념 슬라이드는 메타포로 ("역할 설명서", "업무 매뉴얼", "그때만 살아있는 직원"), 도구 매뉴얼 슬라이드 (Ch3.5 실습) 에서만 코드 표현 등장. (R-19)
- ❌ **"불가능 / 안 됨 / 지원 안 함" 단정형** — 시대 흐름 6개월 안에 뒤집힐 가능성. 예: "ChatGPT 채팅창에서 병렬 불가능" → "1차 기능은 아님 (ChatGPT Tasks 같은 신기능은 흉내 가능, 직접 쓰면 고수 인정)". **현재 우세 + 예외 인정** 톤으로. 청중 중 고급 사용자도 화나게 안 한다. (R-20)
- ❌ **신기술 / 진화 흐름을 단점·비용 언급 없이 묘사** — 멀티에이전트가 단일 호출의 무조건적 진화처럼 적지 말 것. Anthropic 측정으로 멀티에이전트는 **토큰 10~15배**. 같은 슬라이드 (또는 직후) 에 비용·복잡도·실패 모드 한 줄 의무. (R-21)
- ❌ **2개 도구를 한 강의에서 다룰 때 한 번만 선언하고 끝** — 본 강의처럼 A안 (Antigravity 실습) / B안 (Claude Code 본격) 로 한 번 선언했다고 끝나지 않는다. B안 풍경 슬라이드에는 eyebrow / 캡션 / 코드 코멘트로 "B안 기준" 임을 다시 짚어준다. 청중 머릿속의 컨텍스트는 슬라이드마다 리셋된다. (R-18)
- ❌ **외국 도시·인물 가상 사례** (가능한 경우) — 청중 즉시 이해 우선. 파리(켄터키 vs 프랑스) 같은 영어권 사례는 한국 청중에게 한 박자 늦다. 가능하면 동음이의 도시 (광주광역시 vs 경기도 광주, 김포 vs 강화도, 부산 해운대 vs 강원 해운정 등) 로 교체.

### 슬라이드 검수 체크리스트 (each slide pre-flight)

```
[ ] 1. eyebrow 에 시간/속도/단계번호 없음
[ ] 2. accent 색 = 1종 (eyebrow/라벨/강조 모두 동일)
[ ] 3. `border-left:` 키워드 0건
[ ] 4. 인라인 font-size = {24, 28, 40, 56, 80, 96, 160} 중 하나
[ ] 5. 이모지/dingbats 0건
[ ] 6. horizontal flex 항목 ≤ 4개
[ ] 7. 직전 슬라이드와 도구 컨텍스트 일관
[ ] 8. 1720px content 영역(padding 100px 양쪽) 안에서 overflow 없음
[ ] 9. 한국어가 `var(--font-mono)` 안에 들어가 있지 않음 (`grep -E "font-mono.*[가-힣]"` 0건)
[ ] 10. 다이어그램은 pill 노드 + 화살표로 시각화 (mono 텍스트 줄 X)
[ ] 11. 외부 제품명·가격·기술 정의는 web search 로 검증 (R-17)
[ ] 12. 비개발자 청중 슬라이드는 코드 파일명·확장자 노출 0건 (R-19)
[ ] 13. "불가능 / 안 됨" 단정 → "1차 기능 아님 / 메인 흐름 아님" 으로 톤다운 (R-20)
[ ] 14. 신기술 / 진화 묘사 슬라이드에 단점·비용 한 줄 동반 (R-21)
[ ] 15. 2개 도구 강의면 슬라이드 단위로 어느 도구 기준인지 명시 (R-18)
```

### 구조 변경 (슬라이드 추가/삭제) 시 정합성 검증

```
[ ] A. 모든 슬라이드 footer 의 분모가 최종 슬라이드 수와 동일
[ ] B. Cover 슬라이드의 "총 X 슬라이드" meta 도 동일
[ ] C. `grep -c data-label=` 결과와 분모가 동일
[ ] D. 후반부(실습 후)는 **검증 → section divider → 티저 → 한계 → 액션 → 마무리** 6박자 (한계·액션 추가, R-23)
[ ] E. **Prompter.html `scripts` 배열 동시 갱신** — index.html 의 `data-label` 개수와 prompter `title` 개수가 1:1 매핑 (Cover 등 의도적 예외 제외). 슬라이드 인덱스 미스매치는 BroadcastChannel sync 를 깨뜨려 도미노 오정렬을 일으킴
[ ] F. **sync bridge listener 양쪽 존재 검증** — index.html 끝부분에 `window.addEventListener('message', ...)` + `new BroadcastChannel('ai_literacy_sync')` 가 있는지. 한쪽이라도 빠지면 Prompter postMessage / BC 가 무시됨
[ ] G. **개념(Ch1~3) → 실습(Ch3.5) 강의면 둘 사이에 "다리(bridge) 슬라이드" 1장** — "지금까지 배운 X 가 어디로 들어가나" 매핑. 없으면 청중은 실습 슬라이드를 새 정보로 받음 (R-22)
[ ] H. **마무리 시퀀스 = 한계+비용 → 구체 액션 아이템 → 마치며 → QnA**. "할 수 있는 것" 만 보여주고 끝내면 강사 신뢰도 ↓. 한계 슬라이드 1장 + 액션 슬라이드 1장이 retention 결정적 (R-23)
[ ] I. **N장 삽입 후 페이지 번호 갱신**: (1) 분모 일괄 변경 (`replace_all`), (2) 영향받는 분자만 **역순 시프트** (가장 높은 번호부터), (3) 중복 케이스는 다음 섹션 코멘트나 `data-label` 을 anchor 로 disambiguate. 정방향 시프트 시 충돌 발생 (R-24)
```

---

## 4. 파일 구조 (권장)

```
/
├─ CLAUDE.md              ← 이 파일 (작업 지침)
├─ design.md              ← 디자인 시스템 전체 정의
├─ deck-stage.js          ← starter component (copy_starter_component 로 생성)
├─ <주제명>.html           ← 실제 덱 파일 (예: "AI Literacy 101.html")
├─ components/
│   ├─ tokens.js          ← TYPE_SCALE / SPACING / COLORS 객체 (JS export)
│   ├─ Slide.jsx          ← 공통 슬라이드 프레임 (padding, 배경)
│   ├─ layouts/
│   │   ├─ TitleSlide.jsx
│   │   ├─ SectionDivider.jsx
│   │   ├─ SplitProfile.jsx
│   │   ├─ AgendaList.jsx
│   │   ├─ WorkflowSteps.jsx
│   │   ├─ ComparisonBeforeAfter.jsx
│   │   ├─ BigNumber.jsx
│   │   ├─ Quote.jsx
│   │   └─ BarChart.jsx
│   └─ primitives/
│       ├─ ImageFrame.jsx
│       ├─ CircularThumb.jsx
│       └─ ArrowIcon.jsx
└─ uploads/                ← 원본 참고 자료
```

> 덱이 하나뿐이고 레이아웃이 3~4개면 굳이 컴포넌트를 파일로 쪼개지 않고 한 HTML 안에 인라인 JSX 로 둬도 됩니다. 하지만 6개 이상이 되면 위 구조로 분리하세요.

---

## 5. 톤 & 카피라이팅

- **제목은 챕터 제목처럼**: "AI 는 어떻게 학습하는가" (O) / "충격! AI 의 비밀 공개" (X)
- **서술문보다 명사구 선호** (한국어 기준): "모델의 한계" (O) / "모델에는 한계가 있습니다" (△)
- **숫자와 고유명사를 아끼지 말 것**. 추상적 형용사("혁신적인", "강력한")는 빼기
- **번역투 금지**: "~에 대한", "~을 가지고 있다" 는 한국어답게 고치기
- 슬라이드당 **한 가지 메시지**. 두 개면 슬라이드를 쪼갠다

---

## 6. 시각적 다양성 체크리스트

한 덱을 완성했을 때 아래 중 **최소 6종** 이 섞여 있어야 합니다.

- [ ] 타이틀 슬라이드 (브랜드 플레이)
- [ ] 섹션 구분 슬라이드 (큰 숫자 + 섹션명)
- [ ] 2분할 (이미지 + 텍스트)
- [ ] 3~4 스텝 워크플로우
- [ ] Before / After 비교
- [ ] 큰 숫자 강조 (BigNumber)
- [ ] 인용 (Quote)
- [ ] 데이터 차트 (Bar / 간단한 도표)
- [ ] 아젠다 / 리스트
- [ ] 전면 이미지 + 캡션

같은 레이아웃을 **3번 이상 연속**해서 쓰지 마세요. 리듬이 죽습니다.

---

## 7. 검수(Verification) 가이드

`done` 호출 전 스스로 체크:

1. 모든 제목이 같은 문법 스타일인가?
2. 슬라이드마다 `padding` 이 `SPACING.paddingX=100` 이상인가?
3. 본문 텍스트가 28px 이상인가?
4. accent 색이 한 슬라이드에 2개 이상 들어가 있지는 않은가?
5. eyebrow 에 시간 / 단계 번호가 없는가?
6. 코드에서 `border-left:` 검색 시 0건인가?
7. 인라인 `font-size:` 가 모두 {24, 28, 40, 56, 80, 96, 160} 중 하나인가?
8. 이모지 / 유니코드 dingbats(✏ ✓ ✗ 등)가 없는가?
9. 웹 느낌(카드 그림자 남발, rounded-2xl 만 사용 등)이 아니라 **프레젠테이션 느낌**인가?
10. 빈 공간이 두렵지 않은가? (아래쪽 여백은 의도된 것 — 채우지 마세요)
11. 도구 컨텍스트가 직전 슬라이드와 일관 (Claude Code / Antigravity / Gemini)?

그 다음 `done(path)` → `fork_verifier_agent()` 순서로 마무리합니다.

**작업 후 lesson_learned.md 업데이트:** 사용자가 새 피드백을 주면 즉시 lesson_learned.md 에 기록하고, 룰화할 만하면 본 §3 절대 규칙에 반영합니다.

---

## 8. 확장(Extend) 정책

사용자가 "이 템플릿으로 X 주제 덱을 만들어줘" 라고 하면:
- **새 HTML 파일을 만들되**, `design.md` 의 토큰/컴포넌트를 그대로 import/복제해 사용
- 디자인 시스템 변경은 `design.md` 에 **먼저** 반영하고, 그 뒤 덱에 적용
- 새 레이아웃이 필요하면 `design.md §6` 에 블루프린트를 추가한 뒤 구현
