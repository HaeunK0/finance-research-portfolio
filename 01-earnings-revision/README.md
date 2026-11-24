# 01 | Financial Data Integration — Earnings Surprise & EPS Revision

> QUANTIFI · 2026 Fall · Team project

## Problem & data workflow

기업의 최근 실적과 미래 이익 전망을 하나의 분석 가능한 신호로 결합하는 프로젝트. 서로 다른 관측주기, 단위 및 정보 이용 가능 시점을 가진 데이터를 Python으로 정렬하고, 가설을 검증했다.

### Data sources

| 데이터 | 유형 | 출처 및 입력 파일 | 처리 |
|---|---|---|---|
| FY2 EPS Revision | 재무·애널리스트 전망 | FnGuide DataGuide / kospi_EPS_FY2.csv | 월말 정렬, %→소수, 3개월 lag |
| Earnings Surprise | 실적·컨센서스 기반 | FnGuide DataGuide / surprise.csv | 일별 마지막 월 관측값, %→소수, 3개월 lag |
| 주가 | 비재무 시장 데이터 | FnGuide DataGuide / kospi.csv | 월말 가격과 다음 달 수익률 |
| 시가총액·60일 평균 거래대금 | 시장·유동성 | FnGuide DataGuide / 시총 CSV, trading_value_60_0906.csv | 월말 정렬, 시총 및 유동성 필터 |
| KOSPI200 | 벤치마크 | kospi_200.csv | 월별 수익률 |
| KOSPI, SMB, HML, Rf | 시장 및 요인 데이터 | factor_model_index.csv | 지수 월 변화율, 무위험수익률 월환산 |

주요 금융 데이터는 팀의 DataGuide 추출 자료다. 요인 파일의 원 공급처 및 지수 산출 정의는 추가 확인 대상이다.

## Data engineering & bias control

- 날짜 파싱 오류·중복 관측 처리, 숫자형 변환, 종목 열의 교집합 및 월말 인덱스 정렬.
- 일간 Surprise를 월말로 변환하고 월별 EPS 자료와 결합. 서로 다른 관측주기와 단위를 통일.
- **3개월 availability lag:** 회계기간의 종료일과 실제 실적 공시·컨센서스 이용 가능일이 일치하지 않을 수 있으므로 두 신호를 월별 3칸 이동했다. FY2는 전망 자료이므로 이 시차는 보수적 가정이지 모든 공시 시점을 검증한 것은 아니다.
- 시점 t에 확인 가능한 자료로 포트폴리오를 선정하고 t+1 수익률을 적용. 월별 시총 3,000억원 이상, 거래대금 하위 20% 제외, 핵심 신호 결측 제거.
- 월별 유니버스를 재산정하여 현재 종목 목록을 과거 전체에 고정하는 문제를 줄이는 설계. 상장폐지 종목까지 포함한 원천 추출 조건과 상폐 수익률 반영 여부는 별도 검증이 필요하다.

## Feature engineering & experiment

횡단면 percentile rank로 신호 스케일을 통일한다.

$$Score=0.5R_{Revision}+0.5R_{Surprise}$$

상위 20개와 하위 20개, 월별 리밸런싱, Gross 200%, Net 0%. 왕복 거래비용 30bp, 대차비용 연 3%, 공매도 전면 금지 월 미운용.

## Statistical validation

factor_model_index.csv의 KOSPI·SMB·HML 지수를 월말로 정렬하여 pct_change로 월수익률을 계산하고, 연율 Rf(%)를 월 복리 수익률로 변환했다. KOSPI 수익률에서 Rf를 빼 MKT를 구성하고, 전략 월수익률과 날짜를 맞춰 결측·무한값을 제외했다.

$$r_{p,t}-r_{f,t}=\alpha+\beta_M(MKT-RF)_t+\beta_S SMB_t+\beta_H HML_t+\epsilon_t$$

statsmodels OLS, Newey–West HAC(12개월). 이분산성과 시계열 자기상관을 고려한 표준오차로 절편의 유의성을 평가한다.

| Model | 연환산 알파 (월 절편×12) | t-stat | p-value | 시장 베타 | N |
|---|---:|---:|---:|---:|---:|
| CAPM | 14.00% | 3.510 | 0.000449 | -0.00017 | 128 |
| FF3 | 13.88% | 3.552 | 0.000382 | 0.01783 | 128 |

FF3에서 SMB 0.0725, HML 0.0441, R² 0.00584. 표본 내에서 시장·규모·가치 노출을 통제한 절편이 유의하게 양수였으며 시장 베타는 작았다. 미래 성과 보장이나 인과적 효과를 의미하지 않는다.

Notebook 저장 결과: CAGR 17.78%, Sharpe 1.154, MDD -17.06% (거래비용 차감). 독립 재실행 결과가 아니다.

## Contribution & tools

팀 연구의 데이터 처리, 신호 설계, 모델 구현 및 검증에 참여. pandas, NumPy, statsmodels, Matplotlib, Jupyter 및 AI 보조 코딩 활용. 원천 데이터 및 내부 보고서는 게시하지 않는다.

## Reproducibility

원본 로직을 보존한 검토용 코드와 실행 안내를 별도로 정리한다. ZIP 내 Date/ 폴더와 코드의 Data/ 경로 및 시총 파일명 차이는 실행 전 확인해야 한다.
