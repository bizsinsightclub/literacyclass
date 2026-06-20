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

## 2026-05-01 — 전문 강사 관점 콘텐츠 리뷰 반영 (76 → 79 슬라이드)

### A. 사용자 피드백 / 리뷰 응답

| # | 피드백 / 결정 |
|---|---|
| 20 | "AI 리터러시 강사로서 리뷰. 사실 오류·전달 효과 점검." → 내가 진단한 9건 (A~I) 모두 수용. |
| 21 | "비개발자 청중이 알아듣게 쉽게 설명. 나도 Antigravity 시작 → Claude Code 입문~중급 레벨." → 슬라이드 46 코드 파일명 표현 풀어쓰기. |
| 22 | "사례를 광주 vs 광주 같은 국내 사례로." → 파리 호텔 → 광주 호텔 (경기도 vs 광주광역시) 가상 사례 교체. |
| 23 | "최신 시점에 주석으로 달긴 하되, 직접 하고 있으면 고수 인정이라고 해 줘." → "불가능" → "1차 기능 아님" + 고수 인정 톤. |
| 24 | "약한 점 1~4 반영 (출력 톤 다운, 이론↔실습 다리, 한계 슬라이드, 액션 아이템)." → 신규 3 슬라이드 + 17p 톤 다운. |

### B. 발견된 사실 오류 패턴

| 위치 | 오류 | 정정 |
|---|---|---|
| 47p (Prompt Injection) | "Anthropic Claude **Cowork**" — 존재하지 않는 제품명 | "최근 공개된 에이전틱 AI 보안 리서치" 로 출처 일반화 |
| 56p (준비물) | "Max **$200/월**" — Max 는 $100/$200 두 단계 | "$100~$200/월 구독" |
| 9p (Foundation) | "하네스 엔지니어링 = 시스템 프롬프트로 박아 넣기" | "AI 옆에 자동 검수·재시도 장치를 두는 것" — 본질(model surrounding loop) 반영 |
| 27p (Paris Hotel) | "켄터키주 파리" 사례가 실재 사건처럼 톤 | 가상 국내 사례(광주)로 교체 + 문화 적합성 ↑ |

→ Rule R-17 신설 (R-15 보강): **외부 도구·제품명·가격·정의는 슬라이드 작성/리뷰 시 web search 로 검증**. 작성 시점의 기억으로 적은 정보가 1년 안에 어긋날 가능성이 높다. 특히 (a) 제품 라인 이름 (Claude for Work vs Cowork), (b) 가격 티어, (c) 기술 용어 정의 (harness, agent, RAG 등) 는 1차 검증 대상.

### C. 도구 컨텍스트 미스매치 재발 (R-7 의 변형)

이번 세션에서 발견된 케이스:
- **65p (Act 5 CardNews)** : "Claude가 읽습니다" 인데 직전·직후 모두 Antigravity + Gemini 컨텍스트. → "에이전트가 읽습니다" 로 도구 중립 전환.
- **68p / 69p (Full Map / Folder Structure)** : Claude Code 의 `agents/` `skills/` `settings.json` `Hooks` `Agent Teams` 를 "AI 일반론" 처럼 배치했는데 실습은 Antigravity. → eyebrow 를 "본격 도입할 때 만날 풍경" 으로 명시 한정 + "Claude Code 기준 — Antigravity 도 비슷, 이름만 다름" 단서 추가.

→ Rule R-18 신설: **2개 도구를 한 강의에서 다룰 때, 각 슬라이드는 어느 도구 기준인지 명시**. 본 강의처럼 A안(Antigravity 실습) / B안(Claude Code 본격) 으로 한 번 선언했다면, B안 풍경 슬라이드에는 eyebrow / 캡션 / 코드 코멘트로 "B안 기준" 임을 다시 짚어준다. 한 번 선언으로 끝나지 않는다.

### D. 비개발자 청중 친화 카피 — 코드 표현 노출 금지

**증상** (46p Agent Reality 1차본):
> "LLM이라는 '배우'에게 **skill.md · agents/*.md** 라는 '대본'을 쥐여준 상태"
> "지시가 들어오는 순간만 특정 페르소나에 빙의합니다. 대화창을 닫으면 **에이전트도 사라집니다.**"
> "**사이버 분신을 분양받는** 게 아닙니다."

문제: (a) 코드 파일명 직접 노출, (b) "인스턴스/페르소나/빙의" 같은 개발자 어휘, (c) "사이버 분신 분양" 같은 농담이 비개발자에겐 그냥 어색함.

**수정 후** (비개발자 친화):
> "역할 설명서와 업무 매뉴얼"
> "그때만 살아있는 직원" / "부르면 그 순간만 일어나서 일을 합니다"
> "AI가 들어와서 일할 사무실과 매뉴얼을 미리 차려두는 일"

→ Rule R-19 신설: **개념 슬라이드는 코드 파일명·확장자·기술 어휘를 직접 노출하지 않는다**. 도구 매뉴얼 슬라이드 (Ch3.5 실습 슬라이드 등) 에서만 등장. 개념과 실체를 분리하면 비개발자도 끝까지 따라옴.

### E. 시대 흐름 변하는 단정 톤다운

**45p Chat Limit** 의 "ChatGPT, Claude 채팅창에서는 **기본적으로 불가능**합니다" 는 ChatGPT Tasks · Claude Projects 출시 이후 절반은 틀림. 청중 중 일부는 이미 쓰고 있음.

수정 톤: "**1차 기능은 아닙니다**. 신기능으로 흉내 낼 수는 있어요. 직접 쓰고 있다면 — **고수 인정**. 다만 대부분은 아직 1:1 대화로 씁니다."

→ Rule R-20 신설: **"불가능" / "안 됨" / "지원 안 함" 같은 단정형은 6개월 안에 뒤집힐 가능성**. 대신 (a) "1차 기능은 아님", (b) "현재 대부분의 사용 방식은 아님", (c) "신기능으로는 가능하지만 메인 흐름은 아님" 처럼 **현재 우세 + 예외 인정** 톤. 청중 중 고급 사용자도 화나게 안 한다.

### F. 멀티에이전트 비용 단서 — 균형 잡힌 설명

**44p Workflow Comparison** 1차본은 멀티에이전트를 단일 호출의 무조건적 진화처럼 묘사. Anthropic 자체 측정 (`Building effective agents` + `multi-agent research system` 글) 은 **멀티는 토큰 10~15배** 라고 명시.

추가 캡션: "단, 비용은 단일 호출의 **10~15배**. 진짜 분기·병렬이 필요할 때만 — 사실 단일 에이전트 + 좋은 컨텍스트가 이기는 경우가 더 많습니다."

→ Rule R-21 신설: **신기술 / 진화 흐름을 소개할 때 비용·단점 한 줄 의무**. "더 좋은 것" 단순 묘사는 강사 신뢰도 ↓. 비용·복잡도·실패 모드 한 줄을 같은 슬라이드 (또는 직후) 에 둔다.

### G. 페다고지 보강 — 신규 3 슬라이드

| 위치 | 슬라이드 | 목적 |
|---|---|---|
| 신 62 (Ch1.5↔Act2 Files 사이) | **Concept to File Map** | 4 재료(Ch1) → AGENT.md / 3 컨텍스트(Ch2) → GEMINI.md / 워크플로우(Ch3) → SKILL.md 매핑 |
| 신 75 (Series Summary↔Final Closing) | **Limits and Cost** | 할루시네이션 / 토큰 비용 / 검증 루틴 3 카드 |
| 신 76 (Limits↔Final Closing) | **Five Takeaways** | 챕터별 한 줄 액션 아이템 5개 |

→ Rule R-22 신설: **개념(Ch1~3) → 실습(Ch3.5) 이 있는 강의는 둘 사이에 "다리(bridge) 슬라이드" 1장 의무**. "지금까지 배운 X 가 어디로 들어가나" 매핑이 없으면 청중은 실습 슬라이드를 새 정보로 받음 — 인지 부하 ↑.

→ Rule R-23 신설: **마무리 직전 시퀀스 = 한계 → 액션 → 마치며 → QnA**. "할 수 있는 것" 만 보여주고 끝내면 강사 신뢰도 약함. "한계와 비용" 슬라이드 1장 + "구체 액션 아이템" 1장이 retention 결정적.

### H. 페이지 번호 일괄 갱신 — 역순 시프트 패턴

**문제**: 슬라이드 N개 삽입 시 footer 번호가 뒤로 K씩 시프트. 정방향(낮은 번호부터)으로 시프트하면 첫 edit 의 결과(예: 63→64) 가 다음 edit 의 source(64→65) 와 충돌.

**해결**: **역순 시프트 (가장 높은 번호부터)**. 74→77 먼저, 73→74 그 다음, … 63→64 마지막. 각 단계에서 source 번호는 항상 unique 가 보장된다.

이번 작업에서 12개 단순 시프트 + 3개 컨텍스트 disambiguation (분자 중복 케이스 — 신규 슬라이드와 기존 슬라이드가 같은 번호를 일시적으로 공유) 으로 처리.

→ Rule R-24 신설: **N장 삽입 후 페이지 번호 갱신은 (1) 분모 일괄 변경 (replace_all), (2) 영향받는 분자만 역순 시프트, (3) 중복 케이스는 다음 섹션 코멘트나 data-label 을 anchor 로 disambiguate**. 사전에 매핑 테이블을 plan 에 명시하면 안전.

### I. 변경 요약

- **편집 슬라이드 13개**: 9, 17, 27, 29, 44, 45, 46, 47, 56, 65, 68, 69, 71
- **신규 슬라이드 3개**: Concept to File Map (62), Limits and Cost (75), Five Takeaways (76)
- **메타**: Cover meta `76 슬라이드` → `79 슬라이드`, 78개 footer 갱신
- **Prompter sync**: 79 entry, 9개 text 갱신 + 3개 신규 entry 삽입
- **신규 룰**: R-17 ~ R-24 (외부 사실 검증 / 도구 컨텍스트 재선언 / 비개발자 카피 / 시대 단정 톤다운 / 비용 단서 / 다리 슬라이드 / 마무리 시퀀스 / 페이지 갱신 패턴)

### J. CLAUDE.md / design.md 반영

- **CLAUDE.md §3 추가**: R-17 (외부 사실 검증), R-19 (비개발자 카피), R-20 (시대 단정 회피), R-22/R-23 (강의 구조 룰)
- **design.md §6 추가**: "Bridge Mapping (3-카드 recipe)" / "Limits Triple Card" / "Takeaways List" 후반부 시퀀스 블루프린트

---

## 2026-06-20 — 제일기획 사내 변형(index2.html) + 클로드 코드 단일 도구 전환

### A. 작업 개요
원본 `index.html`(80슬라이드, purple/cyan/amber 3색, Pretendard) 을 **보존**하고, 제일기획 사내 세션(같은 '프로'님 대상)용 **클린 에디토리얼 변형** `index2.html`(+`Prompter2.html`) 신규 제작.
- **폰트**: Pretendard → **Samsung SS Head/Body (+KR)** 로컬 woff2 (`design/fonts/woff2`), JetBrains Mono 는 코드/경로용 유지. 헤드 계열=Head, 본문=Body, 웨이트는 보유분(300/400/500/700)로 정리.
- **컬러**: 3색 accent → **단색 민트 `#99EADC`** 슬라이드당 ≤1. glow 12개 `display:none`, 그라디언트 38개 flat 치환.
- **로고**: 우하단 `section.slide::after` 로 전 슬라이드 주입(63×20px≈원본 50%, 흰색 recolor SVG, footer 좌측 페이지번호와 비충돌).
- **단일 도구**: 안티그래비티/GEMINI.md 전면 제거 → Claude Code (CLAUDE.md / `.claude/agents` / `.claude/skills`). 80→77슬라이드(Interlude ×3 제거).
- **발표자**: 동료(프로) 톤 + 사내 세션 프레이밍(시즌→세션).

### B. 재사용한 효율 기법 (다음에도 유용)
1. **accent 별칭 수렴**: `:root` 에서 `--accent-purple/cyan/amber`(+`-10/-20`) 를 **전부 민트값으로 재정의**(별칭). 본문 241개 `var(--accent-*)` 참조를 손대지 않고 일괄 단색화. 리터럴 hex/rgba(`#A855F7`, `rgba(249,115,22,…)` 등) 만 PowerShell `-replace` 로 mint 치환.
2. **그라디언트 패턴별 flat 치환**: `linear-gradient(135deg…)`→transparent(로고박스), `(160deg…)`→`var(--bg-surface)`(카드), `(90deg…)`→transparent(상단 4px 띠 제거), `radial-gradient`→transparent(glow). PowerShell `[regex]::Replace` + 클래스 `[^;"}]*` 로 단일 그라디언트 값 안전 매칭.
3. **footer 전량 재기입**: `[regex]::Replace` 카운터로 `.page` div 76개를 문서순 `02…77 / 77` 일괄. Cover 는 `.page` 없어 자동 스킵. (R-24 의 역순 시프트보다 신규 파일엔 전량 재기입이 안전.)
4. **PowerShell UTF-8 무BOM 저장**: `[System.IO.File]::WriteAllText($p,$t,(New-Object System.Text.UTF8Encoding($false)))` — 한국어 HTML 인코딩 보존.

### C. 신규/강화 학습
- **R-9 (한국어 mono 금지) 재확인**: Claude Code 폴더트리에서 `← 취업규칙` 류 한글 주석은 mono 부모 안에 두면 Malgun fallback 으로 깨짐. **주석 span 만 `font-family:var(--font-body)` 분리**. 파일명(`CLAUDE.md`, `.claude/agents`)은 mono 유지.
- **대형 디스플레이 클래스 폰트 누락 주의**: `.cover-big/.s-title .big/.s-divider .num/.s-bignum .big` 등은 자체 클래스라 `--font-head` 미지정 시 본문(Body) 상속. 헤드 폰트를 묶음 셀렉터로 명시 강제 필요.
- **병렬 덱 변형의 sync 격리**: BroadcastChannel 채널명을 `ai_literacy_sync` → `ai_literacy_sync_v2` 로 index2/Prompter2 양쪽 동시 변경. iframe `src` 도 index2 로. 검증: Prompter2 헤더 `LIVE SYNC ACTIVE` 점등 확인.
- **단일 도구 전환 시 R-18 자동 해소**: 2개 도구(A안/B안) 선언이 사라지면 슬라이드별 "어느 도구 기준" 재선언 부담도 사라짐. 대신 도구별 정확 경로(`.claude/agents/*.md`, `.claude/skills/<name>/SKILL.md`, `.claude/settings.json`)는 web 검증으로 확정(R-17).
- **설치/요금 사실 검증(R-17)**: Claude Code 네이티브 설치기(`irm …install.ps1|iex` / `curl …install.sh|bash`, Node.js 불필요), 로그인=`claude`+계정, 요금 Pro $20·Max $100~$200(전부 포함). 사용량 한도 단서 1줄 동반(R-21).

### D. 남은 알려진 사항
- index2 의 인라인 `font-size` 중 26/27/30/34/36/44px 등 **타입스케일 외 값 다수는 원본 index.html 에서 상속**된 것(본 변형에서 신규 도입분은 전부 허용값). "조금 조정" 범위라 전체 리팩터는 미실시 — 추후 통합 정리 대상.

---

## 2026-06-20 (리파인) — index2 무채색 전환 + 타입 강화 + AP 예시

### A. 피드백 → 조치
- **"형광색 없애"**: 민트 `#99EADC` 가 사용자에게 형광으로 읽힘 → **완전 무채색**(흑·백·회). `:root` 의 `--accent*` 전부 `#FFFFFF`/white-alpha, 리터럴 `#99EADC`·`rgba(153,234,220,…)` 글로벌 → 흰색. 레퍼런스 덱(`design/Cheil_Design.md` §7) 의 "강조 극소 모노톤" 과 정합.
- **"AI 가 상자 안에 있는 느낌 싫다"**: 좌상단 브랜드마크 `AI` 박스 → `.brandmark .logo{display:none!important;}` 한 줄로 Cover+모든 챕터 오프너 일괄 제거(텍스트 라벨만).
- **"세션 같은 거창한 단어 금지"**: `제일기획 · 사내 AI 리터러시 세션` → `제일기획 · AI 리터러시`. 단 "사내 보고서/사내 도입" 같은 **자연스러운 일반 용법의 '사내' 는 유지**(브랜딩 용 거창함만 제거).
- **"재건축 예시가 제일기획 AP 와 안 맞음"**: Context Template "내 회사로 채워보면" + Company Detail + Three Kinds Grid 의 예시를 **광고/AP 세계**(시장·소비자 심층 조사 → 인사이트 → 전략·크리에이티브 제안, 광고주, 경쟁 PT, 금지어 '요즘 트렌드/혁신적인/차별화')로 교체.
- **"타입 위계 Playwright 로 점검 + 폰트 더 커도 비율 맞추라"**: 대표 슬라이드 캡처로 위계 확인. 기존 스케일(Cover 200 / 챕터 176 / 제목 80 / 본문 28)이 이미 §3 TYPE_SCALE·§4 비율을 만족 → **형광/박스 제거만으로 위계가 또렷하게 읽힘**(추가 확대 불필요, overflow 리스크 회피).

### B. 재사용 기법
- **무채색 3계조 규칙**: 형광 제거 후에도 "강조 vs 평문" 구분이 필요 → CSS 묶음 셀렉터로 `열거 번호/라벨(toc-num·cnum·hnum·snum·bullet idx)` 만 `--text-secondary !important` 로 후퇴시키고, 코드·파일명·핵심 em·대형 디바이더 번호는 흰색 유지. `!important` 로 인라인 `color:var(--accent-*)` 잔재를 한 번에 제압.
- **카드 라운딩 일괄 완화**로 "이전 티"↓: `border-radius:32px→20px`, `28px→18px` 글로벌(작은 UI 라디우스 16/14/12 는 보존).
- **Prompter UI 색 충돌 주의**: 텔레프롬프터 `--accent` 는 slide-num 색(밝아야)과 btn-hover 배경(어두워야)에 동시 사용 → 흰색으로 두면 hover 글자가 안 보임. **회색 `#8E8E93`** 으로 두면 둘 다 성립. (audience 비노출 도구지만 형광 제거 일관성 위해 변경.)

### C. 검증 결과
민트 0 / 세션 0 / antigravity·gemini 0 / 재건축·HR SaaS·제조업 0 / @font-face 14 / data-label 77 / footer 전부 /77 / 한글 in mono 0 / **R-12 77=77** / Prompter2 LIVE SYNC ACTIVE. 원본 `index.html`·`Prompter.html` 불변.

---

## 2026-06-20 (리파인 2) — 위계 반전 + 표지/목차 분리 + 플로우차트 + 디테일 강조

### A. 피드백 → 조치 (펀치리스트 14건)
- **표지 다글다글** → Cover 의 4챕터 TOC 제거(타이틀만), 다음에 **목차(Agenda) 슬라이드 신설**(챕터별 개념명 + 한 줄 설명). (+1슬라이드)
- **챕터 표지 < 섹션 구분 위계 거꾸로** → 챕터 오프너를 **흰 배경 반전**(`section.s-title{background:#FFFFFF;color:#0E0E0E}` + 로고 `filter:invert(1)`)으로 띄워 섹션 디바이더(검정)보다 위로. **흰 슬라이드는 우하단 흰 로고가 사라지므로 `::after{filter:invert(1)}` 필수.**
- **챕터 제목이 서술형** → big=개념명(프롬프트/컨텍스트/에이전틱 엔지니어링·실행편), sub=서술형 설명으로 분리.
- **'삽질로 배우는'** → '직접 해보며 배우는' 으로 순화(회사용).
- **박스 안에 글자 디자인 지양** → `.bullet .idx` 의 박스(bg/border-radius/56x56) 제거 → 플레인 mono 글리프(회색).
- **쌍따옴표 불일치** → `.s-quote .mark` 글리프를 `"`(straight)→`“`(curly)로 통일 + 인라인 색 제거(전부 tertiary 회색). 폰트가 straight `"`를 두 막대로 렌더해 튀었음.
- **57~58p 기계적** → `.flow` 컴포넌트(노드+세로/가로 연결선+CSS삼각 화살촉+엣지라벨 bg마스크)로 **mermaid 느낌** 재구성.
- **67p 조직도 선 끊김** → 가로 연결바 `left:17%/right:17%` → **`15%/15%`**(자식 3개가 width:30% space-between 이라 중심이 15/50/85% — 바가 외곽 자식 중심까지 닿아야 연결). 
- **55p 푸터 침범** → 표+불릿 컨테이너 `justify-content:center;flex:1` → **`flex-start`(또는 flex:none)** 로. flex:1+후행 caption 조합은 caption 을 푸터로 밀어냄(구조적 함정).
- **26p 사례 광고화** → 광주 호텔 → **Dove 카피 사례**(비누 Dove[유니레버] vs 초콜릿 Dove[마스] 혼동). 사용자가 'P&G Dove'라 했으나 Dove 비누는 유니레버 — **할루시네이션 강의 슬라이드라 사실 정확성 사수**(P&G 미표기).
- **28p 디테일 강조** → Restaurant '컨텍스트와 함께' 추가 정보에 `<u>` 밑줄(전역 `u{}` 스타일: offset 5px, thickness 2px, 흰 0.55). "밑줄=왼쪽엔 없던 정보" 주석.
- **10~11p** → 슬랙→업무 카톡, 팀장→팀장님, '이번 분기 정리해줘'→'이번 분기 주요 이슈 정리해줘'.
- **interlude 잔재** ('Ch.03 … 잠시 후 시작합니다') 제거.
- **76~77p 삭제**(End, Survey QR) → QnA 가 마지막.

### B. 슬라이드 수 변동 회계
77 → +1(Agenda) −2(End,Survey) = **76**. footer 전량 재기입(`/ 76`), cover-meta 76, Prompter2 = Agenda 추가/End·Survey 삭제 → **R-12 76=76**.

### C. 재사용 교훈
- **`u{}` 전역 스타일** 추가 시 `<u>` 짝 검증 필수(`grep -c '<u>' == grep -c '</u>'`) — 미닫힘 1개면 이후 전 슬라이드 밑줄 오염.
- **큰 굵은 한글 제목이 JPEG 스크린샷에서 밑줄처럼 보이는 건 아티팩트** — `getComputedStyle(el).textDecorationLine` 으로 실측 확인(여기선 전부 none).
- **흰 반전 슬라이드** 도입 시 체크: 텍스트색·brandmark·footer·로고(::after filter) 4종 모두 반전.
- PowerShell 글로벌 치환은 정규식이라 한글 텍스트(특수문자 `.?()'`)는 **`.Replace()`(리터럴)** 사용. 인덱스/페이지 재번호는 `[regex]::Replace` + MatchEvaluator 카운터.

---

## 2026-06-20 (도입부) — Section 00 인트로 4장 추가

- `AI에이전트_도입부_Section00_제작가이드.md`(화면카피·대사·팩트시트 자기완결 명세) 기반으로 **도입부 4장** 신설: 0-A 「어명이오」 훅 → 0-B 「스스로 정하면 에이전트」 3단 사다리 → 0-B½ 「주방」 비유(마누스 배달 / 헤르메스·오픈클로 빈 프로 주방 / 클로드 코드 내 주방) → 0-C 「빌리지 말고 깔아라」(보인다·맞는다·통제된다 → "그래서, 클로드 코드").
- **배치**: Cover **다음**, Agenda **앞** (Cover → S00×4 → Agenda → Ch1). 가이드의 "S00-4 → 본편" 핸드오프는 대사 마지막 줄을 "오늘 갈 길 펼쳐 보고… 말 거는 법부터"로 조정해 Agenda 경유 자연 연결.
- **디자인**: 기존 무채색 그대로 상속. 강조 3블록(사다리 top / 주방 3번째 / 클로즈)은 **흰 테두리 + rgba(255,255,255,0.06) 배경**으로만 — 색 없이 위계. 인라인 font-size 전부 허용값(24/28/40/56/96).
- **flex:1 + 후행 caption 함정 회피**: 사다리·주방 컨테이너를 flex:1 안 주고 자연 높이(top-align)로 둬 caption/footer 비충돌(이전 55p 교훈 적용).
- 슬라이드 76→**80**. footer 전량 `/80` 재기입, cover-meta "도입 + 4개 챕터 · 80". Prompter2 동일 위치에 4 대본 삽입(대사는 가이드 [발표 대사] 사용). **R-12 80=80**, scripts.length=80, LIVE SYNC ACTIVE 확인. 원본 2파일 불변.
- 사실성: 마누스(중국계, Meta 인수 무산)·헤르메스(Nous, MIT, self-host)·오픈클로(Steinberger) — 가이드 §6 팩트시트 근거. 마누스 중국 인프라는 "주방이 어디 있는지조차 확인 안 됨"으로 **은유 처리**(직접 국가 언급 자제).

---

## 2026-06-20 (도입부 리파인2) — 직관성·외주비유·Claude차이·앞단순서

- **3p `Agent Definition` 직관화**: 추상 '길' → **구체 예시 + "다음 수 → 누가" 태그 칼럼**(매크로=사람이 미리 / AI자동화=매번 사람 / 에이전트=AI 그때그때) + caption 자율주행 앵커("도구는 매 단계 내가 운전, 에이전트는 목적지만 주면 자율주행"). 3열 grid(유형/예시/판단주체).
- **4p `Famous Agents` 비유 교체**: 주방 → **외주 vs 인하우스**(광고대행사 직결). 마누스=턴키 외주 / 헤르메스·오픈클로=검증 안 된 신생 외주에 회사 자료 통째로 / 클로드 코드=직접 브리핑해 데리고 일하는 인하우스 팀(흰 강조).
- **5p `Why Build It` 3건**: (a) **"제일기획의 공식 도구" 삭제**(사실 아님) → "공식 도입은 아직 — 그래서 지금 미리" 톤, (b) **Claude(대화) vs Claude Code(행동) 그라운딩 박스** 추가(헤드라인 아래), (c) "보인다/맞는다/통제된다"(번역투) → **"훤히 보인다 / 우리한테 맞춘다 / 고삐는 내가 쥔다"**.
- **앞단 순서**: 사용자 지적 — 표지(1p)와 목차가 인트로로 갈라져 어색. → **Cover → 목차(Agenda) → 인트로 4장 → 1편** 으로 Agenda 를 인트로 앞으로 이동(원래 의도 복원). `Why Build It` 대본 핸드오프도 "오늘 갈 길 펼쳐 보고" 삭제 → "말 거는 법부터 — 1편입니다." 1편 직결.
- **공식 도구 주의**: 가이드 §0 은 "클로드=공식 도구"라 했으나 **사용자가 사실 아님으로 정정** → 본문/대본 모두 도입 대비 톤. (단 "Anthropic 공식 AI 코딩 에이전트"=사실, 준비물에 유지.)
- 슬라이드 80 불변. Agenda 이동으로 footer 전량 재번호(Agenda=02). **R-12 80=80, 순서 1:1, LIVE SYNC ACTIVE.** 무채색·허용폰트·원본 2파일 불변.
- **Edit 로 항목 삭제 후 인접 줄 병합 주의**: 선행 `\n` 포함 매칭 삭제 시 다음 엔트리와 한 줄로 붙음 → `[regex]::Replace('},[ ]{2,}\{ title:' , '},<NL>...')` 로 복원(이전 교훈 재적용).

---

## 2026-06-20 (폰트 감사) — 한글이 mono 로 렌더되던 버그 일괄 수정

- **증상**: 3p eyebrow("INTRO · 한 장으로 끝내는 에이전트"), 4p 외주카드 라벨(마누스/헤르메스·오픈클로/클로드 코드), 18p Four Ingredients 예시 카드(너는 SaaS…) 등에서 **한글이 JetBrains Mono → 시스템 모노 fallback** 으로 떨어져 자간 벌어진 흉한 렌더. (playbook §2.3 화이트리스트=Samsung SS + ChosunilboNM 뿐, mono 는 라틴 전용.)
- **근본 원인 2가지**: (1) 인라인 `font-family:var(--font-mono)` 가 한글 텍스트에 직접 적용(eyebrow, 외주 라벨). (2) **CSS 클래스 `.card .ex` / `.card .cnum` 가 mono 인데 한글 예시/라벨을 상속** — 인라인 grep 으로는 안 잡히는 케이스.
- **수정**:
  1. **안전망**: `--font-mono` 폴백에 Samsung SS KR 삽입 → `"JetBrains Mono","Samsung SS Body KR","Samsung SS Body","Malgun Gothic",ui-monospace,monospace`. JetBrains Mono 가 **라틴 전용(한글 글리프 없음)** 이라 한글 글리프는 자동으로 Samsung SS Body KR 로 폴스루(라틴은 mono 유지). 글리프 단위 폴백.
  2. `.card .ex` / `.card .cnum` → `--font-body` (한글 예시/라벨 전부 Samsung SS Body). 단 Concept-to-File 등 **라틴 경로 span 은 인라인 mono 라 그대로 유지**(class 변경에 안 먹힘) — 의도된 분리.
  3. 인라인 mono 한글 직접 적용분(eyebrow→Samsung SS Head, 외주 라벨 3개→Samsung SS Body) 제거.
- **JetBrains Mono 유지 근거**: 라틴 코드/경로/숫자/폴더트리(├──└── 정렬) 한정. 한글은 0건. (playbook 위주 + 라틴 코드 기능 예외.)
- **검증법**: 인라인은 `grep 'font-family:var(--font-mono)[^>]*>[^<]*[가-힣]'`, **CSS 상속분은 클래스 정의를 직접 점검**(인라인 grep 사각지대). 최종 strict=0, 무채색 0, 80슬라이드·@font-face 14 유지, 원본 불변. Playwright 로 3p·4p·18p 클린 렌더 확인.

---

## 2026-06-20 (도입부·간지·레이아웃 정리 + 전수 오버플로 스윕)

- **장식용 폰트 선별 적용 (Intro Hook 결의 줄)**: 사용자가 특정 한 줄("효시일체, 1인 1에이전트 하랍신다")에 한해 `design/fonts/ttf/Griun_PolSensibility-Rg.ttf`(붓글씨 감성체) 지정. @font-face 1개 추가(ttf 직접 `format("truetype")`), 해당 줄에만 `font-family:'Griun PolSensibility','Samsung SS Head KR',sans-serif;font-weight:400`. **주의**: (1) 장식체는 Regular만 있어 `font-weight:700` 상속 시 합성볼드로 추해짐 → 반드시 400 명시. (2) 한자 글리프 미보장 → 결의 줄에서 한자(嚆矢一體) 빼고, 한자 풀이는 Samsung SS(한자 지원) 쪽 gloss로 분리. playbook 화이트리스트(Samsung SS/ChosunilboNM) 예외지만 **사용자 명시 지정이므로 1줄 한정 허용**.
- **위계 재배치 = 절대크기 아닌 대비**: "어명이오!"(160) > 결의 줄(80, 장식체) > 풀이(24, tertiary, 우하단 absolute). gloss는 `position:absolute;right:100px;bottom:104px;text-align:right`로 Cheil 로고(우하단 right:60 bottom:40) 위에 안 겹치게.
- **간지(s-title) 일괄 정비**: (1) `.s-title{justify-content:flex-end→center}` 한 줄로 4개 간지 전부 세로 중앙. (2) 시리즈 kicker("직접 해보며 배우는 AI 리터러시 · N편") 4개 모두 제거 — 챕터 넘버는 brandmark("Chapter 0N · of 04")가 이미 담당하므로 중복. (3) sub 카피는 첫 문장 절 뒤에서 `<br>` 1회로 2줄 균형(의미 단위 줄바꿈).
- **모호 단정 카피 → 명시적 사실로**: "채팅창의 1차 기능은 아닙니다"(모호) → "원래 '1:1 대화'용입니다"(정체성 명시 = 한계 함의, 비단정). 동반 bullet의 단정형("병렬 실행 불가")도 R-20대로 "기본 동작이 아닙니다"로 톤다운. **슬라이드 카피 변경 시 Prompter 대본의 동일 표현("1차 기능은 이게 아닙니다")도 함께 교체**(인덱스/라벨 불변이라 R-12 카운트는 그대로지만 text 동기화 의무).
- **무채색에서 "상위/다음 개념" 강조법**: 색을 못 쓰므로 (a) 카드 elevation(`bg-elevated`), (b) 밝은 흰 테두리(rgba .55), (c) 상단 4px 흰 띠(absolute child — border-left 금지룰과 무관, 상/하단 띠는 허용), (d) "→" 진행 화살표 칼럼(`grid-template-columns:1fr 88px 1fr`), (e) "오늘의 목적지" 마커, (f) 좌측 카드는 테두리·문구 recede. 이 6개를 합치면 색 없이도 명확한 위계.
- **읽혀야 할 본문에 text-tertiary 금지**: tertiary(#5A5A60)는 footer 페이지번호·"반복…" 같은 진짜 보조용만. 청중이 읽어야 하는 "나쁜 출력 예시" 본문이 tertiary면 너무 옅음 → secondary(#8E8E93). **전역 tertiary 값은 건드리지 말 것**(footer 등 의도된 옅음 깨짐) — 인스턴스별 교체.
- **flow 다이어그램 키울 때 세로 예산 주의**: 노드 키우고 카드에 "예시" 줄까지 추가하면 카드가 길어져 하단 caption이 footer 침범. **정량 검증 필수**: Playwright로 `caption.bottom - footer.top` 측정 후 edge height(62→40~44)·padding·margin 미세 축소로 ~40px 확보. 63p·62p 둘 다 이 패턴으로 해결.
- **전수 오버플로 스윕 기법**: 80장 `goTo(i)` 순회하며 **leaf 텍스트 노드만**(자식 있는 컨테이너 제외 — 카드 배경 box가 footer 근처까지 내려오는 건 정상, 오탐의 주범) 대상으로 (우측 padding 초과 / footer 밴드 침범 / 슬라이드 하단 초과) 검사. 컨테이너 포함 시 오탐 3건 → leaf-only로 실제 1건(62p)만 검출. 최종 "ALL CLEAR".

---

## 2026-06-20 (2차 레이아웃 정리 — footer 침범·다이어그램 간격·슬라이드 삭제·인용 간격)

- **footer 침범의 두 원인 구분**: (1) flex:1 카드가 콘텐츠 min-content보다 작은 공간에 눌려 박스가 footer로 흘러넘침(37p) → `flex:none` + 폰트(28→24)·gap(20→16)·padding(48→36) 축소로 자연 높이를 줄임. (2) `line-height` 과대(72p 트리 `line-height:2`) → 1.7로. **정량 측정이 답**: `el.bottom - footer.top` 으로 침범 px 측정 후 그만큼 트림. leaf 텍스트가 아닌 **컨테이너 박스**가 침범해도 시각적으로 페이지번호와 붙어 보이니, 박스 기준(`*` 순회)으로도 측정.
- **flow edge 라벨 겹침 = 높이 부족**: `.flow .edge` 안에 라벨(top:50% 중앙)과 화살촉(bottom)이 같이 있으면, edge 높이 H가 작을 때 겹침. 무겹침 조건 ≈ **H ≳ 라벨높이+화살촉+여유 ≈ 52px**. 40px로 줄였더니 사람판단 라벨이 화살촉과 붙음(63p) → 56px로. **footer 여유와 trade-off**라 measure→edge↑→다른 margin 미세↓로 균형.
- **인용 mark(큰 따옴표)의 거대 공백 = line-box 잔여공간**: `.s-quote .mark` 280px / `line-height:0.8` 이면 line-box 224px인데 " 글리프는 상단 ~1/3만 차지 → 아래 빈 공간 + margin이 본문과 큰 간격을 만듦. **`line-height`를 0.5로** 줄이면 box가 글리프에 밀착, 본문이 바로 아래로(요청 "0.5cm"). margin-bottom은 0. (글리프는 box 밖으로 overflow하지만 시각만, 레이아웃은 box=line-height 기준이라 본문이 당겨짐.)
- **슬라이드 삭제 + 재번호(R-24) 실전**: 78p(Five Takeaways) 삭제 → 79장. 순서: (1) 섹션 블록 제거, (2) **분모 변경 전에** 꼬리 두 장 numerator를 유니크 풀스트링으로 먼저 교체(`79 / 80`→`78 / 79`, `80 / 80`→`79 / 79`) — 분모가 아직 /80이라 충돌 없음, (3) 남은 전체 `replace_all ' / 80</div>'→' / 79</div>'`, (4) Cover meta `80→79 슬라이드`, (5) Prompter2 해당 객체 1줄 제거.
- **Prompter 객체 제거 시 merge 버그 방지**: 삭제할 엔트리 줄을 **다음 엔트리 시작과 함께** 매칭해 교체(`...겁니다.\` },\n        { title: "Final Closing",` → `        { title: "Final Closing",`)하면 빈 줄·병합 없이 깔끔. R-12 검증 시 `^        { title:` 라인수로 세되, **`{ title: ` 단순 occurrence는 JS 파서 코드(`result.push({ title: ...})`)까지 잡아 +2 과다 계상**되니 라인수 기준이 정확.
- **footer 연속성 검증**: leading-zero(`02`) 때문에 grep/sort가 꼬일 수 있음. `class="page">N / 79` 추출 → `uniq -c`로 중복, `seq`와 비교로 누락 확인. 최종 02..79 연속 + Cover(footer 없음) = 79장.
- (참고) 78p는 R-23의 "액션 아이템" 장이었음 → 사용자 명시 삭제로 마무리 시퀀스는 [한계·비용(77)→마치며(78)→QnA(79)]로 축약. 사용자 > governance 룰.

---

## 2026-06-20 (표지 리브랜딩 + 도입 quote 슬라이드 + 오디오 큐 + 로고 삽입)

- **슬라이드 삽입(insert) 재번호(R-24) — bump 방향 주의**: 신규 1장을 4번 위치에 삽입 → 80장. 순서: (1) **삽입 전에** 기존 numerator를 PowerShell `[regex]::Replace` + MatchEvaluator로 일괄 처리 — `n>=4면 +1`, 분모 79→80 동시에. (2) 그다음 신규 섹션을 `04 / 80`으로 삽입(이미 bump된 05 앞). 삭제(R-24)는 "역순 시프트"였지만 **삽입은 임계값 이상 전부 +1** 한 번에 끝. 분모는 `'{0:00} / 80</div>' -f $n`로 2자리 zero-pad 유지.
- **PowerShell 수정 후 Edit 실패**: PowerShell로 파일을 쓰면 Edit 도구가 "modified since read"로 거부 → 수정 영역을 **Read 한 줄 다시 읽고** Edit. (renumber=PowerShell, 콘텐츠 삽입=Edit 조합 시 항상 발생.)
- **deck-stage 슬라이드 진입 이벤트 = `window` `message`의 `slideIndexChanged`**: 오디오/효과를 특정 슬라이드 진입에 걸려면, 기존 Prompter sync 브리지의 `window.addEventListener('message', ...)`에서 `d.slideIndexChanged` 분기에 훅을 추가. **인덱스가 아니라 `slides[idx].dataset.label`로 판정**(나중에 재배치돼도 안 깨짐). 진입 시 타이머 set, 이탈 시 `clearTimeout`+`pause/currentTime=0`. 초기 위치(`stage.index`)에도 동일 호출.
- **브라우저 오디오 자동재생**: `new Audio('x.mp3')`는 detached(DOM 밖)라 `querySelector('audio')`로 안 잡힘. 발표 중엔 사용자 네비게이션(키/클릭)이 user-gesture라 3초 지연 `play()` 허용됨. `play()`는 항상 `.catch(()=>{})`로 reject 무시. 헤드리스 검증은 **`slideIndexChanged` 발화([0,3]) + 해당 idx label 확인**으로 배선만 검증(실제 소리 X).
- **다크덱 로고 삽입 — 파일별 처리**: 투명 PNG라도 잉크 색이 관건. (1) **검정 일러스트(hermesagent)=다크 배경서 안 보임 → `filter:invert(1)`**. (2) 흰색 로고(manus)=그대로. (3) 컬러 마스코트/마크(openclaw 빨강, claude 주황)=그대로 노출(브랜드 인용은 무채색 룰 예외). `<img height:34~46 object-fit:contain>`, 카드 하단은 `margin-top:auto`로 정렬. **공백 있는 파일명**(`claude logo.png`)도 src에 그대로 쓰면 됨(브라우저가 인코딩).
- **표지 세로 중앙정렬**: `.s-cover{justify-content:space-between→center;padding 대칭}` + 상단 brandmark 줄(`.cover-top`)을 `position:absolute;top/left/right`로 띄워 흐름에서 빼면, 가운데 타이틀 블록만 수직 중앙. (cover 전용 클래스라 안전.)
- **긴 영문 표지 타이틀**: `.cover-big`(200px)는 한글 단어용 → 영문 장문은 인라인 `font-size:160px`(허용값)로 낮춰 2줄. letter-spacing -0.045em이 폭을 잡아줘 22자도 1720 안에 들어옴. `<title>` 태그도 같이 교체(탭 일관성).

---

## 2026-06-20 (인용 위계 통일 + 채움답 밑줄 + 카드 상단정렬)

- **s-quote 통일 패턴**: eyebrow를 마크 위에서 빼서 **cite 블록 첫 줄(letter-spacing 0.06em lead)로 이동** → 마크 → 본문(q) → [라벨+cite] 하단 묶음. "같은 위계로" = 라벨을 cite와 같은 회색·크기 패밀리로(letter-spacing만 살짝). 5개 s-quote 전부 동일 처리. 무채색이라 eyebrow의 amber/cyan/purple 변형은 어차피 text-secondary로 수렴 → 색 손실 없음.
- **상단/하단 따옴표 위계 일치**: 풀쿼트 박스는 여는 마크(좌상 80px tertiary)와 닫는 마크가 따로 놀기 쉬움(닫는 게 본문 끝 인라인 28px). **닫는 마크도 동일 80px tertiary로 별도 div, `text-align:right`** 줘서 좌상-우하 대칭. 여는 " (U+201C) / 닫는 " (U+201D) 구분.
- **'채워진 답' 밑줄**: 템플릿 빈칸 대비 실제 채운 답을 `text-decoration:underline;text-underline-offset:5px;`로. **scope 검증 필수** — `<b style="color:var(--accent-purple);">`가 정확히 그 카드에만 5개인지 grep -c로 확인 후 replace_all. (전역 `u{}` CSS 규칙이 grep line-count에 +1 잡히는 건 무관.)
- **카드 상단정렬**: `justify-content:center→flex-start`. `align-items:stretch`라 두 카드 높이는 같고(긴 쪽 기준), 짧은 카드는 하단 여백이 생김(의도된 정렬). 콘텐츠 양 불변이라 footer 침범 없음.

---

## 2026-06-20 (낭송 순차 강조 애니메이션 + 음악 토글 버튼)

- **슬라이드 단위 순차 강조(낭송 효과)**: 본문을 `<span class="recite-seg">`로 세그먼트화(줄 안에서도 분리, 사이 구두점은 plain text=회색 유지). CSS는 `.recite-seg{transition:color/transform;display:inline-block}` + `.recite-seg.on{color:#fff;transform:scale(1.1)}`. **transform:scale는 reflow 없음** → 이웃 세그먼트 안 밀림(글자만 시각적으로 커짐). 컨테이너 flex `gap:18px`면 1.1배 확대해도 윗줄/footer 안 침범. JS는 `setTimeout` 3초 간격으로 `.on`을 다음 세그먼트로 옮기며 `%length`로 **루프**(머무는 동안 계속). 진입 시 `startRecite()`, 이탈 시 `stopRecite()`(타이머 clear + 전부 회색 복귀).
- **데크 내 인터랙티브 버튼(음악 끄기)**: 슬라이드에 `<button id="cue-mute" data-on="1">`. 브리지 스크립트에서 `addEventListener('click')`로 토글 — on=1이면 stop+라벨"음악 켜기", on=0이면 play+"음악 끄기". `Audio` 객체와 타이머는 IIFE 클로저에 두고 `stopCueAudio()/playCueAudio()` 헬퍼로 버튼·자동재생·슬라이드훅이 공유. 자동재생 타이머는 `if(muteBtn.dataset.on==='1')` 가드로 "끄기 누른 뒤엔 3초 후에도 안 켜짐" 보장. 진입 시 버튼 라벨 리셋.
- **R-12 grep 함정**: JS에 `querySelectorAll('section[data-label="..."]')`를 쓰면 `grep -c data-label=`가 +1 과다 계상(81로 보임). **`grep -cE '<section class="slide'`로 실제 섹션만** 세야 정확. (낭송/오디오 훅처럼 data-label로 슬라이드를 찾는 JS가 생기면 항상 발생.)
- **표지 미니멀화**: cover-top(brandmark) 통째 제거 후에도 `.s-cover{justify-content:center}` + cover-mid만 남아 수직 중앙 유지(cover-top이 absolute였어서 흐름 영향 0). 긴 영문 타이틀은 160px 인라인.

---

## 2026-06-20 (낭송 슬라이드 보강 — 겹침 해결·스피커 아이콘·1회 재생)

- **transform:scale 강조의 겹침 = 같은 줄 이웃 침범**: scale(1.1)은 박스는 그대로 두고 글자만 키워서, **한 줄에 세그먼트 2개면 커진 쪽이 옆 세그먼트를 덮음**(특히 긴 세그먼트). 해결: **세그먼트 1개당 1줄**로 분리(가운데 정렬). 가로 이웃이 없어 겹침 원천 차단, 긴 줄도 scale 후 우측 여백 500px 확보. (대안인 font-size 리플로우는 줄 폭이 넘쳐 위험 → 비채택.)
- **이모지 금지 환경의 아이콘**: 스피커/뮤트는 🔊🔇(이모지) 금지 → **인라인 SVG**로 직접 그림. 스피커콘(filled path) + `.spk-on`(음파 arc 2개) / `.spk-off`(X 2선) 그룹을 두고, `#cue-mute[data-on="0"] .spk-on{display:none}` / `.spk-off{display:inline}` CSS로 상태 스왑. 클릭 핸들러는 `data-on` 토글 + 오디오 stop/play만(텍스트 라벨 없음, aria-label만 갱신). 우상단 `position:absolute;top:44;right:64`.
- **1회 재생(무한루프 금지) + 재진입 시 재시작**: `reciteStep`에서 `reciteIdx % length`(루프) 대신 `if(reciteIdx < length) schedule` → 마지막 세그먼트에서 타이머 안 검 = 1회만, 마지막은 켜진 채 정지. 진입 훅(`handleCueAudio`)이 매번 `stopRecite()`(타이머 clear + `.on` 제거) 후 `startRecite()`(idx=0)라 **재진입마다 처음부터** 다시. 이탈 시에도 다른 슬라이드의 handleCueAudio가 stopRecite 호출.
- **deck-stage 이탈 정리는 async**: `goTo(other)` 직후 동기 검사하면 `slideIndexChanged`(postMessage)가 아직 안 와서 `.on`이 남아 보임(오탐). ~300ms 후 검사하면 정상 클리어 확인됨. 정리 로직 검증은 딜레이 필수.

---

## 2026-06-20 (도입부 재배열 + 카드 디테일 + 메타포 정합)

- **슬라이드 한 칸 이동(reorder)의 footer 재번호 = 회전(rotation)**: 목차를 2→7로 옮기면 {02..07}이 한 칸씩 도는 순열이라 **단일 치환마다 충돌**. 해결: 한 슬라이드를 **temp 번호(`00 / 80` — cover는 footer 없어 안전)** 로 빼두고 → 나머지를 빈 자리로 순차 이동 → 마지막에 temp를 최종값으로. 그 후 **DOM 물리 이동**(delete + insert)으로 화면 순서와 footer 순서 일치. (footer만 바꾸고 블록을 안 옮기면 deck-stage가 DOM 순서로 보여줘 번호가 뒤죽박죽.)
- **재배열에 강한 훅은 label 기반**: Agent Intro Quote가 idx 3→2로 밀렸지만 오디오·낭송 훅이 `dataset.label` 판정이라 무수정 동작. 인덱스 기반이었으면 다 깨졌음. (앞서 적은 교훈 재확인.)
- **혼합 사이즈 타이틀**: 표지 "The Hitchhiker's Guide to"만 96px(=160의 60%) `<span>`, 다음 줄 "the AI Agents." 160px. 작은 행이 부제처럼 큰 행을 받쳐 위계가 살고 줄바꿈도 자연스러워짐.
- **메타포는 헤드라인↔본문 매핑이 일치해야**: "회로판은 내가 깐다 / 전기는 LLM이 흘린다" 인데 본문 박스가 "전기=Claude"만 설명하고 회로판은 안 풀어주면 붕 뜸. 박스에서 **회로판=구조(파일·규칙·권한), 전기=Claude(지능)** 둘 다 명시 → 메타포 완결. Claude(대화) vs Claude Code(행동) 구분은 유지.
- **장점/단점은 라벨+구분선으로 분리**: 인라인 +/− 보다 "장점"/"단점" 헤더 + 가로 divider가 더 명확. 채움 답·예시 서비스(Power Automate·n8n·Claude Code)는 본문 옆/아래 muted gray 한 줄로 — 한·영 혼용이라 mono 금지(Samsung SS body).

---

## 2026-06-20 (비교카드 통일 글로우 + 표지/낭송/음악/영상)

- **비교 카드 통일 — 클래스 + `!important`로 인라인 일괄 제압**: `.cmp-card{bg·border·shadow !important}` + `.cmp-card,.cmp-card *{color:#fff!important}`(인라인 회색 전부 흰색) + `.cmp-card.cmp-hi{밝은 테두리+다층 box-shadow 글로우}`. `.ba-col` 기반 비교 슬라이드는 **CSS 셀렉터에 `.ba-col` 합류**(`.cmp-card,.ba-col` / `.ba-col.after,.ba-col.cmp-hi`)시켜 9장+α 전부 HTML 최소수정으로 통일. `.after`가 강조면 자동 글로우, 인라인 border로 강조하던 카드만 `cmp-hi` 클래스 추가. 위쪽 4px 띠(`<div>`)는 삭제 — 글로우가 대체.
- **글로우 = box-shadow 다층**: `0 0 0 1px(테두리 hug) , 0 0 26px(중간 halo) , 0 0 60px(외곽 번짐)` 흰색 저알파. `overflow:hidden`이 박스섀도를 자르지 않음(콘텐츠만 클립). "은은하게 테두리 따라 빛남" 달성.
- **카드 높이 균일(단점 정렬)**: 장점 줄 수가 카드마다 다르면 구분선·단점이 어긋남 → **장점 블록에 동일 `min-height`** 주면 2줄 카드에 하단 여백이 생겨 divider+단점이 같은 y로 정렬. (양쪽 갯수 통일보다 콘텐츠 보존.)
- **`<video>` 구간 크롭(00:10~01:00)+배속+클릭재생**: HTML5 video는 네이티브 구간크롭 없음 → JS로 `currentTime=10` 시작 + `timeupdate`에서 `>=60`이면 pause+리셋, `playbackRate=1.25`, 클릭 토글. **핵심 함정: 시킹은 서버 HTTP Range(206) 필수.** `python -m http.server`는 Range 미지원(200, `seekable.length===0`)이라 로컬선 시크 실패 → **GitHub Pages는 Range 지원**이라 실제 배포선 동작. 코드엔 `seekable`로 시크가능 판정 + `seeked` 미발생 700ms 폴백 → range 없는 서버에선 그냥 재생(행 안 걸림). 세로영상(480×854)은 `height:680;width:auto`로 레터박스 제거. 클릭아이콘은 이모지 금지라 인라인 SVG(원+삼각).
- **표지 혼합 크기 3줄**: 윗 2줄 116px(96×1.2, 표지 히어로 예외), 막줄 160px. **줄바꿈 orphan 해결**은 보통 `max-width` 넓혀 한 줄에 들어가게(분할 divider 서브의 "데 있습니다" 고아 → max-width 1640으로 2줄화).
- **낭송 마지막 구절 한 번에 강조**: 두 `<span class="recite-seg">`을 **하나의 span에 `<br>` 넣어** 병합 → inline-block scale이 두 줄을 한 덩어리로 강조. 세그먼트 수는 JS가 `reciteSeg.length` 동적이라 무수정.
- **오디오 페이드**: `setInterval`(80ms)로 `currentTime` 감시, 18s부터 `volume=(20-t)/2`, 20s에 0+pause. stop/재생 때 `volume=1` 리셋 + interval clear.
- **푸터 여백 일괄 확보(~35px)**: 거의 모든 콘텐츠 슬라이드가 `.split/.ba/.recipe/.steps`(전부 `flex:1`) 때문에 본문 하단이 footer.top 대비 gap 8px까지 바짝 붙음. **단일 레버** = `section.slide` 하단 패딩 80→105 (footer는 `position:absolute;bottom:40` 불변) → flex:1 블록이 25px 덜 늘어 자동 상승, 여백 있는 슬라이드 일괄 해결. **함정**: 여백 없이 꽉 찬 dense 슬라이드(번호 그리드 등)는 10~14px clipping → `section.slide.tight-bottom{padding-bottom:80px}` 예외 클래스로 원복(이들은 "여백 많은" 케이스 아님). Playwright로 `scrollHeight-clientHeight` 전수 스윕해 예외 대상 식별.
- **내부 여백을 예시박스로 채우기**: flex:1+align-items:stretch로 늘어난 카드의 빈 하단을, 설명 다음 예시 블록에 `margin-top:auto`를 줘 바닥에 정렬 → 상단 설명/하단 예시 2단 분포로 여백 해소(20p 역할 예시). 후략은 표준 `…`(U+2026, 이모지 아님; 덱에 기존 사용).
- **박스 높이 통일은 스코프 클래스+min-height**: 3카드 하단 예시박스 높이 제각각 → `.recipe`에 `three-kinds` 클래스 + `.recipe.three-kinds .ex{min-height:160px}` (19p `four-ing` 패턴 재사용). min-height는 **가장 긴 박스의 실측 자연높이 이상**으로(3줄=156 → 160). 공유 CSS(.ex) 직접 수정 금지 — 다른 슬라이드 오염. 좌우 인용박스도 동일 — 둘 다 같은 `min-height`(긴 쪽 실측+여유)로 주면 크기 통일(34p 89/166 → 168/168), 짧은 쪽은 top-align 빈공간이 오히려 "정보 적음"을 강조.
- **표지/대형 디스플레이 자간은 design.md 룰 준수**: design.md = 한글 `-0.01em`, 영문 `-0.02em` 기준. `.cover-big`의 `-0.045em`은 둘 다보다 과밀 → 96px 미만 줄에선 글자 붙어 보임. 서브히어로(88px) 영문 줄은 `letter-spacing:-0.02em`(Latin 기준)으로 풀어야 자연스러움. 최대 히어로(160px+)만 타이트 유지 가능.
- **2x2 사분면 플로우(flow-quad)**: 세로 일렬 노드(검색→요약→디자인→저장)는 카드 가로 공간을 버림 → 3×3 grid(`auto Npx auto`)로 4모서리 노드 + 사이 화살표. 흐름 **2(TL)→1(TR)→4(BR)→3(BL)** 시계방향. 커넥터 라벨은 `position:absolute` + `background:var(--bg-surface)`(카드 bg와 동일)로 선을 마스킹 → 다이어그램과 안 겹침. 화살촉은 기존 CSS 삼각형(border 기법) 재사용.
- **비교 카드 라벨 위계 대칭**: A/B 비교 카드에서 한쪽 eyebrow 옆에만 보조 라벨(예 우측 "오늘의 목적지")이 있으면 비대칭 → 반대편도 eyebrow 옆에 같은 스타일 보조 라벨(좌측 "지난 편에서 끝낸 기본기")을 둬 위계 통일. 하단 떠 있던 라벨을 eyebrow 옆으로 끌어올리면 빈 하단도 정리됨.

---

## (다음 작업 추가 예정)
