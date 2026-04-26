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

### 해도 되는 것
- `design.md` 에 정의된 컬러 토큰, 타입 스케일, 스페이싱, 컴포넌트만 사용
- 강조가 필요할 때 **accent 3색** (`purple / cyan / amber`) 중 **슬라이드당 최대 1색**만 사용
- 실제 이미지가 없으면 **#1C1C1E 배경 + 모노톤 placeholder** 사용
- 한 덱 안에서 레이아웃 블루프린트는 **4~6개 내에서 반복** (리듬 확보)

### 하면 안 되는 것
- ❌ 새로운 색상 발명 (임의 hex 금지). 필요하면 `oklch()` 로 기존 팔레트 변형
- ❌ 이모지 사용 (브랜드에 없음)
- ❌ 24px 미만 본문, 48px 미만 제목
- ❌ `<section>` 에 직접 `position / inset / width / height` 지정 (deck-stage 가 처리)
- ❌ 그라디언트 남발, 좌측 accent border 카드, "It's not X. It's Y." 식 카피
- ❌ 이모지로 아이콘 대체
- ❌ 슬라이드 하나에 글머리표 5개 초과
- ❌ `styles` 라는 전역 이름 사용 (컴포넌트마다 고유 이름: `titleSlideStyles`, `agendaStyles` …)

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
5. 웹 느낌(카드 그림자 남발, rounded-2xl 만 사용 등)이 아니라 **프레젠테이션 느낌**인가?
6. 빈 공간이 두렵지 않은가? (아래쪽 여백은 의도된 것 — 채우지 마세요)

그 다음 `done(path)` → `fork_verifier_agent()` 순서로 마무리합니다.

---

## 8. 확장(Extend) 정책

사용자가 "이 템플릿으로 X 주제 덱을 만들어줘" 라고 하면:
- **새 HTML 파일을 만들되**, `design.md` 의 토큰/컴포넌트를 그대로 import/복제해 사용
- 디자인 시스템 변경은 `design.md` 에 **먼저** 반영하고, 그 뒤 덱에 적용
- 새 레이아웃이 필요하면 `design.md §6` 에 블루프린트를 추가한 뒤 구현
