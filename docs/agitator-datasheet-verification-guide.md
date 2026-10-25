# Agitator 설계 검증 가이드 (공급사 Datasheet 검토용)

> 목적: 공급사로부터 접수한 Agitator datasheet로 **무엇을 검증할 수 있는지**, 그리고 실제 Agitator 설계의 **주요 항목과 각 항목 검증에 필요한 데이터**를 정리한다.
>
> 참고: 본 문서는 일반적인 Top-entry 교반기 설계 기준으로 작성했다. BL1U / BL2U 실제 datasheet 값은 반영되어 있지 않으며, 파일 확보 후 4장 체크리스트에 값을 채워 검토한다.

---

## 1. 검증 수준 구분

공급사 datasheet만으로 확인할 수 있는 범위는 제한적이다. 검증을 세 단계로 나누어 보면 무엇을 추가로 요구해야 하는지 명확해진다.

| 단계 | 내용 | 필요한 자료 |
|---|---|---|
| **L1. 사양 일치(Compliance) 확인** | 당사 Spec/Data Sheet(구매사양)와 공급사 datasheet 값이 일치하는지, 요구 최소값을 만족하는지 | 당사 구매사양서 + 공급사 datasheet |
| **L2. 재계산(Cross-check)** | datasheet에 적힌 값들끼리 물리적으로 맞는지 직접 계산해 확인 (동력, 토크, 회전수, 응력 등) | 공급사 datasheet (+ 문헌 계수) |
| **L3. 상세 설계 검증** | Shaft 처짐·임계속도, 하중, 베어링 수명, 공정 성능(부유·혼합시간) 등 | 공급사 계산서, GA 도면, 부품 datasheet, 공정 데이터 |

일반적으로 datasheet 한 장으로는 **L1 전부 + L2 일부**까지 가능하고, L3는 공급사에 계산서와 도면을 별도로 요구해야 한다.

---

## 2. 공급사 Datasheet로 검증 가능한 항목

### 2.1 L1 – 사양 일치 확인 (datasheet만으로 가능)

| 구분 | 확인 항목 | 확인 포인트 |
|---|---|---|
| 운전조건 | 유체명, 밀도, 점도, 온도, 압력(설계/운전), 고형분 농도 | 당사 공정 데이터와 동일한지 (특히 **최대 밀도·최대 점도** 기준으로 설계했는지) |
| Vessel | 탱크 내경(T), 액위(H, 최소/정상/최대), 경판 형상, Baffle 유무/개수/폭 | 당사 Vessel 도면과 일치 여부 |
| Impeller | 형식, 직경(D), 단수, 날개 수/각도, 설치 높이(C) | 요구 형식과 일치, D/T·C/T가 일반 범위인지 |
| 회전수 | 출력축 rpm, 인버터(VFD) 사용 시 운전 범위 | 요구 rpm 범위 충족 |
| Motor | 정격 출력(kW), 극수, 전압/주파수, 효율 등급(IE3 등), 보호등급(IP), 방폭등급(Ex), 절연등급 | 당사 전기사양과 일치, 방폭 구역 등급 충족 |
| Gearbox | 형식, 감속비, Service Factor(AGMA), 윤활 방식, 출력 베어링 수명(L10) | SF ≥ 요구값(통상 1.5~2.0), L10 ≥ 요구값 |
| Shaft | 직경, 길이, 재질, 분할(Coupling) 여부, Steady bearing 유무 | 재질·형식 요구 충족 |
| Sealing | 형식(Gland packing / Lip / Mechanical seal, single/double), Seal flush plan, 설계압력·온도 | Vessel 설계압력·온도 및 유체 특성(독성·휘발성)에 적합 |
| 재질 | 접액부(Shaft, Impeller, Hub), 비접액부, Lining/Coating, 부식여유 | 당사 재질 선정표와 일치 |
| 기계 데이터 | 1차 임계속도 또는 운전속도/임계속도 비, 소음(dB(A)), 진동 기준, 총 중량 | 임계속도 비 기준(통상 ≤ 70~80%), 소음 ≤ 85 dB(A) 등 요구값 충족 |
| 설치 하중 | Mounting flange에 작용하는 중량, 토크, 굽힘모멘트, 축방향 하중 | Vessel/지지 구조 설계에 넘겨줄 값이 모두 있는지 |
| 검사·시험 | 무부하 운전시험, Balancing 등급(ISO 1940 G6.3 등), 도장, 문서 | 요구 ITP 충족 |

### 2.2 L2 – Datasheet 값으로 재계산 가능한 항목

datasheet에 아래 값들이 있으면 직접 계산해서 공급사 값과 비교할 수 있다.

| 검증 항목 | 계산식 | 비교 대상 |
|---|---|---|
| 출력 회전수 | N_out = N_motor / 감속비 | datasheet rpm |
| Tip speed | v_tip = π·D·N / 60 [m/s] | 공정 요구(전단, 결정 파손 등) 한계 |
| Reynolds 수 | Re = ρ·N·D² / μ (N: rps) | 유동영역 판단 (Re > 10⁴ 난류, < 10 층류) |
| 소요동력 | P = Np·ρ·N³·D⁵ (N: rps, 다단이면 단별 합) | datasheet absorbed power |
| 단위체적당 동력 | P/V [kW/m³] | 공정 요구 교반강도 |
| 토출유량 | Q = Nq·N·D³ | 순환횟수(Q/V), 혼합 성능 |
| Motor 여유율 | 정격출력 / 소요동력 | 통상 1.1~1.25 이상 (최대 밀도·점도 조건 기준) |
| 출력축 토크 | T [N·m] = 9550 × P[kW] / N[rpm] | Gearbox 정격토크, Shaft 응력 계산 입력값 |
| Shaft 비틀림 응력 | τ = 16·T / (π·d³) | 재질 허용응력 (굽힘 포함 시 L3로) |
| Gearbox SF 검토 | Gearbox 정격 / Motor 정격(또는 소요동력) | 요구 SF |

**표준 임펠러의 대략적인 동력수·토출수 (난류, Baffle 있음, 참고값)**

| 임펠러 | Np | Nq |
|---|---|---|
| Rushton turbine (6-blade disc) | ≈ 5.0 | ≈ 0.72 |
| 45° Pitched blade turbine (4-blade, W/D=0.2) | ≈ 1.2~1.3 | ≈ 0.79 |
| Hydrofoil (A310, HE-3 계열) | ≈ 0.3 | ≈ 0.55 |
| Anchor / Helical ribbon | 층류에서 Np = K / Re | – |

> 공급사가 Np를 제시하지 않으면 위 값으로 동력을 역산해 datasheet 동력과 크게 다른지만 우선 확인하고, 실제 Np는 공급사에 요구한다.

### 2.3 L3 – Datasheet만으로는 검증이 어려운 항목 (추가 자료 필요)

- Shaft 굽힘 응력 및 처짐 (임펠러 위치, Seal 위치)
- 1차 임계속도 계산 근거
- 임펠러에 작용하는 Hydraulic radial load, 블레이드 강도
- Gearbox 열정격(Thermal rating), 베어링 수명 계산
- 공정 성능 (고형물 부유 Njs, 혼합시간, 가스분산, 열전달)
- Vessel nozzle / 지지 구조물 하중 검토
- Mechanical seal 허용 runout·처짐과 Shaft 처짐의 비교

---

## 3. Agitator 설계 주요 항목과 검증에 필요한 데이터

### 3.1 공정 목적 및 교반 성능

교반기 설계의 출발점. 목적에 따라 임펠러 형식과 설계 기준이 완전히 달라진다.

| 공정 목적 | 주요 설계 기준 | 검증 방법 | 필요 데이터 |
|---|---|---|---|
| 균일 혼합 (Blending) | 혼합시간 θ, Bulk velocity | 난류: N·θ₉₅ ≈ 5.20·Np^(-1/3)·(T/D)² (H≈T, Grenville) | 밀도, 점도, 액위, T, D, N, Np, 요구 혼합시간 |
| 고형물 부유 (Suspension) | Njs (Just-suspended speed), 균일 부유 여부 | Zwietering: Njs = S·ν^0.1·[g·Δρ/ρL]^0.45·X^0.13·dp^0.2·D^(-0.85) | 고형분 밀도, 입도(d50, dmax), 농도(wt%), 액 밀도·점도, 임펠러 형식·C/T, 탱크 바닥 형상 |
| 가스 분산 | Flooding 방지, Gassed power | Flow number(Fl_G), Froude 수, Gassed/Ungassed power 비 | 가스 유량, 압력, 액 물성 |
| 열전달 | Jacket/Coil 측 열전달계수 | Nu = f(Re, Pr) 상관식 | 열부하, 유체 열물성, Jacket/Coil 사양 |
| 액-액 분산, 결정화 등 | 전단, Tip speed 제한, 액적/결정 크기 | P/V, Tip speed | 공정 요구 사양 |

**필요 데이터 (공정 측에서 준비)**
- 유체 물성: 밀도(최소/최대), 점도(온도별, Non-Newtonian 여부), 온도, 압력
- 고형물: 밀도, 입도분포, 농도, 침강속도
- 운전 시나리오: 최소/최대 액위, Batch/Continuous, 투입·배출 순서, **정지 후 재기동(고형물 침전 상태에서 기동)** 여부
- 요구 성능: 혼합시간, 부유 수준, 교반강도(Scale of agitation) 등

### 3.2 Vessel 및 Impeller 형상

| 항목 | 일반 범위 / 검토 포인트 | 필요 데이터 |
|---|---|---|
| H/T | 1.0 내외 단단, 1.2 이상이면 다단 임펠러 고려 | Vessel 도면 |
| D/T | Pitched/Hydrofoil 0.3~0.5, Anchor 0.9 이상 | 임펠러 도면 |
| C/T (Off-bottom clearance) | 0.15~0.35 (부유 목적은 낮게) | GA 도면 |
| 다단 간격 | 통상 1.0~1.5 D | GA 도면 |
| 최저 액위 운전 | 최하단 임펠러가 잠기는지, 액면 근처 임펠러의 하중 증가 | 최소 액위, 임펠러 높이 |
| Baffle | 통상 4개, 폭 T/10~T/12, 벽과 간격 확보 | Vessel 도면 |
| 내부 구조물 | Coil, Dip pipe, Thermowell 등과의 간섭 | Vessel 도면 + 교반기 GA |

### 3.3 동력 및 구동부 (Motor / Gearbox / VFD)

| 항목 | 검토 포인트 | 필요 데이터 |
|---|---|---|
| 소요동력 | Np 근거, **최대 밀도·점도·최고 액위** 조건 적용 여부, 가스 유입 시 Ungassed 조건 기준 | Np(또는 동력곡선), 운전조건 |
| Motor 정격 | 여유율, 기동토크(고형물 침전 후 재기동), VFD 운전 시 저속 냉각 | Motor datasheet, 기동 조건 |
| Gearbox | AGMA Service factor, 열정격, 출력 베어링 L10 수명(통상 ≥ 50,000~100,000 h), 축방향/반경방향 허용하중 | Gearbox 카탈로그/계산서 |
| 윤활 | 윤활 방식, 오일 교환 주기, Dry-well 구조(오일 누유 방지) | Gearbox 도면 |

### 3.4 Shaft 기계 설계 (가장 중요한 기계 검증 항목)

| 항목 | 검토 방법 | 필요 데이터 |
|---|---|---|
| 하중 | 토크 + 임펠러 Hydraulic radial load에 의한 굽힘모멘트, 자중, 축방향 추력 | 임펠러별 Hydraulic load (공급사 경험계수), 각 임펠러 위치 |
| 응력 | 등가토크 Te = √(M² + T²), 응력 = 16·Te / (π·d³) ≤ 허용응력 (피로 고려) | Shaft 직경, 재질 허용응력, 굽힘모멘트 |
| 처짐 | 임펠러 끝단 및 **Seal 위치** 처짐이 Seal 허용치 이하인지 | Shaft 길이, 베어링 간격, 단면, 탄성계수 |
| 임계속도 | 운전속도 ≤ 1차 임계속도의 약 70~80% (공급사 기준 확인) — VFD 최대속도 기준으로 검토 | Shaft 치수, 임펠러 질량, 베어링 지지 조건, 계산서 |
| Steady bearing | 장축일 때 필요 여부, 마모·윤활 문제 | 축 길이, 공정 유체 |
| 연결부 | Coupling, Hub-key, Shrink fit, 볼트 강도 | 상세 도면 |

### 3.5 Impeller 강도 및 마모

| 항목 | 검토 포인트 | 필요 데이터 |
|---|---|---|
| Blade 두께/강도 | Blade 근부 굽힘 응력 | 임펠러 도면, 하중 |
| 마모/부식 | Slurry 시 Tip speed 제한, 경화 육성, 부식여유 | 고형분 성상, 재질 |
| 분해/교체 | Split type 여부, Manhole 통과 가능 여부 | Manhole 크기, 임펠러 치수 |

### 3.6 Sealing

| 항목 | 검토 포인트 | 필요 데이터 |
|---|---|---|
| Seal 형식 | 독성/인화성/고압 유체는 Double mechanical seal 등 | 유체 위험성, 설계압력·온도 |
| Seal 지원 시스템 | Barrier/Buffer 유체, 압력, Thermosiphon 등 | Seal 계통도 |
| Shaft 처짐·Runout | Seal 위치 Shaft 처짐/Runout이 Seal 허용치 이하 | Seal datasheet 허용치 + Shaft 처짐 계산 |
| 정비성 | Cartridge type, 교반기 분해 없이 교체 가능 여부 | Seal 도면 |

### 3.7 설치 및 지지 구조

| 항목 | 검토 포인트 | 필요 데이터 |
|---|---|---|
| Mounting 하중 | 정하중(중량) + 동하중(토크, 굽힘모멘트, 추력)을 Vessel nozzle/Beam 설계에 반영 | 공급사 Mounting load 표 (정적/동적 구분) |
| Nozzle/Flange | 크기, Rating, 보강 | Vessel 노즐 사양 |
| 유지보수 공간 | Shaft 인출 높이, Hoist/크레인 | 설치 레이아웃, GA 도면 |
| 진동 | 구조물 고유진동수와 운전속도(및 날개통과주파수) 분리 | 구조 해석 또는 공급사 데이터 |

### 3.8 재질, 소음·진동, 검사·시험

- 재질: 접액부 내식성, Lining/Coating 적용 시 시공 가능 형상
- 소음: 통상 1 m 기준 ≤ 85 dB(A) 등 당사 요구치
- 진동: ISO 10816 / ISO 20816 계열 기준
- 검사/시험: 무부하 운전시험(진동·소음·온도), Balancing(ISO 1940), 재질증명서(MTC), 치수검사, Run-out 측정

---

## 4. Datasheet 접수 시 검토 체크리스트 (BL1U / BL2U 공통 양식)

| No | 항목 | 당사 요구값 | BL1U 공급사 값 | BL2U 공급사 값 | 재계산값 | 판정 | 비고 |
|---|---|---|---|---|---|---|---|
| 1 | 유체 밀도(최대) [kg/m³] | | | | – | | |
| 2 | 점도(최대) [cP] | | | | – | | |
| 3 | 설계압력/온도 | | | | – | | |
| 4 | 탱크 내경 T / 액위 H [mm] | | | | – | | |
| 5 | 임펠러 형식 / 단수 | | | | – | | |
| 6 | 임펠러 직경 D [mm], D/T | | | | | | |
| 7 | 설치 높이 C [mm], C/T | | | | | | |
| 8 | 회전수 N [rpm] | | | | Motor rpm ÷ 감속비 | | |
| 9 | Tip speed [m/s] | | | | πDN/60 | | |
| 10 | Reynolds 수 | – | | | ρND²/μ | | |
| 11 | 소요동력 [kW] | | | | Np·ρ·N³·D⁵ | | |
| 12 | P/V [kW/m³] | | | | | | |
| 13 | Motor 정격 [kW] / 여유율 | | | | 정격 ÷ 소요동력 | | |
| 14 | Motor 방폭/IP/효율등급 | | | | – | | |
| 15 | Gearbox 감속비 / SF | | | | | | |
| 16 | 출력축 토크 [N·m] | – | | | 9550·P/N | | |
| 17 | Shaft 직경 / 재질 | | | | τ = 16T/(πd³) | | |
| 18 | 임계속도 비 (N / Nc) | | | | | | |
| 19 | Seal 형식 / Flush plan | | | | – | | |
| 20 | 접액부 재질 | | | | – | | |
| 21 | Mounting 하중 (중량/토크/모멘트/추력) | – | | | – | | 구조 설계 전달 |
| 22 | 소음 [dB(A)] | | | | – | | |
| 23 | 중량 [kg] | – | | | – | | |
| 24 | 베어링 L10 [h] | | | | – | | |

---

## 5. 공급사에 추가로 요구할 자료 (L3 검증용)

1. **GA 도면** (임펠러 위치, Shaft 길이, 베어링 간격, Seal 위치, Mounting 상세)
2. **Shaft 계산서** (하중 조건, 굽힘/비틀림 응력, 처짐, 1차 임계속도)
3. **동력 계산서** (사용 Np, 계산 조건 – 최대 밀도/점도/액위, Gassed/Ungassed)
4. **공정 성능 근거** (부유 목적이면 Njs 대비 운전속도 비, 혼합 목적이면 혼합시간 또는 Scale of agitation)
5. **Mounting load 표** (정하중/동하중 구분, 하중 방향)
6. **Gearbox datasheet** (정격토크, SF, 열정격, 베어링 수명, 허용 외부하중)
7. **Motor datasheet** (기동토크, 기동전류, 방폭 인증서)
8. **Mechanical seal datasheet** (허용 압력·온도, 허용 Shaft 처짐/Runout, 지원 시스템)
9. **임펠러 상세도** (블레이드 두께, 재질, 질량)
10. **ITP / 시험 절차서**

---

## 6. 검토 시 자주 발견되는 문제

- 정상 운전 밀도·점도로만 동력을 계산 (최대 조건, 고형분 포함 Slurry 밀도 미반영)
- VFD 최대 회전수가 아닌 정격 회전수 기준으로 임계속도 검토
- 최소 액위에서 임펠러가 액면 부근에 위치 → 하중 급증, 진동
- Seal 위치 Shaft 처짐을 검토하지 않음
- Mounting 동하중(굽힘모멘트) 누락 → Vessel 노즐/Beam 과소 설계
- 고형물 침전 후 재기동 조건 미검토 (기동토크 부족, Shaft 과부하)
- 공급사 datasheet의 Vessel 치수·액위가 최신 Vessel 도면과 불일치
