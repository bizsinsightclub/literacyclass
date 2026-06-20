# Cheil_Design.md — Samsung SS 타이포그래픽 디자인 플레이북

> 이 문서는 `Re-Cheil` 0 할루시네이션 파이프라인의 **디자인 강제 규칙**이다.
> Documenter 단계의 모든 시각 산출물(`output/report.md`, `output/deck/`)과 `deck-builder` 스킬은
> 이 문서의 토큰·규칙을 **엄격히** 따르며, 위반 시 차단한다(`CLAUDE.md` §7, `IMPLEMENTATION_PLAN.md` Phase 6).
> 이 문서는 **자기완결적**이다 — 외부 PDF를 다시 열지 않고도 이 md만으로 전 규칙을 적용·강제할 수 있다.

---

## 0. 사용법 (에이전트가 이 문서를 소비하는 법)

- **먼저 §1 Quick Reference 블록을 로드**하라. 폰트·컬러·크기 핵심 토큰이 1곳에 있다.
- 적용 시 **결정 규칙(if/then)**과 **체크리스트(§10)**를 그대로 분기 로직으로 사용하라.
- 모든 단정에는 출처 태그가 붙는다. **권위 등급**은 다음과 같다:
  - `[PB]` = Samsung 공식 타이포그래픽 플레이북 → **최고 권위, 변경 불가**.
  - `[USER]` = 사용자 지시 → 플레이북 공백을 메우는 결정.
  - `[DECK]` = 레퍼런스 제안 덱(`260611_Next Foldable…`) 관찰값 → 플레이북에 규칙이 없을 때 패턴 근거.
- 우선순위: **`[PB]` > `[USER]` > `[DECK]`**. 충돌 시 상위 등급을 따른다.
- **기본 톤 = 검정 배경(`#000000`) + 흰 텍스트(`#FFFFFF`), 강조 최소(모노톤)**. 레퍼런스 덱(다크 `#0D0D0D`)과 같은 다크 계열을 **순검정(#000000)** 으로 통일하고, 레퍼런스의 '극소 강조'(민트 42p 중 3회) 원칙을 유지한다(§7). 추후 공식 hex 입수 시 값만 교체.
- **슬라이드 캔버스 = 960×540pt(= PowerPoint 와이드스크린)**. 좌표는 §8.5 기하 규격을 따른다 — 이 규격이 HTML 미리보기와 네이티브 PPTX의 1:1 무손실 변환을 보장한다.

---

## 1. Quick Reference (핵심 토큰 — 단일 블록)

```yaml
# Cheil / Samsung SS Design Tokens — v1.0
fonts:
  display_headline: { family: "Samsung SS Head", weight: Bold }      # [PB]
  subhead_1line:    { family: "Samsung SS Head", weight: Medium }    # [PB]
  subhead_2line+:   { family: "Samsung SS Head", weight: Regular }   # [PB]
  body:             { family: "Samsung SS Body", weight: Regular }   # [PB]
  body_emphasis:    { family: "Samsung SS Body", weight: Bold }      # [PB]
  disclaimer:       { family: "Samsung SS Body", weight: Light }     # [PB]  min 7pt
  headline_excpt:   { family: "Samsung SS Head", weight: Light }     # [PB]  의도적 큰 헤드라인만
  korean_head:      { family: "Samsung SS Head KR" }                 # [PB]  한글
  korean_body:      { family: "Samsung SS Body KR" }                 # [PB]  한글
  literary_quote:   { family: "ChosunilboNM", official: false }      # [DECK] 문학 인용 한정(세리프)

size_pt:            # [DECK] 관찰 스케일 (1:1 기준, CLAUDE 기준 pt)
  display: [96, 60, 54, 44]
  subhead: [36, 32, 28, 24]
  body:    [18, 16, 14]
  caption: [12, 11, 10]
  disclaimer_min: 7        # [PB] 7pt 미만 금지
  graphic_oversize: 199    # [DECK] 섹션 번호 등 장식 전용

rule_head_vs_body_threshold_pt: 18   # [PB] >18pt→Head, <=18pt 또는 텍스트과밀→Body

letterspacing: font-default   # [PB][DECK] 폰트 내장값 사용, 임의 트래킹 금지
linespacing:   font-default   # [DECK] 단락 행간 오버라이드 미사용(단일 행간)

align: { primary: [left, center], rare: [right, bottom] }   # [DECK]

colors:             # [USER 결정] 기본 톤 = 검정 배경 + 흰 텍스트 (모노톤 최소)
  bg:        "#000000"   # 기본 배경(순검정)
  fg:        "#FFFFFF"   # 기본 텍스트(흰색)
  ink:       "#FFFFFF"   # 헤드라인·최대대비(흰색)
  muted:     "#A6A6A6"   # 보조 텍스트(밝은 회색)        [DECK]
  faint:     "#777777"   # 캡션·키라인(어두운 회색)      [DECK]
  zone:      "#1A1A1A"   # 영역 구획용 어두운 패널        [USER]
  accent:    "#99EADC"   # 유일 포인트(민트) — 필/키라인 한정, 극소량 [DECK]
  # 비활성(레퍼런스 본문 미사용): blue #1357F6 / paleblue #C8D1FF / amber #FFC000
```

---

## 2. 폰트 패밀리

### 2.1 패밀리 & 웨이트 `[PB]`
| 패밀리 | 용도 | 웨이트 | 디자인 특징 |
|---|---|---|---|
| **Samsung SS Head** | 제목·헤드라인 (큰 사이즈) | Bold / Medium / Regular / Light (4종) | high x-height, 기하학적 형태, tight spacing → 임팩트 있는 큰 텍스트 |
| **Samsung SS Body** | 본문·작은 글씨·긴 글 | Bold / Regular / Light (3종) | regular x-height, 긴 어센더, 비모호 글자꼴(2층 `a`, 곡선 `l`) → 작은 사이즈 가독성 |

> Head·Body는 동일한 디자인 정신으로 조화되도록 설계되었으나, 각자 최적 용도가 다르다. `[PB]`

### 2.2 한글 패밀리 `[PB]` (저장소 실제 보유 파일과 1:1)
| 한글 패밀리 | 웨이트 / 파일명 |
|---|---|
| **Samsung SS Head KR** | `SamsungSSHeadKR-Light` / `-Regular` / `-Medium` / `-Bold` |
| **Samsung SS Body KR** | `SamsungSSBodyKR-Light` / `-Regular` / `-Bold` |

### 2.3 폰트 화이트리스트 (이 목록 외 사용 금지 — §10에서 강제)
```
# 라틴 (제목)
SamsungSSHead-Light  SamsungSSHead-Regular  SamsungSSHead-Medium  SamsungSSHead-Bold
# 라틴 (본문)
SamsungSSBody-Light  SamsungSSBody-Regular  SamsungSSBody-Bold
# 한글 (제목)
SamsungSSHeadKR-Light  SamsungSSHeadKR-Regular  SamsungSSHeadKR-Medium  SamsungSSHeadKR-Bold
# 한글 (본문)
SamsungSSBodyKR-Light  SamsungSSBodyKR-Regular  SamsungSSBodyKR-Bold
# 편집적 예외(문학 인용 한정, 비공식): ChosunilboNM
```

### 2.4 포맷 선택 가이드 `[PB]`
| 포맷 | 용도 | 권장 환경 |
|---|---|---|
| `.ttf` | 일반 문서 | MS Office (Word/PowerPoint/Excel) — 가벼움, 빠른 처리 |
| `.otf` | 그래픽·고해상도 출력 | Adobe Suite (InDesign/Illustrator/Photoshop) — 정교한 곡선 |
| `WOFF2 / WOFF / EOT` | 웹폰트 전용 | 웹 개발 (개발 직무만 설치 권장) |

> 한 OS에 `.ttf`/`.otf` 중 **하나만** 설치 가능(둘 다 설치 시 교체 요구). `[PB]`

### 2.5 폰트 파일 위치 & 웹 `@font-face` 스니펫
모든 폰트는 `design/fonts/` 아래 포맷별로 평탄화되어 있다(Latin·KR 동일 폴더, 파일명으로 구분):
- `design/fonts/ttf/` — Office(PowerPoint 등) 14파일
- `design/fonts/otf/` — Adobe(InDesign 등) 14파일
- `design/fonts/woff2/` — 웹/HTML 14파일

```css
/* 경로는 프로젝트 루트 기준. KR은 동일 폴더의 *KR-*.woff2 사용. */
@font-face { font-family:"Samsung SS Head"; font-weight:300;
  src:url("design/fonts/woff2/SamsungSSHead-Light.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Head"; font-weight:400;
  src:url("design/fonts/woff2/SamsungSSHead-Regular.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Head"; font-weight:500;
  src:url("design/fonts/woff2/SamsungSSHead-Medium.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Head"; font-weight:700;
  src:url("design/fonts/woff2/SamsungSSHead-Bold.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Body"; font-weight:300;
  src:url("design/fonts/woff2/SamsungSSBody-Light.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Body"; font-weight:400;
  src:url("design/fonts/woff2/SamsungSSBody-Regular.woff2") format("woff2"); }
@font-face { font-family:"Samsung SS Body"; font-weight:700;
  src:url("design/fonts/woff2/SamsungSSBody-Bold.woff2") format("woff2"); }
/* 한글: design/fonts/woff2/SamsungSSHeadKR-*.woff2, SamsungSSBodyKR-*.woff2 (동일 패턴) */
```

---

## 3. 타입 위계 (강제 규칙) `[PB]`

### 3.1 역할별 폰트/웨이트
| 역할 | 폰트 / 웨이트 | 규칙·예외 |
|---|---|---|
| Headline (제목) | Samsung SS Head **Bold** | 18pt 초과 권장. 가장 크고 굵게 |
| Sub-headline (부제, 1줄) | Samsung SS Head **Medium** | 권장 기본 |
| Sub-headline (부제, 2줄 이상) | Samsung SS Head **Regular** | **2줄 넘어가면 Medium→Regular 전환** |
| Body text (본문) | Samsung SS Body **Regular** | 18pt 이하 / 텍스트 과밀 환경 |
| 강조 본문 | Samsung SS Body **Bold** | 본문 중 강조에 한정 |
| Disclaimer (면책조항) | Samsung SS Body **Light** | **7pt 미만 금지** |
| 예외 헤드라인 | Samsung SS Head **Light** | "의도적 디자인 목적"의 큰 헤드라인만 허용 (남용 금지) |

### 3.2 결정 규칙 (if/then — 에이전트 분기용)
```
# 폰트 패밀리 선택
if size_pt > 18 OR role in {headline, subhead}:  family = "Samsung SS Head"
elif size_pt <= 18 OR text_heavy:                family = "Samsung SS Body"

# 부제 웨이트
if role == subhead and lines >= 2:  weight = Regular   # else Medium

# 면책/디스클레이머
if role == disclaimer:  family="Samsung SS Body"; weight=Light; assert size_pt >= 7

# 한글 텍스트면 *_KR 패밀리로 치환 (Head→Head KR, Body→Body KR)
```

### 3.3 위계 원칙
- 정보 위계 = **크기 위계 + 웨이트 위계**. 가장 중요한 정보가 가장 크고 굵게.
- 헤드라인과 부제는 **충분히 구별되는 크기·웨이트**로 설정한다.
  - ✅ 올바름: 헤드라인 Bold·대형 + 부제 Medium·소형 (대비 뚜렷).
  - ❌ 잘못됨: 헤드라인·부제의 크기와 웨이트가 너무 비슷 (위계 붕괴).
- 크기 권장값은 **1:1 스케일 기준**. 빌보드·대형 영상 등은 **비례 확대** 적용.

---

## 4. 크기 스케일 `[DECK]` (+ §3 임계 `[PB]`) — 960×540pt 캔버스 실측

> 레퍼런스 덱(42p)을 PyMuPDF로 span 단위 실측한 값. 캔버스가 PPT 와이드스크린과 동일하므로 **pt 값을 그대로 PPTX·HTML에 사용**한다.

| 계층 | pt 토큰 | 용도 | 실측 빈도 |
|---|---|---|---|
| Display | **96(커버 히어로) / 60 / 54 / 44** | 커버 헤드라인, 핵심 메시지, 모티프 앵커 | 60·54·44 다수 |
| Subhead | **36 / 32 / 28 / 24 / 20** | 소제목, 부제 | 32·28·24·20 |
| Body | **18 / 16 / 14 / 12** | 본문 | 16(최빈)·18·14·12 |
| Caption | **11 / 10 / 9** | 캡션, 출처 표기 | 9 |
| Disclaimer | **8** (하한 **7** `[PB]`) | 면책조항 — **7pt 미만 금지** | 8(최빈) |
| Graphic | **≈199** | 섹션 번호 등 장식 전용 (본문 텍스트 아님) | 섹션 디바이더 |

- **정렬**: 좌측·중앙 정렬을 기본(지배 패턴). 우측·하단 정렬은 의도적 소수. `[DECK]`
- 위 토큰은 **960×540pt 캔버스(=PPT 와이드스크린) 1:1 실측**. 다른 캔버스는 비례 환산.

---

## 5. 자간·줄간격

- `[PB]` 공식 플레이북은 **수치형 자간(tracking)/줄간격(leading) 값을 정의하지 않는다**. 정성 규칙만 존재(Head = tight spacing).
- `[DECK]` 레퍼런스 덱은 **단락 행간/자간 오버라이드를 거의 사용하지 않는다** (lnSpc/spcBef/spcAft 미설정, 자간 오버라이드 단 1건=5pt). → **폰트 내장 메트릭(단일 행간, 기본 커닝)에 의존**.
- **규칙**: Samsung SS **폰트 기본 메트릭을 그대로 사용**한다. **임의 트래킹/행간 변형 금지.**
- 수치 자체는 `확인되지 않음(폰트 내장값)` — 지어내지 않는다.

---

## 6. (예약) — 본 섹션 없음

> 컬러는 §7, 레이아웃은 §8 참조.

---

## 7. 컬러 (정식 토큰) `[USER 결정]` — 검정 배경 · 모노톤 최소

- **기본 톤: 순검정 배경(`#000000`) + 흰 텍스트(`#FFFFFF`).** `[USER]`
- 레퍼런스 덱 실측: 배경 `#0D0D0D`(31/42p) 다크, 텍스트 `#000000`·`#FFFFFF`·`#1F1F1F`, 회색 `#595959`·`#A6A6A6`, **유일 강조 민트 `#99EADC`는 텍스트로 단 3회**. → 레퍼런스와 같은 다크 계열을 **순검정(#000000)** 으로 통일하고 '극소 강조' 철학을 유지.
- 원칙: **색으로 강조하지 않는다. 위계는 크기·웨이트·여백으로 만든다.** 컬러는 흑/백/회 + 포인트 1색만.

### 7.1 팔레트
| 토큰 | hex | 역할 | 근거 |
|---|---|---|---|
| `--color-bg` | `#000000` | 기본 배경(순검정) | `[USER]` |
| `--color-fg` | `#FFFFFF` | 기본 텍스트(흰색) | `[USER]`·덱 148회 |
| `--color-ink` | `#FFFFFF` | 헤드라인·최대대비(흰색) | 덱 |
| `--color-muted` | `#A6A6A6` | 보조 텍스트(밝은 회색) | `[DECK]` |
| `--color-faint` | `#777777` | 캡션·키라인·구분선(어두운 회색) | `[DECK]` |
| `--color-zone` | `#1A1A1A` | 영역 구획용 어두운 패널(분할 레이아웃) | `[USER]` |
| `--color-accent` | `#99EADC` | **유일 포인트(민트)** — 검정 위 대비 우수, 극소량 | `[DECK]` 3회 |

> **비활성 토큰**(레퍼런스 본문 미사용 — 사용 금지, 기록만): blue `#1357F6`, paleblue `#C8D1FF`, amber `#FFC000`.

### 7.2 CSS 변수 블록
```css
:root {
  --color-bg:     #000000;
  --color-fg:     #FFFFFF;
  --color-ink:    #FFFFFF;
  --color-muted:  #A6A6A6;
  --color-faint:  #777777;
  --color-zone:   #1A1A1A;
  --color-accent: #99EADC;   /* 포인트 1색 — 극소량 */
}
```

### 7.3 조합 가이드
- 기본 화면: `--color-bg`(검정 배경) + `--color-fg`(흰 텍스트). 헤드라인도 흰색(`--color-ink`).
- 보조 정보: `--color-muted`, 캡션·구분선: `--color-faint`.
- 분할(좌 텍스트 / 우 데이터 등) 레이아웃의 구획 패널은 `--color-zone`(어두운 회색)으로 — 순검정 위 미세 단차.
- **강조색(`--color-accent`)은 한 화면에 한 곳, 작은 필/언더라인/키라인에만.** 검정 위 민트는 대비가 좋으나 과용 금지.
- 팔레트 외 hex 사용은 §10 `[COLOR]`에서 경고. 공식 브랜드 hex 입수 시 토큰 **값만 교체**(구조 불변).

---

## 8. 레이아웃 카테고리 `[DECK]`

레퍼런스 덱(42 slides) 관찰에서 도출한 선호 패턴. 어떤 콘텐츠가 어떤 레이아웃으로 가는지의 기본 매핑.

| # | 카테고리 | 용도 | 구성 | 권장 폰트/크기·정렬 | 예시 슬라이드 |
|---|---|---|---|---|---|
| 1 | **Cover** | 표지/타이틀 | 초대형 헤드라인 + 최소 요소, 이미지 없음 | Head Bold/Light, 96pt, 좌/중앙 | S01 |
| 2 | **Question / Hook** | 질문 던지기 | 큰 질문문 + `Q.` 마커 | Head Medium~Bold, 32–44pt | S02·S06·S09 |
| 3 | **Section Divider** | 섹션 전환 | 섹션명 + 초대형 번호(≈199pt) + 단계 라벨 | Head, 번호 199pt | S20·S30·S36 |
| 4 | **Big-Statement (모티프 앵커)** | 핵심 개념 강조 | 단일 개념어 54–60pt + 주변 소형 텍스트, 반복 앵커("a Book") | Head Light/Bold, 54–60pt | S04·S05·S13–17 |
| 5 | **Data / Stat** | 데이터 제시 | 대형 수치(예 70%·63%·37B) + **소형 출처 표기** | 수치 54pt + 출처 캡션 10–12pt | S07·S08·S18 |
| 6 | **Image-grid / 비주얼** | 사례·인물·제품 | 다중 이미지 + 라벨 | Body/Head 캡션 14–24pt | S24·S27·S37 |
| 7 | **Literary / Quote** | 문학·감성 인용 | 한글 문학 인용을 **세리프(ChosunilboNM)**로, 구조 텍스트(Head)와 대비 | ChosunilboNM, 본문~44pt | S25·S37 |

> **Data/Stat의 출처 표기 패턴**(예: "Deloitte / Digital Consumer Trends 2026")은 본 프로젝트의 `[S#]` 출처 강제 철학과 정합한다 — 모든 수치에 출처를 병기하라.

---

## 8.5 슬라이드 기하 규격 (HTML ↔ PPTX 1:1 매핑) — 신설·핵심

> 이 규격이 **HTML 웹슬라이드**와 **네이티브 편집형 PPTX**를 같은 좌표/토큰에서 파생시켜 "포맷 파괴 없음"을 보장한다. Documenter·deck-builder·`/api/export/pptx`가 모두 이 규격을 단일 근거로 쓴다.

### 8.5.1 캔버스 · 단위
- **캔버스 = 960 × 540 pt** (= 16:9, PowerPoint 와이드스크린 13.333"×7.5"와 정확히 동일). 레퍼런스 덱과 동일 좌표계. `[DECK]`
- **단위 = pt**. 변환: HTML 렌더 `1pt = 1px`(스테이지 960×540px, CSS `transform: scale()`로 뷰포트 맞춤) / PPTX `1pt = 12700 EMU`(python-pptx `Pt(v)`) → 무손실.
- 원점 = 좌상단 (0,0), x→우, y→하.

### 8.5.2 세이프 에어리어 · 그리드 `[DECK]`
- 마진: 좌우 **96pt**, 상하 **54pt**. 콘텐츠는 마진 안에.
- 12열 그리드: 열폭 = (960 − 96·2) / 12 = **64pt**, 거터 포함 환산은 자유. 분할 레이아웃은 좌/우 480pt 1:1 기본.

### 8.5.3 공통 요소 좌표
| 요소 | x | y | 폰트/크기 | 색 | 정렬 |
|---|---|---|---|---|---|
| 상단 브레드크럼 | 96 | 28 | Body Regular 12 | muted | 좌 |
| 좌상단 키커(Cover/섹션) | 96 | 72 | Head Medium 16–18 | accent 또는 muted | 좌 |
| 푸터 페이지번호 | 480(중앙) | 512 | Body Regular 10 | faint | 중앙 |
| 로고(Cheil Worldwide) | 864 | 502 | 이미지 `assets/cheil-logo.png`(흰색·투명, `data-fit="contain"`) w≈58 h≈18 | — | 우하단(여백 우38·하20) |

### 8.5.4 슬라이드 HTML 문법 (`data-*` 계약 — 자유형 마크업 금지)
모든 슬라이드는 `<section class="slide">` 1개 = 1장. 모든 요소는 `.el` + `data-*` 기하/토큰을 **반드시** 가진다(없으면 §10 `[GEOM]` BLOCK). export 파서가 이 속성만 읽어 PPTX 네이티브 요소로 재구성한다.

```html
<section class="slide" data-w="960" data-h="540" data-bg="#000000">
  <!-- 텍스트: PPTX TextBox -->
  <div class="el" data-type="text"
       data-x="96" data-y="300" data-w="600" data-h="120"
       data-font="Samsung SS Head" data-weight="700" data-size="60"
       data-color="#FFFFFF" data-align="left" data-valign="top">More Humanity</div>

  <!-- 도형/영역 패널: PPTX AutoShape(rectangle) -->
  <div class="el" data-type="shape" data-shape="rect"
       data-x="480" data-y="0" data-w="480" data-h="540" data-fill="#1A1A1A"></div>

  <!-- 이미지: PPTX Picture (assets/ 상대경로). 로고는 data-fit="contain" -->
  <img class="el" data-type="image"
       data-x="540" data-y="0" data-w="420" data-h="540" data-fit="cover"
       src="assets/book.jpg">

  <!-- 키라인: PPTX Connector(line) -->
  <div class="el" data-type="line"
       data-x="96" data-y="120" data-w="768" data-h="0" data-stroke="#777777" data-weight-pt="1"></div>
</section>
```

| data-* | 적용 | 비고 |
|---|---|---|
| `data-type` | 공통 | `text` / `image` / `shape` / `line` |
| `data-x,y,w,h` | 공통 | pt. 캔버스(0–960, 0–540) 안이어야 함 |
| `data-font,weight,size` | text | 폰트=화이트리스트(§2.3), size=§4 토큰, weight=100/300/400/500/700 |
| `data-color` | text | §7 팔레트 토큰 hex만 |
| `data-align,valign` | text | left/center/right · top/middle/bottom |
| `data-shape,fill` | shape | rect 기본, fill=§7 토큰(주로 zone) |
| `data-fit` | image | cover/contain (PPTX는 crop/fit 매핑) |
| `data-stroke,weight-pt` | line | 색=faint 기본, 두께 pt |

### 8.5.5 카테고리별 박스 좌표(기본 골격 — 7카테고리)
| 카테고리 | 핵심 박스(x,y,w,h pt) | 폰트/크기 |
|---|---|---|
| Cover | 키커(96,72,600,28) · 히어로(96,250,768,120) · 서브(96,372,768,48) · 날짜(96,470,400,24) | Head Bold 96/Head Medium 24 |
| Question/Hook | 키커(96,72) · 질문(96,200,768,200) | Head Medium 44 |
| Section Divider | 번호(96,120,300,300, Graphic 199) · 라벨(중앙) · 목업(중앙) | Head |
| Big-Statement | 리드(96,250,600,60) · 앵커(96,330,760,140 Bold) | Head Regular 24 + Head Bold 60 |
| Data/Stat | 좌 텍스트(96,260,560,220) · 우 zone패널(560,0,400,540) · 수치행 반복(각 수치 60 + 설명 16 + 출처캡션 9 faint) | Head Bold 60 / Body 16 / 9 |
| Image-grid | 이미지 셀 다중(grid) + 라벨(셀 하단 14–24) | Body/Head 14–24 |
| Literary/Quote | 타이틀(중앙 상) · 인용(중앙, **ChosunilboNM** 32–44) · 이미지(중앙) · 하단 세리프 1줄 | ChosunilboNM |

---

## 9. 편집적 예외 & 금지

### 9.1 편집적 예외 (제한적 허용)
- **세리프 문학 인용**: 한글 문학/감성 인용에 한해 `ChosunilboNM`(세리프) 사용 가능 — **비공식 폰트, 문학 인용 한정**. 구조·정보 텍스트는 반드시 Samsung SS.
- **Head Light 헤드라인**: "의도적 디자인 목적"의 큰 헤드라인에만. `[PB]`

### 9.2 금지 사항
- 폰트 화이트리스트(§2.3) 외 폰트로 본문/제목 조판 금지(문학 인용 예외 제외).
- 임의 자간/행간 변형 금지(§5). 폰트 내장 메트릭 사용.
- Disclaimer/본문 7pt 미만 금지(§3).
- §7 팔레트 외 컬러 사용 금지(경고 대상).
- Head Light 남용 금지(예외 용도 외 헤드라인 사용 금지).
- 18pt 분기 위반 금지(큰 텍스트에 Body, 작은 텍스트에 Head 금지).
- 헤드라인·부제 크기/웨이트 과유사로 위계 붕괴 금지.

---

## 10. 강제 체크리스트 (machine-checkable) — deck-builder / Documenter 차단 규칙

```
[FONT]     모든 텍스트 font-family ∈ 화이트리스트(§2.3)         → 아니면 BLOCK
[FONT-KR]  한글 텍스트는 *_KR 패밀리 사용                        → 아니면 WARN
[SIZE-MIN] 모든 본문/면책 size_pt >= 7                          → 미만이면 BLOCK
[HVB]      size_pt > 18 → Head 계열 / <= 18 → Body 계열         → 위반이면 WARN
[SUBHEAD]  부제 2줄 이상이면 Head Regular(Medium 아님)           → 위반이면 WARN
[HEADLIGHT] Head Light는 '의도적 큰 헤드라인'에만               → 그 외면 WARN
[COLOR]    모든 색상 ∈ §7 팔레트(bg/fg/ink/muted/faint/zone/accent) → 외부 hex면 WARN
[ACCENT]   accent(민트)는 텍스트색 사용 금지·화면당 1곳 이하     → 위반이면 WARN
[BG]       슬라이드 배경 = #000000 (data-bg)                     → 다르면 WARN
[TRACK]    자간/행간 임의 오버라이드 없음(폰트 기본)             → 오버라이드면 WARN
[HIER]     헤드라인 vs 부제 크기·웨이트 충분히 구별              → 과유사면 WARN
[STAT-SRC] Data/Stat 레이아웃의 모든 수치에 출처 캡션 병기       → 누락이면 BLOCK
[GEOM]     모든 .el 이 data-type·data-x/y/w/h 보유(§8.5)        → 누락이면 BLOCK
[CANVAS]   캔버스 960×540pt, 모든 박스가 캔버스 내(여백 96/54)   → 이탈이면 WARN
```

---

## 11. 출처 (References)

| 태그 | 소스 파일 (모두 `design/references/` 하위) | 제공 규칙 |
|---|---|---|
| `[PB]` | `references/SamsungTypographicPlaybook_2025.pdf` (27p, 영문) | 폰트 패밀리·웨이트(§2), 위계·18pt 임계·7pt 하한·Head Light 예외(§3), 포맷 가이드(§2.4) |
| `[PB]` | `references/브랜드서체_업데이트_공지_GBC_251217.pdf` (국문) | 7종 용도 매핑(§3), As-is/To-be(Sharp Sans→SS Head, Samsung One→SS Body), 포맷 용도 |
| `[PB]` | `references/SamsungSSKorean_QuickGuide.pdf` | 한글 패밀리 7종(§2.2), 설치/포맷 |
| `[DECK]` | `references/reference-deck/260611_Next Foldable_Answer Company_F.pptx` (42 slides) | 컬러 팔레트(§7), 크기 스케일(§4), 자간/행간 양상(§5), 레이아웃 카테고리(§8) |
| `[USER]` | 사용자 지시 | **검정 배경(#000000)/흰 텍스트** 기본 + 모노톤 최소 강조(§7), 우하단 Cheil 로고 이미지(§8.5.3), 슬라이드 캔버스 960×540pt(§8.5) |
| `[DECK실측]` | `reference-deck/...F.pdf` PyMuPDF span 실측 | 폰트/크기/색/배경 빈도(§4·§7), 캔버스 960×540pt(§8.5) |

### 미확정 / 추후 교체
- **공식 브랜드 컬러 hex**: 현재 §7은 덱 관찰값을 정식 토큰으로 사용. 공식 가이드 입수 시 토큰 값만 교체.
- **수치형 자간/줄간격**: 폰트 내장값. 공식 수치 미정의 → 임의 설정 금지.
