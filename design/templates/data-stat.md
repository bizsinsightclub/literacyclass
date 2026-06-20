# 템플릿 — Data / Stat (데이터 제시)

> `Cheil_Design.md §8` 카테고리 #5. 대형 수치 + **소형 출처 캡션**.
> `[STAT-SRC]`(§10): **모든 수치에 출처 캡션 `[S#]` 병기 — 누락 시 BLOCK**.

## 레이아웃 슬롯
```
┌──────────────────────────────────────────────┐
│  [LABEL]  지표명 (소형)                        │   ← 무엇의 수치인가
│                                                │
│      [STAT]  70%        [STAT]  37B            │   ← 대형 수치(1~3개)
│      [STAT-CAPTION]     [STAT-CAPTION]         │   ← 각 수치 바로 아래 출처+의미
│                                                │
│  [NOTE]  부연/정의 (선택)                      │
│  [SOURCE]  출처 목록 [S#]                      │   ← 하단 출처 캡션(필수)
└──────────────────────────────────────────────┘
```

## 슬롯 ↔ 토큰 매핑 (placeholder)
| 슬롯 | 폰트 | 크기 pt | 컬러 | 비고 |
|---|---|---|---|---|
| LABEL | `{{head}}` Medium | `subhead: 24` | `{{fg}}` | 지표명 |
| STAT | `{{head}}` Bold | `display: 54` | `{{accent_mint}}` | 수치 1개당 1슬롯. 강조색 최소화 |
| STAT-CAPTION | `{{body}}` Regular | `caption: 11` | `{{fg}}` | 수치 의미 + `[S#]` |
| NOTE | `{{body}}` Regular | `body: 14` | `{{fg}}` | 선택. 정의/단위 |
| SOURCE | `{{body}}` Light | `caption: 10` (≥7) | `{{fg}}` | 출처 캡션. 면책/출처 하한 7pt |

토큰: `head="Samsung SS Head KR"` `body="Samsung SS Body KR"` `fg="#FFFFFF"` `bg="#000000"` `accent_mint="#99EADC"` (전부 `Cheil_Design.md §1·§7`)

## 작성 골격
```
배경: {{bg}}  / 텍스트: {{fg}}  / 수치 강조: {{accent_mint}}

[LABEL]         : __________
[STAT] #1       : __값__        ← 반드시 [S#]
[STAT-CAPTION]  : __의미__  [S#]
[STAT] #2       : __값__        ← 반드시 [S#]
[STAT-CAPTION]  : __의미__  [S#]
[NOTE]          : 단위/정의 __  [S#]
[SOURCE]        : [S1] 발행처/문서/페이지 · [S3] ...
```

## 강제 체크
- **[STAT-SRC] 모든 STAT/수치에 `[S#]` — 하나라도 누락 시 BLOCK**(헌법 §3, §10).
- [SIZE-MIN] SOURCE/면책 size_pt ≥ 7. [HVB] STAT/LABEL = Head(>18pt), 캡션 = Body.
- [COLOR] §7 팔레트만, 강조 최소화. [TRACK] 자간/행간 임의 변형 금지.
- 소스 충돌 수치는 임의 선택·평균 금지 — 양쪽 값·출처 병기(헌법 §6).
- 미확인 수치는 `확인되지 않음(unknown)`. 사전지식·추정 금지(헌법 §2).
