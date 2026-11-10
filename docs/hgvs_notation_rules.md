# HGVS 단백질(p.) 표기 규칙 요약 — 암종 분류 해커톤 v3

기준: HGVS Nomenclature (Hart et al., 2024). 데이터에서는 `p.` 접두사를 생략하고 아미노산을 1글자 코드로 쓴다. `*` = 종결코돈, `X` = 알 수 없는 아미노산.

## 1. 변이 유형 (검사 순서 = 우선순위)

구체적인 패턴부터 차례로 검사한다.

| 순서 | 유형 | 형식 | 예시 |
|---|---|---|---|
| 1 | frameshift | `[ref]위치[alt]fs[*n]` | `P58fs`, `WQ288fs`, `*214fs`, `K437Rfs` |
| 2 | extension | `*위치[alt]ext*n` | `*110Gext*17` |
| 3 | delins | `ref위치[_ref위치]delins삽입서열` | `R376_A377delinsP`, `K629delinsKX` |
| 4 | insertion | `ref위치_ref위치ins삽입서열` | `P11_K12insP` |
| 5 | duplication | `ref위치[_ref위치]dup` | `A12_K14dup` |
| 6 | deletion | `ref위치[_ref위치]del` | `P233del`, `A630_P649del` |
| 7 | delins (구표기) | `시작_끝 ref서열>alt서열` | `91_92NH>KY`, `355_356QG>H*` |
| 8 | substitution | `ref위치alt` | `R132H`, `L42L`, `R213*` |
| – | unknown | 위치만 있음 | `M391` |

검사 순서에서 주의할 점:
- `delins`는 `del`보다 먼저 검사한다. 그래야 delins가 deletion으로 잘못 분류되지 않는다.
- substitution에서는 ref와 alt가 같은지를 먼저 비교한다. 그래야 `*261*`이 nonsense로 분류되지 않는다.

## 2. 단백질 영향 (모델 피처로 쓰는 분류)

| 영향도 | 영향 | 판정 | 예시 |
|---|---|---|---|
| 0 | WT | 변이 없음 / 결측 | `WT` |
| 1 | synonymous | ref == alt | `L42L`, `*261*`, `1529_1530AL>AL` |
| 2 | missense | ref ≠ alt, `*` 없음 | `R132H`, `91_92NH>KY` |
| 3 | inframe_indel | del / ins / dup / delins (`*` 없음) | `P233del`, `P11_K12insP` |
| 3 | extension | ref가 `*` | `*123W`, `*110Gext*17` |
| 4 | nonsense | alt(삽입 서열)에 `*` 포함 | `R213*`, `355_356QG>H*` |
| 4 | frameshift | `fs` 포함 | `K16fs`, `K437Rfs` |

- truncating(절단) 변이는 nonsense와 frameshift를 합친 것이다.
- splice 표기(`c.`, `+`, `?`)는 데이터에 0건이라 범주에서 뺐다.

## 3. 데이터 처리 메모

- 한 셀에 변이 여러 개가 공백으로 이어진 경우가 약 10%다. 공백으로 나눠서 처리한다.
- 결측은 `WT`로 본다.
- frameshift는 축약형(`K16fs`)과 표준형(`K437Rfs`)을 같은 유형으로 처리한다. ins, delins, 범위 del도 위 규칙으로 분류한다.
- 그래서 원래 문자열 대신 유형과 영향으로 바꿔서 피처를 만든다. 남은 과제는 hotspot 토큰도 `유전자_위치_영향` 형태로 정규화하는 것이다.
- train 영향 분포: missense 164,849 / synonymous 67,049 / nonsense 13,352 / frameshift 9,911 / inframe_indel 3
