# 템플릿 — Section Divider (섹션 전환)

> `Cheil_Design.md §8` 카테고리 #3. 섹션명 + 초대형 번호(≈199pt, 장식 전용) + 단계 라벨.
> 초대형 번호는 **그래픽 장식**이지 본문 텍스트가 아니다(§4 Graphic).

## 레이아웃 슬롯
```
┌──────────────────────────────────────────────┐
│                                    [BIGNUM]    │   ← 초대형 섹션 번호(장식)
│                                       02       │
│  [STAGE-LABEL]  단계/파트 라벨                 │
│  [SECTION-TITLE]  섹션명                       │   ← 섹션 제목
│  [LEAD]  한 줄 리드 (선택)                     │
└──────────────────────────────────────────────┘
```

## 슬롯 ↔ 토큰 매핑 (placeholder)
| 슬롯 | 폰트 | 크기 pt | 컬러 | 비고 |
|---|---|---|---|---|
| BIGNUM | `{{head}}` Light/Bold | `graphic_oversize: 199` | `{{accent_mint}}` 또는 `{{fg}}` | 장식 전용. 본문 텍스트 아님 |
| STAGE-LABEL | `{{head}}` Medium | `caption: 12` | `{{accent_mint}}` | "Part 02" 등 |
| SECTION-TITLE | `{{head}}` Bold | `display: 60` | `{{fg}}` | 섹션명 |
| LEAD | `{{body}}` Regular | `body: 18` | `{{fg}}` | 선택. 사실이면 `[S#]` |

토큰: `head="Samsung SS Head KR"` `body="Samsung SS Body KR"` `fg="#FFFFFF"` `bg="#000000"` `accent_mint="#99EADC"` (전부 `Cheil_Design.md §1·§7`)

## 작성 골격
```
배경: {{bg}}  / 텍스트: {{fg}}  / 번호·라벨 강조: {{accent_mint}}

[BIGNUM]        : __번호__            ← 장식(출처 불요)
[STAGE-LABEL]   : Part __
[SECTION-TITLE] : __섹션명__
[LEAD]          : __한 줄 리드__       ← 사실이면 [S#]
```

## 강제 체크
- [HVB] BIGNUM/TITLE/STAGE = Head 계열. [FONT-KR] 한글이면 `*_KR`.
- BIGNUM은 §4 Graphic(≈199pt) 장식 — 정보 전달 텍스트로 쓰지 않음.
- [COLOR] §7 팔레트만, 강조 최소화. [TRACK] 자간/행간 임의 변형 금지.
- LEAD가 사실 진술이면 `[S#]` 필수(헌법 §3). 미확인은 `확인되지 않음`.
