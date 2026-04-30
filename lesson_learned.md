# lesson_learned.md — 사용자 피드백 누적 기록

> 슬라이드 덱 작업 중 사용자가 반복적으로 지적한 사항과 디자인 시스템 위반 사례.
> 이 파일은 **CLAUDE.md / design.md 업데이트의 근거 자료**이며, 새 작업을 시작하기 전 반드시 검토한다.

---

## 운영 원칙

1. 사용자가 두 번 이상 같은 피드백을 주면 → 이 파일에 즉시 기록
2. 기록과 동시에 CLAUDE.md / design.md 의 해당 섹션을 강화
3. 새 슬라이드 작업 시작 전 이 파일을 먼저 읽고 체크리스트로 사용

---

## 2026-04-30 — 실습 슬라이드(62–69) 1차 작업 → 재작업

### A. 사용자 피드백 (받은 즉시 기록)

| # | 피드백 | 빈도 |
|---|---|---|
| 1 | "eyebrow에 00:00–02:00 같은 시간 쓰지마" | 1회 (지속 룰화) |
| 2 | "기존 슬라이드들의 열화판처럼 보임. 퀄리티 끌어올려" | 1회 |
| 3 | "67p가 완전히 깨져 보임" | 1회 |
| 4 | "폰트가 잘못 사용된 것 없는지 확인" | 1회 |
| 5 | "71p gemini.md가 claude 폴더에 있는게 어색함" (도구 컨텍스트 미스매치) | 1회 |
| 6 | "claude.md / design.md 를 제대로 따르지 않음" | 1회 |

### B. 1차 작업본의 디자인 시스템 위반 (자체 진단)

#### B-1. 한 슬라이드에 accent 색 2개 이상
- 62p: cyan eyebrow + amber 본문
- 64p: purple GEMINI.md + cyan AGENT/SKILL.md
- 66p: amber 단계 번호 + cyan 파일명 + purple GEMINI.md (**3색**)
- 69p: purple eyebrow + amber 번호 + cyan 파일명 (**3색**)
- 70+ 기존 슬라이드 71p에도 동일 위반(purple/cyan/amber 동시 사용) 잔존

→ 룰: **한 슬라이드 = accent 1색.** eyebrow, 라벨, 강조어, 박스 모두 같은 색. 보조는 `text-secondary` / `text-tertiary` (회색).

#### B-2. 좌측 accent border 카드 (`border-left: Npx solid accent`)
- 1차본 66p, 68p, 69p에서 사용
- design.md §11 에 이미 ❌ 로 명시되어 있음에도 무의식적으로 추가

→ 룰 강화: **`border-left:` 키워드 자체를 코드에서 검색해 0건이어야 한다.** 강조는 (a) full border, (b) 배경 그라디언트, (c) 아래/위 4px 띠 중 하나로.

#### B-3. TYPE_SCALE 외 임의 폰트 크기
- 22, 26, 30, 32, 34, 36, 44, 48 px 다수 사용 (1차본)
- 허용된 토큰 값: **24 / 28 / 40 / 56 / 80 / 96 / 160 px**

→ 룰 강화: 인라인 `font-size:` 작성 시 위 6개 값만 허용. 클래스(`.body`, `.caption`, `.subsection`, `.section-h`, `.title`, `.title-lg`)를 우선 사용.

#### B-4. 유니코드 심볼/이모지
- ✏ (U+270F PENCIL) 사용 — design.md "이모지 금지" 위반
- ✓ ✗ 같은 dingbats 도 동일 범주

→ 룰 명확화: **유니코드 dingbats / emoji-rendering 문자 = 이모지.** ASCII (`*`, `+`, `>`, `→`, `↓`, `↑`) 만 허용. 화살표는 텍스트의 일부로만 사용, 장식 X.

#### B-5. Horizontal flow overflow
- 1차본 67p: 5개 박스 + 4개 화살표 horizontal flex → 1720px 콘텐츠 영역 초과 → 깨짐

→ 룰 추가: **`.steps` 는 3–4개 항목 한정.** 5개 이상이면 (a) `.bullets` 수직, (b) 좌우 분할 + 카운트 다운으로.

#### B-6. eyebrow 타임스탬프
- 1차본 모든 새 슬라이드의 eyebrow에 `00:00–02:00` 등 강사 진행표 표기
- 청중 슬라이드에 노출되면 안 됨

→ 룰 추가: **eyebrow 는 의미 라벨만** (예: `HANDS-ON PRACTICE`, `실습 ①`). 시간/길이/속도 표기 금지.

#### B-7. 도구 컨텍스트 미스매치
- 71p가 Claude Code의 `.claude/` 폴더 구조를 보여줌
- 직전 실습은 Antigravity (Gemini 기반) → 컨텍스트 충돌
- `CLAUDE.md` 도 같은 사유로 `GEMINI.md` 로 교체했었음

→ 룰 추가: **새 슬라이드 작성 시 직전 슬라이드의 도구 컨텍스트(Claude Code / Antigravity / ChatGPT 등)를 확인.** 도구별 경로·파일·키워드를 체크리스트로 검증.

---

### C. 작업 검수 체크리스트 (slide-pre-flight)

새 슬라이드를 작성할 때 **각 슬라이드마다** 아래를 검증:

```
[ ] 1. eyebrow 에 시간/속도/길이 없음
[ ] 2. 슬라이드 내 accent 색 = 1종 (eyebrow/라벨/강조 모두 동일)
[ ] 3. `border-left:` 키워드 0건
[ ] 4. font-size 인라인 = {24, 28, 40, 56, 80, 96, 160} 중 하나
[ ] 5. 이모지/유니코드 dingbats 0건
[ ] 6. horizontal flex 항목 ≤ 4개
[ ] 7. 직전 슬라이드와 도구 컨텍스트 일관 (Antigravity/Claude Code 등)
[ ] 8. 1920×1080 안에서 overflow 없음 (특히 padding 100px 양쪽 = 1720px content)
```

---

---

## 2026-04-30 — 후반부 플로우 재구성 (Option B)

### A. 사용자 피드백

| # | 피드백 | 카테고리 |
|---|---|---|
| 7 | "전반적으로 퀄리티를 다시 끌어올리고" | 품질 일관성 |
| 8 | "후반부를 살리면서 자연스럽게 마무리할 수 있는 방법을 고민하자" | 구조 개선 (호흡 / 리듬) |
| 9 | "변경점·반복 수정 요청을 lesson_learned.md 로 누적하면서 CLAUDE.md / design.md 에 반영" | 거버넌스 |

### B. 구조 변경 사항

**Before** (12 슬라이드):
70 Full Map → 71 Folder → 72 Skills vs Context → 73 Hooks → 74 Subagents vs Teams → 75 Token Cost → 76 Running Order → 77 AG vs CC → 78 Series Summary → 79 Final Close → 80 Q&A → 81 End

**After** (9 슬라이드):
70 Full Map → 71 Folder → **72 Section Divider** "조감도" → **73 4-카드 티저** "다음 단계의 풍경" → 74 Running Order → 75 Series Summary → 76 Final Close → 77 Q&A → 78 End

- 5장 삭제: Skills vs Context, Hooks, Subagents vs Teams, Token Cost, Antigravity vs Claude Code
- 2장 신규: Section Divider (purple), 4-Card Teaser (purple)
- 5장의 깊이 있는 비교 슬라이드는 4-카드 티저에 한 줄씩 보존 → "다음 챕터" 입구로 위치 변경

### C. 추가 룰 (이번에 발견)

#### Rule R-9: 슬라이드 카운트 정합성
- 1차 작업 시작 시점에 페이지 번호 분모가 제멋대로였음:
  - 1–61 슬라이드: `/ 73` (원본 73장 시절 잔재)
  - 62–69 슬라이드: `/ 81` (실습 추가 후 의도)
  - 70+ 슬라이드: `/ 73` (실습 추가 시 갱신 누락)
- 구조 변경 시 **반드시 모든 분모를 최종 슬라이드 수와 일치시킨다**
- Cover slide 의 "총 X 슬라이드" meta 도 동일하게 갱신 (`총 4개 챕터 · 78 슬라이드`)

→ design.md / CLAUDE.md 에 반영: "구조 변경 후 slide count 정합성 검증" 단계 추가

#### Rule R-10: 실습 직후 리듬
- 실습 30분 → 후반 10분 으로 끝내려면 후반 슬라이드 8장 이내
- 실습 → **즉시 검증 슬라이드** (방금 한 것의 큰 그림) → **section divider** → **티저 1장** → **마무리 시퀀스**
- 후반에 깊은 비교/논증 슬라이드를 넣으면 강의가 "두 번째 강의"처럼 느껴짐 → 다음 시즌으로 미루기

→ design.md §6 에 "post-practice rhythm" 블루프린트 추가 후보

### D. 결과물

- index.html: 81 → 78 슬라이드
- 모든 page 분모 `/ 78` 로 통일 (60건의 `/ 73` 포함 자동 정렬)
- Cover meta `총 4개 챕터 · 78 슬라이드`
- 새 룰 2종 (R-9, R-10) 후보 — 다음 작업에서 사용 시 검증 후 CLAUDE.md / design.md 반영

---

---

## 2026-04-30 — 폰트 폴백 이슈 (62/63/68p)

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 10 | "62, 63 페이지 다이어그램과 폰트가 어색한데, 다시 한번 확인. 68p도 그런 폰트가 있네." |

### B. 진단 — Rule R-11 신설

#### Rule R-11: 한국어 텍스트에 `var(--font-mono)` 적용 금지

**증상**: 한국어가 모노스페이스 박스 안에서 흉하게 렌더됨. 가독성 저하 + 위엄 없음.

**원인**:
- `--font-mono: "JetBrains Mono", ui-monospace, monospace`
- JetBrains Mono 는 **Latin 전용** 코드 폰트. 한국어 글리프 없음
- 결과: 한국어가 시스템 fallback (Consolas 한글, NanumGothicCoding 등)으로 떨어짐
- 영문 mono 의 폭 + 한국어 fallback 의 다른 폭/굵기 → **혼합 렌더 부조화**

**룰**:
- mono 는 다음 경우에만 허용:
  - 파일명 / 경로: `GEMINI.md`, `my-ai-team/`, `.docx`
  - 명령 / 코드 스니펫: `git commit`, `npm install`
  - 영문 라벨 / 헤더: `FILE EXPLORER`, `Option A`, `01`, `02`
  - 영문 약어 / 상수: `REQUEST`, `RESPONSE`
- 한국어 텍스트(다이어그램 노드, 본문, 인용문 등)는 **항상** `var(--font-sans)` (Pretendard 스택)
- **혼합 inline은 `<span>` 으로 분리**: 예) `<span style="font-family:var(--font-mono);">AGENT.md</span> 생성` — 파일명만 mono, 동사는 sans

**검증 패턴 (코드 검색)**:
```
grep -E "font-mono.*[가-힣]|[가-힣].*font-mono" *.html
```
한 글자라도 한국어가 mono 컨텍스트 안에 있으면 위반.

### C. 이번 작업 수정 내역

- 62p 좌측 chain "검색/요약/디자인/저장" → mono 제거, sans pill 박스로 재구성
- 62p 우측 "주제 입력 한 줄" → mono 제거, sans amber pill
- 63p 좌·우측 비교 다이어그램 → mono 텍스트 chain 제거, sans pill+arrow 다이어그램으로 시각화 강화
- 64p `↓ 참조` 의 mono 제거 (한국어 단어)
- 68p 명령 인용문 `"이번 주 AI 업계 동향으로 카드뉴스 만들어줘"` → mono 제거, sans + 큰따옴표 그래픽

### D. 구조적 부수 개선 (퀄리티)

기존 1차본 62/63 은 "텍스트 행 4–5줄 mono 박스" 라서 **다이어그램이 아니라 코드 블록처럼 보였음**. 재작업 후:
- 62p: pill + 화살표 + 결과 박스 → 흐름이 시각적으로 명확
- 63p: pill 노드 + 가로 chain → "워크플로우" 라는 단어가 시각적으로 표현됨
- 68p: 인용문 따옴표 그래픽 → "발화 한 줄" 임이 분명

→ 다이어그램 슬라이드는 **텍스트 그대로 늘어놓지 말고 노드(pill/박스) + 연결자(화살표)로** 표현해야 다이어그램다워진다.

→ design.md §6 에 "DiagramFlow" 블루프린트 추가 후보 (pill node + arrow connector 패턴).

---

---

## 2026-04-30 — Prompter.html 동기화 (78 슬라이드)

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 11 | "Prompter.html 은 강의자료 과거 버전 기반 텔레프롬프터. 현재 버전(78 슬라이드)에 맞게 변경, 톤앤매너 유지." |

### B. Rule R-12 — Prompter scripts 동기화

**룰**: `index.html` 의 슬라이드 추가/삭제/재배치가 일어나면 `Prompter.html` 의 인라인 `scripts` 배열도 **같은 커밋 안에서** 갱신한다. 두 파일은 항상 1:1 매핑을 유지해야 한다.

**검증 명령**:
```powershell
$ix = (Get-Content index.html | Select-String -Pattern 'data-label="([^"]+)"' -AllMatches).Matches.Count
$pr = (Get-Content Prompter.html | Select-String -Pattern '^        \{ title: "([^"]+)"' -AllMatches).Matches.Count
"index: $ix / prompter: $pr"  # 두 값이 같아야 함
```

**Why**: `Prompter.html` 의 `goSlide(idx)` 는 인덱스 기반으로 슬라이드와 동기화 (BroadcastChannel `ai_literacy_sync`). 인덱스가 1개만 어긋나도 강사가 보는 대본과 청중이 보는 슬라이드가 도미노로 어긋남.

**How to apply**:
- 슬라이드 추가 → 같은 위치에 `{ title, text }` 객체 삽입
- 슬라이드 삭제 → 해당 객체 제거
- 슬라이드 재배치 → 객체 순서 재배치
- `title` 은 슬라이드 `data-label` 과 동일하게 (Cover 같은 예외만 강사용 더 설명적)
- 신규 대본은 기존 톤(위트·메타포·`<b>` 강조) 유지

### C. 이번 작업 변경 내역

- 73 → 78 entries (net +5)
- 신규 10 대본 작성: Acts 1–7 (62–69), Take a Step Back (72), Next Steps Preview (73)
- 키워드 패치 3 대본: Full Map (70), Folder Structure (71), Running Order (74) — `CLAUDE.md`/`.claude/` → `GEMINI.md`/`project/`
- 삭제 5 entries: Skills vs Context, Hooks, Subagents vs Teams, Token Cost, AG vs CC
- 무변경 60 entries (1–61, 75–78)
- 인라인 주석 `// 73페이지 대본 완벽 정렬 하드코딩` → `// 78페이지 대본 정렬`

### D. 톤 작성 가이드 (재확인)

기존 강사 톤 패턴:
- 위트 + 살짝 시니컬한 도발 (예: "귀찮으면 그냥 야근하시면 됩니다", "그런 직원은 현실에도 없고, 있어도 곧 퇴사합니다")
- 메타포 일관: 지휘자 / 신입사원 / 망치 / 회사·조직 비유
- `<b>...</b>` 1–2회로 핵심 강조 (절대 더 늘리지 않음)
- 슬라이드당 3–5문장, 약 100–150자
- 결론은 한 줄로 명확히 끊기 (예: "이게 지휘자의 기본입니다.", "다음 시즌에서요.")

→ 이 패턴은 design.md 영역이 아니므로 별도 가이드로 lesson_learned 에만 보관.

---

---

## 2026-05-01 — Prompter ↔ index.html 동기화 단절 (사이드바 iframe & BC 양방향 모두 미작동)

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 12 | "연동된 슬라이드(sync)가 제대로 작동 안 함. Prompter는 03 Ch1 TOC인데 iframe은 Cover 그대로." |

### B. 진단 — Rule R-13 신설

**증상**: Prompter가 슬라이드 인덱스를 진행해도 사이드바 iframe(index.html)이 그대로. 별도 창으로 띄워도 동기화 안 됨.

**원인**:
- Prompter는 두 가지 방식으로 sync 시도:
  1. `slideFrame.contentWindow.postMessage({slide:idx}, '*')` — iframe 사이드바용
  2. `BroadcastChannel('ai_literacy_sync').postMessage({slide:idx})` — 별도 창용
- **둘 다 받는 쪽(index.html / deck-stage.js)에 listener 가 없었음**
- deck-stage.js 는 자기 window에 `{slideIndexChanged:idx}` 만 post 할 뿐, 외부와 통신하지 않음
- 결과: Prompter 가 보내는 명령이 허공으로 사라지고, deck-stage 의 navigation 변화도 외부로 새지 않음

### C. Rule R-13: 멀티-창 sync 의 양쪽 listener 필수

웹 컴포넌트(deck-stage)와 외부 컨트롤러(Prompter) 사이의 sync 는 **단방향이라도 두 쪽 모두 listener 가 있어야** 동작:
- 컨트롤러 → 컴포넌트: 컴포넌트에 `message` 이벤트 리스너 + `goTo()` 호출
- 컴포넌트 → 컨트롤러: 컴포넌트가 `parent.postMessage` 또는 BroadcastChannel 로 변경 사실 외부 송출

**postMessage vs BroadcastChannel 선택 기준**:
- iframe 임베드 (한 창 안): `parent.postMessage` 또는 `iframe.contentWindow.postMessage`
- 별도 창 (멀티 모니터): `BroadcastChannel('채널명')` 양쪽에 인스턴스
- 두 시나리오 모두 지원해야 한다면 **둘 다 구현** (Prompter 가 양쪽 다 보내고 있으므로 컴포넌트 쪽에서도 양쪽 다 받아야 함)

### D. 수정 내역

`index.html` 끝에 sync bridge `<script>` 추가 (deck-stage.js 로드 직후):
- `customElements.whenDefined('deck-stage')` 대기 후 stage 핸들 확보
- window message 리스너:
  - `{slideIndexChanged:n}` 수신 → `parent.postMessage` + `bc.postMessage` 로 외부 전파
  - `{slide:n}` 수신 → `stage.goTo(n)` (Prompter → 컴포넌트)
- BroadcastChannel `ai_literacy_sync` 리스너:
  - `{slide:n}` → `stage.goTo(n)`
  - `{type:'CHECK_CONNECT'}` → `{type:'CONNECT_ACK'}` 회신 (Prompter UI "LIVE SYNC ACTIVE" 표시 트리거)
- 초기 로드 시 `forwardOut(stage.index)` 로 현재 위치 announce

**무한 루프 방지**: `_go(i)` 에서 `clamped === this._index` 시 early return (deck-stage.js:597–600). 외부 echo 가 같은 인덱스를 다시 보내도 `goTo` 가 no-op 이라 안전.

### E. lesson 업데이트

→ CLAUDE.md §3 "구조 변경 정합성 검증" 의 E 항목(Prompter 동기화 검증)에 **listener 양쪽 존재 검증** 추가:
- `grep "addEventListener.*message" index.html` 1건 이상
- `grep "BroadcastChannel" index.html` 1건 이상

---

---

## 2026-05-01 — 32p/33p 예시 강화 + mono 글로벌 스캔

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 13 | "32p 회사 컨텍스트 추가에 예시 넣어 명확히. 33p에 내 회사 예시(재건축 B2C, 시니컬 위트) 옆에 보여주자. mono font 더블체크." |

### B. 변경 사항

**32p (Company Detail)**:
- 우측 "회사 컨텍스트 추가" 컬럼이 추상 텍스트("사업 분야, 타깃 고객…")만 있었음 → 4행 구체 예시(사업/타깃/경쟁사/KPI 각 1줄)로 교체
- 좌·우 결과 비교 chat-bubble 추가 (컨텍스트 없이 vs 있을 때의 출력 1줄씩)
- `class="mono"` 제거 → sans 박스로. `border-left:3px solid` 자동 제거 (`.ba-col .mono` CSS 규칙에서 옴)

**33p (Context Template)**:
- 단일 column 템플릿 → 2-column (템플릿 / 내 회사 채워넣기)
- `class="template"` 제거 → 자체 스타일링. `font-family:var(--font-mono)` 자동 제거
- 시니컬 위트 톤 예시: 재건축 B2C 부서 / 조합원·정부 / "내 집값" / "조합 총회에서 잡아먹힘" / "수주 망함, 우리도 망함"

### C. 글로벌 mono+한국어 스캔 결과

여전히 잔존하는 위반(이번 작업 외 슬라이드, R-11 위반):

| 라인 | 슬라이드 | 내용 |
|---|---|---|
| 676 | Slide 18 (Template — Ch1) | `<div class="template">` Korean 슬롯 텍스트 mono 렌더 |
| 839, 844 | Slide 29 (Restaurant Analogy) | `class="mono"` "4명, 7시 예약이요." / 70대 부모님 긴 문장. **border-left:3px solid 까지 동반** |
| 1127–1131, 1137 | Slide 44 (Workflow Comparison) | "경쟁사 A 조사해줘" 등 5건 + 임원 보고서 prompt block 모두 inline mono |

→ 사용자 결정 필요: 일괄 정리할지 / 다음 사용자 요청 때 함께할지.

### D. 발견된 패턴 룰

CSS 클래스 자체에 `font-family:var(--font-mono)` 와 `border-left:Npx solid` 가 묶여 있으면, **그 클래스를 호출한 곳마다 자동으로 두 가지 위반이 동시에 발생**한다 (`.ba-col .mono`, `.template`).

→ Rule R-14 후보: design system 의 클래스 정의 단계에서 `font-mono`+`border-left` 조합 자체를 제거. 호환성 위해 클래스는 남겨두되 정의를 sans+full-border 로 교체하면, 호출부 일괄 변환 없이 디자인이 정렬됨.

→ 다음 design.md 업데이트 후보: `.ba-col .mono` 와 `.template` CSS 정의를 토큰 준수 형태로 리팩터.

### F. 후속 일괄 정리 (사용자 승인 후 진행)

세 슬라이드 + CSS 정의 동시 처리:

| 위치 | 변경 |
|---|---|
| Slide 18 (Template — Ch1, line 676) | `<div class="template">` → 인라인 sans 스타일 (28px line-height 1.85, purple slot, 큰따옴표 그래픽) |
| Slide 29 (Restaurant Analogy, lines 839, 844) | `<div class="mono">` 두 개 → sans chat-bubble (32p와 동일 패턴) |
| Slide 44 (Workflow Comparison, line 1137) | inline `font-family:var(--font-mono)` 제거, 28px sans chat-bubble |
| CSS `.ba-col .mono`, `.ba-col.after .mono`, `.template`, `.template .slot`, `.template .quote-mark` | **정의 자체 삭제** (호출부 0건이라 안전) |

### G. 최종 검증

- `grep -oE 'font-mono[^>]*>[^<]*[가-힣]'` = **0건** (단일 element 안에서 mono+한국어 조합 0건)
- `grep -E "border-left:\s*[3-9]px\s+solid"` = 0건
- 78 슬라이드 데이터-라벨 카운트 유지
- false positive 잔존 패턴(허용): `<span style="font-mono">파일명</span> 한국어` — 정상적인 Latin-only mono 사용

→ Rule R-11 (mono+한국어 금지) 전면 준수 달성.

→ Rule R-14 신설: **CSS 클래스 정의가 위반(font-mono+border-left+border-left:3px+ 등)을 포함하면, 호출부를 일괄 변환할 게 아니라 CSS 정의 자체를 제거/리팩터**. 호출부 손 안 대고 디자인 시스템 정렬 가능.

---

---

## 2026-05-01 — 47/48 (Claude Code Skills, Agent Teams) 삭제 + Prompter 동기화

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 14 | "47, 48이 어색. 에이전트 개념 설명 중에 갑자기 Claude Code 도구 + Agent Teams 실험 기능 등장. 위치 변경 또는 삭제 고려." |
| 15 | "Option A로 가고, Prompter도 동기화. 앞으로 슬라이드 변동 시 Prompter 동기화를 CLAUDE.md 룰로 강제." |

### B. 변경 사항

**index.html**:
- Slide 47 (Claude Code Skills) 삭제
- Slide 48 (Agent Teams) 삭제
- 페이지 49–78 → 47–76 으로 리넘버링 (총 30 슬라이드 footer 갱신)
- 페이지 1–46 → 분모 `/ 78` → `/ 76`
- Cover meta `78 슬라이드` → `76 슬라이드`
- 총 슬라이드: 78 → **76**

**Prompter.html**:
- `{ title: "Claude Code Skills", ... }` 삭제
- `{ title: "Agent Teams", ... }` 삭제
- 인라인 주석 `// 78페이지 대본 정렬` → `// 76페이지 대본 정렬`
- 총 scripts: 78 → **76**

**1:1 매핑 검증**: 76 = 76 ✓ (Cover 1건만 의도된 title 차이)

### C. 흐름 개선 효과

Before:
```
46. Agent Reality (개념 클라이맥스)
47. Claude Code Skills ← 갑자기 도구
48. Agent Teams ← 도구 실험 기능 깊게
49. Prompt Injection (다시 개념)
```

After:
```
46. Agent Reality (개념 클라이맥스)
47. Prompt Injection (자연스럽게 위험 주제로 전환)
```

→ Ch3 가 끝까지 "에이전트 개념" 챕터로 일관. 도구 얘기는 Ch4 (Antigravity 실습) 와 73p 티저 (다음 단계 풍경) 에서 보존.

### D. Rule R-12 강화 (CLAUDE.md §3 최상단)

기존: §3 체크리스트의 한 항목 (E, F)
변경: **§3 최상단 "⚡ 최우선 룰" 으로 분리**, 표 형식으로 변경 유형별 의무 명시

| 변경 유형 | Prompter 갱신 의무 |
|---|---|
| 추가/삭제/재배치 | 객체 동일 작업 |
| 본문 수정 (키워드·톤·핵심 메시지) | `text` 필드 갱신 |
| `data-label` 변경 | `title` 필드 갱신 |
| 디자인만 변경 (대본 무영향) | 불필요, 단 명시 보고 |

**Why 강조**: index.html 만 수정하고 Prompter 를 깜빡하면 강의 당일 도미노 오정렬 발생. 작업 종료 전 검증 명령 필수.

---

---

## 2026-05-01 — 56p 준비물 슬라이드 사실관계 수정 + 선택 근거 재프레이밍

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 16 | "56p Antigravity 설명 잘못됨. 브라우저 아니고 데스크톱 앱. Claude Code도 터미널만 아님. 둘의 진짜 차이는 Claude Code가 유료 멤버십 필수, Antigravity는 무료라 실습용. Antigravity를 A안으로." |

### B. 사실 정정 (웹 리서치 후)

| 항목 | 기존 슬라이드 (오류) | 정정 사실 |
|---|---|---|
| Antigravity 형태 | "브라우저에서 바로 실행" | **macOS / Windows / Linux 데스크톱 앱** (antigravity.google 다운로드) |
| Antigravity URL | `antigravity.google.com` | `antigravity.google` (`.com` 없음) |
| Antigravity 출시 | 명시 없음 | 2025년 11월 무료 퍼블릭 프리뷰 |
| Antigravity 모델 | 명시 없음 | Gemini 3 Pro · Claude Sonnet 4.5 · OpenAI 다 지원 |
| Claude Code 형태 | "터미널 기반" | **CLI · VSCode 확장 · IDE 플러그인 · 웹** 다양 |
| Claude Code 비용 | 명시 없음 | **Pro $20/월 또는 Max $200/월 구독 필수** |

### C. 선택 근거 재프레이밍

기존: 사용자 수준 기반 ("터미널 좀 다루면 A, 비개발자면 B")
변경: **카드값 기반** (실제 강의 결정 이유)
- A안 = Antigravity = 무료 = 깔자마자 실습 가능 = 오늘 사용
- B안 = Claude Code = 유료 구독 입장료 = 본격 도입할 때

타이틀: "성능은 Claude, 오늘은 Antigravity."
캡션: "한쪽은 무료라 깔자마자 됩니다. 다른 한쪽은 카드부터 꺼내야 합니다."

### D. 디자인 시스템 정리 부수 작업

기존 슬라이드는 Claude Code(purple) + Antigravity(cyan) 두 accent 동시 사용 → R-1 (single accent) 위반.
재설계: A안 cyan accent + cyan-10 gradient + cyan border, B안 plain bg-surface + gray accent. **단일 accent (cyan) 만** 사용.

검증: 슬라이드 56 안에서 cyan=7, purple=0, amber=0 ✓

### E. Prompter 동기화 (Rule R-12 적용 사례)

`{ title: "준비물", text: ... }` 의 `text` 필드 전면 갱신:
- 사실 정정 반영 (데스크톱 앱, 무료, $20/월 구독 등)
- 톤 유지 (위트 + "카드값 차이로 오늘의 무기를 정한 겁니다" 같은 시니컬 마무리)
- `<b>` 강조 2회 사용 (A안, B안)

### F. Rule R-15 후보 — 외부 도구·서비스 정보는 리서치 후 작성

신규 도구·서비스(Google Antigravity, Claude Code 등) 슬라이드 작성 시:
1. 출시 1년 미만이면 **반드시 web search** 로 최신 사실 확인
2. 다음 정보를 명시: 형태(데스크톱/웹/CLI), 비용, 필수 계정, 지원 OS, 지원 모델
3. 강의 컨텍스트의 선택 근거를 솔직하게 (예: "성능 vs 가격 — 실습은 가격 우선")
4. 추측·예전 정보 사용 금지 (도구 빠르게 변함)

→ design.md / CLAUDE.md 후보 (다음 외부 도구 슬라이드 작성 시 적용)

---

---

## 2026-05-01 — 57p 준비물 압축 (에이전트 자율 부트스트랩 활용)

### A. 사용자 피드백 / 인사이트

| # | 피드백 |
|---|---|
| 17 | "에이전트가 npm install 같은 걸로 알아서 설치할 수 있지 않나?" — 사용자가 슬라이드의 prereq 과다 안내에 의문 제기 |
| 18 | "의견 수용. Antigravity는 미리 설치 안내 완료." |

### B. 진단

기존 57p는 Node.js + Python + 브라우저 확장 3종 사전 설치를 강제 → **방어적 과다 설치**:
- Antigravity 데스크톱 앱은 자체 Node.js 런타임 번들링
- 이 워크숍 실습(Acts 4–6)에서 사용자가 손으로 nodejs/python 명령을 칠 일 없음
- 필요 시 에이전트가 winget/brew/installer 다운로드로 자체 처리 가능
- 닭-달걀 한계는 Antigravity 본체뿐 (이미 사전 설치 안내됨)

→ Rule R-15 부수 발견: **외부 도구 prereq 슬라이드는 "도구 자체"와 "워크플로우 의존성"을 구분해야 함**. 워크플로우 의존성은 에이전트가 처리할 수 있으면 사전 설치 강제하지 말 것.

### C. 변경 사항

**Slide 57 (Antigravity Prerequisites)**:
- Title: "일꾼들이 일할 책상부터 깔아야 합니다." → "오늘 준비물, 한 가지면 됩니다."
- Eyebrow: amber `PRE-REQUISITES` → cyan `PRE-FLIGHT`
- 3개 항목:
  1. (구) Node.js 사전 설치 → (신) Antigravity 설치 완료 확인
  2. (구) Python 사전 설치 → (신) 나머지는 에이전트가 챙깁니다
  3. (구) 브라우저 확장 → (신) Python · Git 은 오늘 불필요 (미리 깔지 마라)
- Single accent: cyan only (이전 amber 단일이었으므로 전환만)

**Prompter.html "Antigravity Prerequisites"** 동기 갱신 (Rule R-12):
- 구 톤: "체크박스 안 누르면 에이전트가 파업"
- 신 톤: "빈손으로 와서 에이전트한테 일 시키는 게 오늘의 미덕"

검증: 76 = 76 mapping ✓, slide 57 accent: cyan=4 / purple=0 / amber=0 ✓

### D. 부수 학습 포인트

이 변경 자체가 강의 메시지 강화:
- "AI에게 일을 시킨다"는 컨셉을 **수업 시작 직전부터** 학습 포인트로 활용
- 에이전트가 자기 도구를 챙기는 모습을 학생이 직접 봄
- 사전 설치 스트레스 ↓, 운영 부담 ↓, 강의 핵심 메시지 ↑

### E. Rule R-15 보강

기존 R-15: "외부 도구 정보는 web search 후 작성"
보강: **"필수 prereq" 와 "선택 prereq" 를 구분. 에이전트가 자체 처리 가능한 의존성은 선택으로 격하.** 30분 워크숍 시작 직전 사전 설치 부담을 최소화하는 것이 학습 효과 우선.

---

---

## 2026-05-01 — 64p / 65p 워크숍 메커니즘 정정 (생성 → 다운로드+드래그)

### A. 사용자 피드백

| # | 피드백 |
|---|---|
| 19 | "64p는 노션에서 만드는 게 아님. 노션은 강사가 .md 파일을 미리 올려둔 다운로드 링크 페이지일 뿐. 학생은 다운받아서 Antigravity에 끌어다 놓고 작동하는 걸 보기만 함. 실제 .md 만들게 하지 않음." |

### B. 진단

64p (Act 4 Setup) 와 65p (Act 5 CardNews) 모두 **"파일 생성"** 액션을 가정했음:
- 64p: AGENT.md / SKILL.md / GEMINI.md "생성" + 일부 "직접 입력"
- 65p: news-card-team/ 폴더 생성 + skill_design.md "복붙 → 저장" + GEMINI.md "복붙 → 저장"

→ 실제 워크숍 메커니즘과 불일치:
- 강사가 노션 페이지에 두 파일(skill_design.md, GEMINI.md) 미리 업로드
- 학생은 다운로드 → Antigravity 드래그 → 작동 관찰
- **학생은 .md 파일을 직접 만들지 않음**

### C. 변경 사항

**Slide 64 (Act 4 Setup)**:
- Title: "나만의 AI 팀 만들기" → "받기 → 끌기 → 끝."
- 5단계 → 3단계 압축:
  1. 노션 링크 열기
  2. 두 파일 다운로드 (skill_design.md · GEMINI.md)
  3. Antigravity 에 드래그
- 우측 박스: "* 표시 항목만 직접 수정" → ".md 직접 만들지 않습니다 / 받아서 끌어다 놓고 / 작동하는 모습을 봅니다"

**Slide 65 (Act 5 CardNews)**:
- Title: "카드뉴스 자동화 워크플로우" → "방금 끌어다 놓은 두 파일이 이걸 합니다."
- 3단계 워크플로우 설명 (유지)
- "수강생 액션 — news-card-team/ 폴더 생성 + 복붙" 박스 **제거**
- "파일 2개가 이 전체를 실행" 박스 → 두 파일별 역할 설명 (skill_design.md · GEMINI.md)
- 캡션: "안에 뭐가 적혀있는지 지금 읽을 필요 없습니다. Claude가 읽습니다."

**Prompter.html 동기 갱신** (Rule R-12):
- "Act 4 Setup" 스크립트: "노션에서 폴더 만들고 세 파일 만드세요" → "노션 링크 → 다운로드 → 드래그. 끝."
- "Act 5 CardNews" 스크립트: "복붙만 하세요" → "끌어다 놓은 그 두 파일이 뭐 하는 애들인지"

검증: 76 = 76 mapping ✓ / 64p amber=5 / 65p cyan=5 (단일 accent 유지) ✓

### D. 부수 학습 — 워크숍 메커니즘과 슬라이드 정합성

워크숍 슬라이드 작성 시 **실제 강사 운영 동선** 을 먼저 확인해야 한다. 추측으로 "학생이 직접 만든다" 같은 메커니즘을 적으면 강의 당일 따라하지 못해 무너짐.

→ Rule R-16 후보: **워크숍 실습 슬라이드는 강사 진행안(노션/zip/링크 등 사전 준비 자산)을 확인 후 작성**. 학생 액션 단계는 강사가 실제로 수강생에게 시킬 행동만 적는다.

---

## (다음 작업 추가 예정)
