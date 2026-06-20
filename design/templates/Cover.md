# 템플릿 — Cover (표지/타이틀)

> `Cheil_Design.md §8` 카테고리 #1. 초대형 헤드라인 + 최소 요소, 이미지 없음. 정렬: 좌측 또는 중앙.
> 토큰 값은 `Cheil_Design.md` §1·§7에서만 가져온다(지어내지 않음).

## 레이아웃 슬롯
```
┌──────────────────────────────────────────────┐
│  [EYEBROW]   (선택, 소형 라벨)                 │   ← 상단 라벨
│                                                │
│  [HEADLINE]                                    │   ← 초대형 핵심 메시지
│  [HEADLINE 2line]                              │
│                                                │
│  [SUBHEAD]   (선택)                            │   ← 부제 1줄
│                                                │
│  [META] 발행처 · 날짜 · 범위                   │   ← 하단 메타
└──────────────────────────────────────────────┘
```

## 슬롯 ↔ 토큰 매핑 (placeholder)
| 슬롯 | 폰트 | 크기 pt | 컬러 | 비고 |
|---|---|---|---|---|
| EYEBROW | `{{head}}` Medium | `caption: 12` | `{{accent_mint}}` | 선택. 소형 라벨 |
| HEADLINE | `{{head}}` Bold | `display: 96` | `{{fg}}` | 1줄. 의도적 대형은 Head Light 허용(§9.1) |
| HEADLINE 2line | `{{head}}` Bold | `display: 60` | `{{fg}}` | 2줄 이상 시 크기 한 단계 ↓ |
| SUBHEAD (1줄) | `{{head}}` Medium | `subhead: 32` | `{{fg}}` | 2줄 이상이면 **Regular**로 전환(§3) |
| META | `{{body}}` Regular | `caption: 12` | `{{fg}}` | 발행처/날짜는 사실 → `[S#]` 병기 |

토큰: `head="Samsung SS Head KR"` `body="Samsung SS Body KR"` `fg="#FFFFFF"` `bg="#000000"` `accent_mint="#99EADC"` (전부 `Cheil_Design.md §1·§7`)

## 작성 골격
```
배경: {{bg}}  / 텍스트: {{fg}}  / 강조: {{accent_mint}}

[EYEBROW]    : __________
[HEADLINE]   : __________________________      ← 출처 있는 진술이면 [S#]
[SUBHEAD]    : __________________________      ← [S#]
[META]       : 발행처 __ · 날짜 __ · 범위 __   [S#]
```

## 강제 체크
- [HVB] HEADLINE/SUBHEAD = Head 계열(>18pt). [FONT-KR] 한글이면 `*_KR`.
- [COLOR] §7 팔레트만. 강조는 민트 우선·최소화.
- [TRACK] 자간/행간 임의 변형 금지(폰트 내장값).
- 사실 슬롯(발행처·날짜·범위 등)은 `[S#]` 없으면 작성 금지(헌법 §3). 미확인은 `확인되지 않음`.
