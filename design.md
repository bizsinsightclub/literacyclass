# design.md — AI Literacy Club Deck Design System

> 샘플 템플릿 (`uploads/sample template.pdf`, 30p, 다크 모드, 1920×1080) 을 기반으로 추출한 프레젠테이션 디자인 시스템.
> 모든 슬라이드는 이 문서의 토큰과 컴포넌트만 사용해 작성합니다. **새 값을 즉흥적으로 만들지 마세요.**

---

## 1. 디자인 원칙 (Design Principles)

1. **Dark-first, 고대비.** 배경은 검정, 텍스트는 순백. 강조는 purple/cyan/amber 중 한 가지.
2. **타입이 주인공.** 대형 타이포그래피로 위계를 만든다. 아이콘·이모지로 보완하지 않는다.
3. **여백은 구조다.** 패딩 100px는 "빈 공간"이 아니라 "호흡"이다. 채우지 않는다.
4. **반복이 리듬을 만든다.** 섹션 디바이더, 타이틀, 본문 레이아웃은 파라미터만 바뀌고 구조는 고정.
5. **한 슬라이드 = 한 메시지.** 정보가 두 개면 슬라이드를 둘로 쪼갠다.

---

## 2. 전역 변수 (Global Tokens)

```js
// components/tokens.js
export const CANVAS = {
  width:  1920,
  height: 1080,
};

export const COLORS = {
  // Surface
  bgMain:      '#0E0E0E',  // 기본 배경 (거의 검정)
  bgSurface:   '#1C1C1E',  // 카드 / 2차 배경 / 비활성 바
  bgElevated:  '#2A2A2D',  // 호버·강조 카드 (선택)

  // Text
  textPrimary:   '#FFFFFF',
  textSecondary: '#8E8E93',
  textTertiary:  '#5A5A60', // 캡션보다 더 약한 보조 (선택)

  // Accents — 슬라이드당 최대 1종
  accentPurple: '#A855F7',
  accentCyan:   '#06B6D4',
  accentAmber:  '#F97316',

  // Accent 투명 오버레이 (10% / 20%) — 그라디언트 & Before/After용
  accentPurple10: 'rgba(168, 85, 247, 0.10)',
  accentPurple20: 'rgba(168, 85, 247, 0.20)',
  accentCyan10:   'rgba(6, 182, 212, 0.10)',
  accentAmber10:  'rgba(249, 115, 22, 0.10)',
};

export const FONT = {
  sans: `"Inter", "Pretendard", "Helvetica Neue", -apple-system, sans-serif`,
  // 한국어 덱일 땐 "Pretendard" 를 앞에 두길 권장
  mono: `"JetBrains Mono", ui-monospace, monospace`,
};
```

> **한글 폰트 주의**: Inter 는 라틴 전용. 한국어가 섞일 때는 `Pretendard` (OFL, Google Fonts) 를 우선시하고 Inter 를 fallback 으로.

---

## 3. 타이포그래피 스케일 (TYPE_SCALE)

모든 폰트 사이즈는 이 객체의 값만 사용합니다. 애드혹 픽셀값 금지.

```js
export const TYPE_SCALE = {
  // Display (타이틀 슬라이드, 큰 숫자)
  display:    { size: 160, weight: 700, lineHeight: 1.0, letterSpacing: '-0.03em' },

  // H1 — 슬라이드 타이틀
  title:      { size: 80,  weight: 700, lineHeight: 1.2, letterSpacing: '-0.02em' },

  // H2 — 섹션 / 스텝 제목
  section:    { size: 56,  weight: 600, lineHeight: 1.3, letterSpacing: '-0.01em' },

  // H3 — 리스트 항목, 컬럼 제목
  subsection: { size: 40,  weight: 600, lineHeight: 1.35, letterSpacing: '-0.005em' },

  // Body
  body:       { size: 28,  weight: 400, lineHeight: 1.6 },
  bodyBold:   { size: 28,  weight: 600, lineHeight: 1.6 },

  // Caption / metadata
  caption:    { size: 20,  weight: 400, lineHeight: 1.5, color: '#8E8E93' },

  // Eyebrow (섹션 번호, 스텝 번호 라벨)
  eyebrow:    { size: 22,  weight: 500, lineHeight: 1.2, letterSpacing: '0.12em', textTransform: 'uppercase' },
};
```

### 사용 가이드

| 용도 | 토큰 |
|---|---|
| 덱 표지 메인 | `display` |
| 슬라이드 타이틀 | `title` |
| 섹션 구분 슬라이드의 섹션명 | `title` 또는 `section` |
| 워크플로우 스텝 제목 | `section` |
| 아젠다 리스트 아이템 | `subsection` |
| 본문 단락 | `body` |
| 짧은 강조 단어 | `bodyBold` |
| 이미지 캡션, 출처, 페이지 번호 | `caption` |
| "01 — INTRO" 같은 오버라인 | `eyebrow` |

---

## 4. 스페이싱 토큰 (SPACING)

```js
export const SPACING = {
  // 슬라이드 프레임 패딩
  paddingTop:    100,
  paddingBottom: 80,
  paddingX:      100,

  // 수직 리듬
  titleGap:      52,   // 타이틀 ↔ 서브타이틀/본문
  sectionGap:    64,   // 본문 내부 섹션 간
  itemGap:       28,   // 리스트 아이템 간

  // 수평 리듬
  columnGap:     80,   // 2분할 레이아웃 기본 gap
  columnGapLg:   120,  // 여유 있는 2분할

  // Radii
  radiusSm:      16,
  radiusMd:      24,
  radiusLg:      32,
  radiusXl:      40,   // 이미지 프레임 기본값
  radiusFull:    9999,

  // Shadows
  shadowCard:    '0 20px 40px rgba(0,0,0,0.5)',
  shadowSubtle:  '0 8px 24px rgba(0,0,0,0.35)',
};
```

---

## 5. 컴포넌트 스펙 (Components)

### 5.1 `ImageFrame` — 라운드 이미지 프레임
```
border-radius:  40px         (SPACING.radiusXl)
overflow:       hidden
box-shadow:     SPACING.shadowCard
object-fit:     cover
```
용도: 인물 사진, 제품 사진, 큰 일러스트.

### 5.2 `CircularThumb` — 원형 썸네일
```
width:          240px
height:         240px
border-radius:  50%
object-fit:     cover
```
용도: 아젠다 리스트 좌측 인물 썸네일 스택, 팀 슬라이드.

### 5.3 `ArrowIcon` — 진행 화살표
```
SVG, stroke-width: 8px
color:  currentColor (기본 #FFFFFF)
transform: rotate(-45deg)   // 우상향 기본
```
용도: 스텝 구분자, "다음으로" 시그널. **accent 색으로 override 가능** (단 슬라이드당 1곳).

### 5.4 `BarChart` — 심플 데이터 바
```
bar_width:            80px
border_top_radius:    16px 16px 0 0
inactive_color:       #1C1C1E (bgSurface)
active_color:         #A855F7 (accentPurple)   // cyan/amber 로 대체 가능
gap_between_bars:     24px
```
라벨은 바 아래 `caption` 스타일.

### 5.5 `Card` — 일반 카드
```
background:     #1C1C1E
border-radius:  24px (radiusMd) 또는 32px (radiusLg)
padding:        48px ~ 64px
border:         none    // 좌측 accent border 금지
```

### 5.6 `Eyebrow` — 섹션/스텝 라벨
```
font:     TYPE_SCALE.eyebrow
color:    textSecondary 기본, 강조시 accent
margin-bottom: 24px
```

### 5.7 `NumberBadge` — 큰 숫자 인덱스
아젠다·스텝 번호용.
```
font:     TYPE_SCALE.section (56px, 600)
color:    accentAmber (기본) | accentPurple | accentCyan
margin-right: 32px
line-height: 1
```

---

## 6. 슬라이드 레이아웃 블루프린트 (Layouts)

각 레이아웃은 1920×1080 기준. padding 은 SPACING 토큰 사용.

### 6.1 `TitleSlide` — 표지
- 중앙 또는 좌하단 정렬
- `display` 또는 `title` 사이즈의 덱 이름
- 하단에 `caption` 으로 발표자 / 날짜
- 배경: `bgMain`. 선택적으로 우측에 accent 색 원형 블러(지름 800px, opacity 20%).

### 6.2 `SectionDivider` — 섹션 구분
- 좌측: 큰 `eyebrow` "01" + `title` 섹션명
- 우측: 비움 또는 `ImageFrame`
- 다음 섹션 디바이더와 **완벽히 동일한 구조** (숫자 + 텍스트 위치 고정)

### 6.3 `SplitProfile` — 2분할 (이미지 + 텍스트)
```
grid-template-columns: 4fr 6fr;
gap: 80px;
padding: 120px 80px;

left:  ImageFrame (높이 100%)
right: flex-column, justify-content: center
         - eyebrow
         - title
         - body (2~3줄)
```

### 6.4 `AgendaList` — 아젠다
```
grid-template-columns: 3fr 7fr;
gap: 80px;

left:  CircularThumb 의 수직 스택 (3~5개, 살짝 겹침)
right: list_container
         - list_item: flex-row, margin-bottom 48px
             - NumberBadge (amber)
             - subsection 제목 + caption 부연
```

### 6.5 `WorkflowSteps` — 3~4 스텝
```
display: flex; flex-direction: row; justify-content: space-between;
padding: 160px 80px;

step_container: width 22%, flex-column
  - step_number (section, color textSecondary)
  - step_divider (2px line, bgSurface, margin 24px 0)
  - step_title (section)
  - step_desc  (body)
```
스텝 사이에 선택적으로 `ArrowIcon` 배치.

### 6.6 `ComparisonBeforeAfter`
```
grid-template-columns: 1fr 1fr;
gap: 64px;

col_before:  flex-column, bg bgSurface, padding 64px, radius 32px
col_after:   flex-column, bg linear-gradient(bgSurface → accentPurple10), padding 64px, radius 32px
```
각 컬럼 상단에 `eyebrow` ("BEFORE" / "AFTER"), 그 아래 `subsection` 결론 한 줄, 본문.

### 6.7 `BigNumber` — 숫자 강조
- 중앙 정렬 `display` (160px) 로 숫자
- 숫자 색: `textPrimary` 기본, accent 강조 가능
- 아래 `section` 한 줄 설명 + `caption` 출처

### 6.8 `Quote`
- 좌측 정렬, `section` (56px) 큰 따옴표 + 인용
- 하단에 `caption` 으로 인용자 / 출처
- 배경은 `bgMain`, 좌측 상단 큰 `"` 글리프는 `accentPurple` 240px display

### 6.9 `FullBleedImage`
- 전면 이미지 (object-fit: cover)
- 하단 또는 상단에 하프톤 그라디언트 오버레이 (검정 → 투명)
- 오버레이 위 `title` + `caption`

### 6.10 `DataChart`
- 좌측 3fr: `title` + `body` 서술
- 우측 7fr: `BarChart` (5~7 막대)
- 축 라벨은 `caption`

### 6.11 `BridgeMapping` — 개념↔실습 다리 슬라이드
**용도**: 개념 챕터(Ch1~3)와 실습 챕터(Ch3.5) 사이에 둬서 청중 인지 부하를 낮춘다. "지금까지 배운 X 가 실습 파일 Y 로 들어간다" 매핑.

```
title:    "지금까지 배운 것이, 어디에 들어가나."
caption:  매핑이 어디로 향하는지 한 줄
recipe (3-card grid):
  card_n:
    cnum:   "Ch.0X — 키워드"        // accent 한 가지
    ctitle: 챕터의 핵심 개념 (subsection)
    csub:   한 줄 설명
    ex:     mono span — "→ FILENAME.md"
caption (하단): "이 매핑만 머리에 들어오면, 다음 슬라이드에서 X 가 갑자기 나와도 안 헷갈립니다."
```
**규칙**: 단일 accent (보통 amber 또는 직전 챕터 색). 카드 3장 이내. 각 카드의 `ex` 영역만 mono 허용 (Latin 파일명).

### 6.12 `LimitsTriple` — 한계·비용·검증 3카드
**용도**: 마무리 직전, 가능성만 보여주고 끝내지 않도록 한계와 검증 책임을 명시.

```
eyebrow: "잊지 말 것" (amber)
title:   "가능성만 봤습니다.\n한계도 봐야 합니다."
recipe (3-card):
  - 한계 1 (예: 할루시네이션) — 한 줄 진단 + 검증 액션
  - 한계 2 (예: 토큰 비용)  — 정량 데이터 + 적용 범위
  - 한계 3 (예: 검증 루틴)  — 5초 의심 / 손으로 한 번
caption: 책임을 강조하는 한 줄
```
**규칙**: 단일 accent amber. "할 수 있는 것" 슬라이드와 같은 시각 무게로 배치 — 약하게 처리하면 무시당함.

### 6.13 `TakeawaysList` — 액션 아이템 5
**용도**: Final Closing 직전. 청중이 "월요일 아침" 부터 따라할 수 있는 구체 액션.

```
eyebrow: "오늘 가져갈 다섯 가지" (purple 권장)
title:   시간·요일 명시 ("월요일 아침,\n이 다섯 줄로 시작하세요.")
bullets (5개):
  bullet_n:
    idx:  "0N"                 // mono, accent
    txt:
      <b>액션 동사 + 무엇</b>
      <span>한 줄 설명 — 챕터 출처</span>
```
**규칙**: 정확히 5개 (4개도 6개도 아님 — 손에 꼽힌다는 인지 효과). 각 bullet 끝에 챕터 출처 (`Ch.1`, `Ch.2` 등) 명시. 단일 accent.

---

## 7. 그리드 & 안전영역 (Safe Area)

- 기본 슬라이드 프레임: `padding: 100px 100px 80px 100px`
- 콘텐츠 실영역: **1720 × 900**
- 페이지 번호/풋터는 `bottom: 40px, right: 60px`, `caption` 스타일로. 모든 슬라이드 동일 위치.
- 2분할 레이아웃의 gap 은 `80px` 기본, 여백감 필요 시 `120px`.

---

## 8. 색상 사용 가이드

### accent 색 의미 (권장)
| 색상 | 심볼릭 용도 |
|---|---|
| `accentPurple` #A855F7 | **기본 강조** — 주제·핵심 데이터 |
| `accentCyan`   #06B6D4 | **정보·Before/데이터** 계열 (선택) |
| `accentAmber`  #F97316 | **경고·액션·번호 인덱스** |

### 규칙
- **한 슬라이드 = accent 1색.** 두 색을 섞지 마세요.
- **한 덱에서 accent 주 색은 1개**, 보조로 1개 더(숫자용)까지.
- 그라디언트는 `linear-gradient(bgSurface, accent10%)` 처럼 **지극히 은은하게**. 네온·과채도 금지.
- 텍스트는 99% 의 경우 흰색. 보조 설명만 `textSecondary`.

### 마무리 시퀀스 색상 흐름 (R-23 권장)

실습 → 마무리 시퀀스의 정서 흐름을 색으로 매핑:

| 슬라이드 | accent | 정서 |
|---|---|---|
| 후반부 진입 디바이더 ("조감도") | purple | 한 발짝 떨어짐 |
| 다음 단계 티저 (4-카드) | purple | 가능성 |
| 시리즈 요약 (Series Summary) | amber | 정리 / 회고 |
| **한계와 비용** (LimitsTriple) | amber | 책임 / 무게 |
| **5가지 액션 아이템** (TakeawaysList) | purple | 행동 / 출발 |
| 마치며 (Final Closing) | mixed (3색 절제 사용) | 종합 마무리 |

**원칙**: 한계 슬라이드 = amber (경고/책임 톤), 액션 슬라이드 = purple (출발/추진 톤). 한 슬라이드 안에서는 여전히 단일 accent 룰 유지.

---

## 9. 모션 & 트랜지션

- 슬라이드 간 전환: deck-stage 기본(페이드/스냅). 화려한 트랜지션 금지.
- 슬라이드 내부 진입 애니메이션은 사용자가 명시적으로 요청할 때만 추가.
- 기본 이징: `cubic-bezier(0.22, 1, 0.36, 1)` (easeOutExpo 느낌), duration 400ms.

---

## 10. 아이콘 & 이미지

- 아이콘: **Lucide** (line, stroke-width 1.5~2) 단일 시스템. 필요시 Heroicons outline. **이모지 금지.**
- 이미지가 없을 때: `ImageFrame` 대신 **모노톤 플레이스홀더** 사용
  ```
  background: #1C1C1E;
  border-radius: 40px;
  centered label: "IMAGE — caption" in textSecondary caption.
  ```
- 사진 톤: 가능하면 **저채도·그레인 있는 다큐멘터리 톤**. 광택 있는 스톡 이미지 회피.

---

## 11. 구현 체크리스트 (Do's & Don'ts)

> 이 섹션은 `lesson_learned.md` 의 누적 피드백을 반영합니다.

### Do
- ✅ `deck_stage.js` 로 덱 스캐폴드 생성
- ✅ `TYPE_SCALE`, `SPACING`, `COLORS` 를 `tokens.js` 에 한 번만 정의하고 모든 컴포넌트가 import
- ✅ **인라인 `font-size:` 는 토큰 값 6개만**: `{24, 28, 40, 56, 80, 96, 160}` px. 그 외 모두 금지
- ✅ 가능하면 클래스 (`.body`, `.caption`, `.subsection`, `.section-h`, `.title`, `.title-lg`) 우선
- ✅ 제목 문법 스타일 통일 (명사구 OR 서술문, 섞지 않기)
- ✅ 섹션 디바이더는 모든 섹션에서 동일 구조
- ✅ 페이지 번호·풋터는 모든 슬라이드 동일 위치
- ✅ 한글 본문은 `word-break: keep-all; text-wrap: pretty;`
- ✅ accent 강조: 한 슬라이드에서 eyebrow / 라벨 / 강조어 / 박스 모두 **같은 한 색**
- ✅ 도구별 키워드(`CLAUDE.md`, `GEMINI.md`, `.claude/`, `agents/`)는 직전 슬라이드의 컨텍스트와 정합 확인

### Don't
- ❌ `position: absolute` / `inset` 을 `<section>` 에 직접 부여 (deck-stage가 처리)
- ❌ 24px 미만 텍스트
- ❌ **한 슬라이드에 accent 색 2개 이상** (purple+cyan, amber+cyan, 3색 동시 등 모두 위반). 보조 차별화는 `text-secondary / text-tertiary` 회색만
- ❌ **좌측 accent border 카드** (`border-left:` 키워드 자체를 코드에서 검색해 0건이어야 함). 강조는 full border, 배경 그라디언트, 상단/하단 4px 띠 로
- ❌ **이모지 + 유니코드 dingbats**: ✏ ✓ ✗ ★ ⚡ 🚀 등 emoji-rendering 문자 모두 금지. 화살표는 `→ ↓ ↑` 같은 텍스트 글자로만
- ❌ **임의 폰트 크기** (22, 26, 30, 32, 34, 36, 44, 48 px 등)
- ❌ **eyebrow 에 시간/단계/속도** (`00:00–02:00`, `Act 1`, `Step 03` 모두 강사 진행표 — 청중에게 노출 X). eyebrow 는 의미 라벨만
- ❌ **horizontal flex 항목 5개 이상** (1720px content 영역 overflow). `.steps` 는 3–4개 한정
- ❌ 동일 레이아웃 3연속
- ❌ "It's not X. It's Y." / "마법 같은 순간" 류 카피
- ❌ **도구 컨텍스트 미스매치** — Antigravity 실습 슬라이드 옆에 Claude Code `.claude/` 폴더 표기, GEMINI.md 가 `.claude/` 안에 들어있는 식의 모순
- ❌ **한국어를 `var(--font-mono)` 로 렌더** — JetBrains Mono 는 Latin 전용 → 한국어가 시스템 fallback 으로 떨어져 폭/굵기 부조화. mono 는 파일명·경로·명령·영문 라벨에만, 한국어 섞이면 `<span>` 으로 분리
- ❌ **다이어그램을 mono 텍스트 행으로 표현** — `검색<br>↓<br>요약<br>↓` 식은 코드 블록. 다이어그램은 **pill 노드(`bg-elevated` rounded-14px padding 14×36) + 화살표 connector** 로

---

## 12. 부록 — 한국어 타이포 팁

- `font-family` 에 `Pretendard` 우선: `"Pretendard Variable", "Pretendard", Inter, ...`
- `letter-spacing` 은 한글에선 음수값을 **약하게만** (-0.01em 수준). 영문 기준 -0.02em 을 그대로 쓰면 한글이 붙어 보인다.
- 한글 본문은 `line-height: 1.65~1.7` 이 영문(1.6)보다 더 편하다.
- 긴 영문 고유명사(OpenAI, Anthropic 등) 가 한글과 섞이면 고유명사 앞뒤에 얇은 공백(space) 을 넣어 가독성을 높인다.

---

*이 문서가 프로젝트의 진실(source of truth)입니다. 디자인 결정에 의문이 들면 즉흥적으로 정하지 말고, 여기부터 고친 뒤 구현에 반영하세요.*
