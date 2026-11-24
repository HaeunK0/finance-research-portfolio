# 04 | Statistical Feature Engineering — Price Asymmetric Strategy (PAS)

> QUANTIFI · 2026 Fall · Final team research

## Problem & data workflow

가격 추세의 강도뿐 아니라 수익률 분포와 변동성을 활용해 지속적인 상승과 취약한 상승을 구분하는 연구. 재무제표 없이 시계열 시장 데이터를 처리하고 통계적 피처를 설계했다.

| 데이터 | 유형 | 출처 / 입력 파일 | 처리 |
|---|---|---|---|
| 일별 수정주가 | 비재무 시장 | FnGuide DataGuide / adj_close_0906.csv | 일간 수익률, 월말 모멘텀, rolling 통계 |
| 배당반영 수정주가 | 시장·총수익 | DataGuide / adj_close_dividend_0906.csv | 월별 총수익률 |
| 월말 시가총액 | 시장 | DataGuide / mkt_cap_0906.csv | 시점별 상위 30% |
| 일별 ADV20 | 시장·유동성 | DataGuide / 20일평균거래대금.csv | 예상 체결 가능성 |
| KOSPI, Rf, SMB, HML, Mom | 시장·요인 | Factor_0917.csv | 벤치마크, Rf 조정, 보고서의 Carhart 회귀 |

주요 데이터는 팀 DataGuide 추출 자료. 요인 데이터(KOSPI, SMB, HML, MOM, Rf)도 FnGuide DataGuide에서 추출했다. 지수 산출 정의는 별도 확인 대상이다.

## Data engineering & bias control

- 날짜 파싱, numeric coercion, 종목 교집합, 실제 마지막 거래일을 기준으로 일·월 패널 정렬.
- 신호용 수정주가와 성과 측정용 배당반영 수정주가를 구분.
- MOM6 = P(t−1)/P(t−6)−1. 최근 한 달을 제외한 모멘텀, MOM12는 최소 가격 이력 확인에 사용.
- 일간 수익률에서 126일 rolling mean·std·skew(최소 100개 관측)를 계산하고, 평균/표준편차로 종목의 Sharpe 대용 지표 생성.
- 재무제표 공시 시차를 쓰는 전략이 아니므로 일률적인 3개월 lag를 적용하지 않는다. 형성일과 보유기간을 분리하고 과거 관측값으로 시총·거래대금을 정렬한다.
- 유니버스를 시점별로 구성하며 상폐 기업을 포함하는 원천 자료 사용은 생존편향 완화를 위한 팀의 데이터 수집 원칙이다. 팀의 확인에 따르면 네 프로젝트 모두 상장기업과 상장폐지 기업을 포함해 추출했다. 상폐 수익률 커버리지는 아직 독립 확인하지 않았다.
- **추가 점검:** 실제 코드의 pick()에는 next_date 가격 존재 여부를 확인하는 필터가 있다. 이는 미래 데이터 가용성을 이용할 수 있어 look-ahead 위험이 있으며, forward fill된 가격의 상폐 처리 및 비중 재정규화 후 체결률도 재검토해야 한다. 원본 계산 로직은 임의 수정하지 않는다.

## Model & validation

$$L=R(MOM6)R(SHARPE126)(1-R(SKEW126))$$
$$S=R(MOM6)(1-R(VOL126))(1-R(SKEW126))$$

숏 하위 100개 우선, 롱 상위 50개 후보 및 하락일 60일 이상 조건. ADV20 10%, 5일간 목표수량의 80% 체결 조건. 롱110/숏90, 분기 리밸런싱, 왕복 50bp 및 연 3% 대차.

제출 Notebook에는 체결기간별 성과 비교, Rolling IC 및 Signal Decay, 스코어 분포 분석이 있다. 단기 수익률과 점수의 Spearman 상관 및 예측 기간별 관계를 확인하는 실험이다.

### Factor regression (팀 전략설명서)

보고서의 Carhart 4요인 모델은 KOSPI 월수익률−Rf, SMB·HML·MOM 지수의 월 변화율과 전략 월 초과수익률을 정렬하고 HAC(12)로 표준오차를 보정했다.

| Variable | Coefficient | t-stat | p-value |
|---|---:|---:|---:|
| Alpha (월) | 0.0127 | 3.30 | <0.001 |
| MKT−RF | 0.0324 | 0.43 | 0.664 |
| SMB | -0.2148 | -1.39 | 0.165 |
| HML | 0.1413 | 0.96 | 0.338 |
| MOM | 0.0818 | 0.72 | 0.473 |

N=120, R²=0.106. 시장·규모·가치·모멘텀 통제 후 양의 절편이 보고되었고 시장 베타는 통계적으로 유의하지 않았다. 낮은 R²는 설명되지 않는 변동 비율이 크다는 뜻이지 그 전체가 알파라는 뜻이 아니다. 회귀 구현 셀은 현재 제출 Notebook에 포함되지 않아 보고서 수치로 구분한다.

| 5-day execution (report) | Strategy | KOSPI200 |
|---|---:|---:|
| CAGR | 21.6% | 15.4% |
| Sharpe (Rf adjusted) | 1.14 | 0.57 |
| MDD | -13.6% | -35.9% |

2016.09–2026.08 제출 코드와 연결된 설명서 기준. 최종 발표의 별도 버전은 CAGR 23.55%, Sharpe 1.24, MDD -13.5%를 보고한다. 두 버전을 혼합하지 않는다. 공매도 금지기간의 실행 가능성 및 비용 정합성은 한계다. 독립 재실행 결과가 아니다.

## Contribution & tools

팀 연구의 피처 설계, Python 구현, 데이터 처리, 백테스트 및 성과·리스크 분석에 참여. pandas, NumPy, Matplotlib, Jupyter 및 AI 보조 코딩 활용. 원천 데이터 및 내부 보고서는 게시하지 않는다.

## Notebook

[backtest.ipynb](backtest.ipynb) — 기존 계산 로직을 유지하고 설명 셀을 추가한 버전. 저장된 실행 출력은 제거했으며 독립 재실행은 수행하지 않았다.
