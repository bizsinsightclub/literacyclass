# 템플릿 — Big-Statement (모티프 앵커)

> `Cheil_Design.md §8` 카테고리 #4. 핵심 개념어/메시지 1개를 크게 앵커. 결정 트리의 기본 폴백. 정렬: 좌측 또는 중앙.
> 토큰 값은 `Cheil_Design.md` §1·§7에서만 가져온다(지어내지 않음).

## 레이아웃 슬롯
```
┌──────────────────────────────────────────────┐
│  [EYEBROW]  (선택, 소형 라벨)                  │
│                                                │
│  [STATEMENT]   단일 개념어 / 메시지            │   ← 초대형 앵커
│                                                │
│  [AROUND]  주변 소형 텍스트 (선택)             │   ← 보조 설명
└──────────────────────────────────────────────┘
```

## 슬롯 ↔ 토큰 매핑 (placeholder)
| 슬롯 | 폰트 | 크기 pt | 컬러 | 비고 |
|---|---|---|---|---|
| EYEBROW | `{{head}}` Medium | `caption: 12` | `{{accent_mint}}` | 선택. 소형 라벨 |
| STATEMENT | `{{head}}` Light 또는 Bold | `display: 60`(또는 54) | `{{fg}}` | 단일 개념어. Head Light는 의도적 대형에만(§9.1) |
| AROUND | `{{body}}` Regular | `body: 16` | `{{fg}}` | 선택. 사실이면 `[S#]` |

토큰: `head="Samsung SS Head KR"` `body="Samsung SS Body KR"` `fg="#FFFFFF"` `bg="#000000"` `accent_mint="#99EADC"` (전부 `Cheil_Design.md §1·§7`)

## 작성 골격
```
배경: {{bg}}  / 텍스트: {{fg}}  / 앵커 강조: {{accent_mint}}(선택)

[EYEBROW]   : __________
[STATEMENT] : __핵심 개념어__              ← 사실 주장이면 [S#]
[AROUND]    : __보조 설명__                 ← 사실이면 [S#]
```
> 반복 앵커(모티프) 패턴 허용: 같은 STATEMENT를 여러 장표에 반복해 리듬을 만든다(예 "a Book"). 단, 매 장표 사실 슬롯은 각자 `[S#]`.

## 강제 체크
- [HVB] STATEMENT = Head 계열(>18pt). [FONT-KR] 한글이면 `*_KR`.
- [HEADLIGHT] Head Light는 '의도적 큰 헤드라인'에만(그 외 WARN). [HIER] STATEMENT vs AROUND 충분히 구별.
- [COLOR] §7 팔레트만, 강조 최소화. [TRACK] 자간/행간 임의 변형 금지(폰트 내장값).
- STATEMENT가 사실 주장이면 `[S#]` 필수(헌법 §3). 개념·슬로건이면 출처 불요하나 사실로 오인될 표현 금지.
