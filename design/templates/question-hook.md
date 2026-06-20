# 템플릿 — Question / Hook (질문 던지기)

> `Cheil_Design.md §8` 카테고리 #2. 큰 질문문 + `Q.` 마커로 독자에게 문제·질문을 제기. 정렬: 좌측 또는 중앙.
> 토큰 값은 `Cheil_Design.md` §1·§7에서만 가져온다(지어내지 않음).

## 레이아웃 슬롯
```
┌──────────────────────────────────────────────┐
│  [Q-MARK]  Q.                                  │   ← 질문 마커(소형)
│                                                │
│  [QUESTION]                                    │   ← 큰 질문문
│  [QUESTION 2line]                              │
│                                                │
│  [SUB]  보조 문구/단서 (선택)                  │   ← 부연
└──────────────────────────────────────────────┘
```

## 슬롯 ↔ 토큰 매핑 (placeholder)
| 슬롯 | 폰트 | 크기 pt | 컬러 | 비고 |
|---|---|---|---|---|
| Q-MARK | `{{head}}` Bold | `subhead: 24` | `{{accent_mint}}` | "Q." 마커. 강조 최소화 |
| QUESTION | `{{head}}` Medium~Bold | `display: 44` | `{{fg}}` | 1줄. 핵심 질문 |
| QUESTION 2line | `{{head}}` Regular~Medium | `subhead: 32` | `{{fg}}` | 2줄 이상이면 한 단계 ↓ + Regular(§3) |
| SUB | `{{body}}` Regular | `body: 18` | `{{fg}}` | 선택. 사실이면 `[S#]` |

토큰: `head="Samsung SS Head KR"` `body="Samsung SS Body KR"` `fg="#FFFFFF"` `bg="#000000"` `accent_mint="#99EADC"` (전부 `Cheil_Design.md §1·§7`)

## 작성 골격
```
배경: {{bg}}  / 텍스트: {{fg}}  / 마커 강조: {{accent_mint}}

[Q-MARK]   : Q.
[QUESTION] : __________________________?        ← 사실 전제면 [S#]
[SUB]      : __________                          ← 사실이면 [S#]
```

## 강제 체크
- [HVB] Q-MARK/QUESTION = Head 계열(>18pt). [FONT-KR] 한글이면 `*_KR`.
- [SUBHEAD] 질문이 2줄 이상이면 Medium→Regular 전환. [HIER] 질문 vs 보조 충분히 구별.
- [COLOR] §7 팔레트만, 강조 최소화. [TRACK] 자간/행간 임의 변형 금지(폰트 내장값).
- 질문이 사실을 전제하면 그 사실에 `[S#]`(헌법 §3). 미확인 전제는 `확인되지 않음`.
