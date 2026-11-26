# 01 | Financial Data Integration — Earnings Surprise & EPS Revision

> QUANTIFI · 2026 Fall · Team project

> **연구 질문:** 서로 다른 관측주기와 단위를 가진 기업 실적·전망 데이터를 어떻게 결합해 투자신호를 만들 수 있는가?

[Backtest Notebook](backtest.ipynb) · [데이터 및 방법론](#problem--data-workflow) · [연구 한계](#limitations--reproducibility) · [그림 교체 안내](assets/README.md)

## 연구 질문

1. 일별 실적 서프라이즈와 월별 EPS 전망 변화율을 어떻게 정렬할 것인가?
2. 두 신호의 스케일과 정보 이용 시점 차이를 어떻게 처리할 것인가?
3. 결합 점수에 따른 성과가 시장·규모·가치 요인을 통제한 뒤에도 유의한가?

## 대표 차트

![Cumulative return and drawdown — 실제 그래프 교체 예정](assets/hero-placeholder.svg)

*실제 연구 그래프가 확보되면 교체할 자리입니다. 현재 그림은 성과 데이터가 아닙니다.*

## English summary

A monthly-rebalanced Korean equity long/short strategy combining earnings surprise and FY2 EPS revisions using cross-sectional percentile ranks. The two signals are aligned at month-end and lagged by three months as a conservative timing assumption. Historical results are evaluated with CAPM and Fama–French three-factor regressions using HAC standard errors.

## 한눈에 보기

| 항목 | 내용 |
| --- | --- |
| 신호 | EPS Revision + Earnings Surprise (동일 비중 순위 결합) |
| 운용 | 월별 리밸런싱 · 롱 20 / 숏 20 |
| 보고 성과 | CAGR 17.78% · Sharpe 1.154 · MDD -17.06% |
| 통계 검증 | CAPM / FF3 연환산 알파 14.00% / 13.88% |
| 한계 | 3개월 시차는 보수적 가정, 공시일·상폐 수익률 검증 필요 |

성과는 기존 Notebook 저장 결과 또는 팀 보고값이며 독립적으로 재실행하지 않았습니다.

## Project overview

기업의 실적이 예상보다 좋았는지, 향후 이익에 대한 전망이 개선되고 있는지를 함께 판단하려면 서로 다른 형식의 데이터를 동일한 기준으로 비교할 수 있어야 한다. 이 연구에서는 두 종류의 정보를 Python으로 통합해 투자 가설을 검증했다.

## Problem-solving process

- **데이터 특성 파악:** 일별 실적 서프라이즈와 월별 이익 전망 변화율은 관측주기와 단위가 달라 직접 비교하기 어렵다.
- **전처리 및 통합:** 종목과 날짜를 맞추고, 관측주기와 단위를 통일한 뒤 결측·중복 관측을 처리했다.
- **정보 이용 시점 고려:** 과거 데이터를 활용하는 실험에서 당시 확인하기 어려운 정보를 미리 사용하는 위험을 줄이기 위해 보수적인 시차를 설정했다.
- **판단 기준 및 검증:** 두 정보를 상대적 순위로 변환해 하나의 점수로 결합하고, 과거 수익률 및 회귀분석을 통해 가설의 타당성을 평가했다.

데이터의 양을 늘리는 것보다 서로 다른 데이터의 정의, 관측 기준, 이용 시점을 일치시키는 것이 분석의 출발점이라는 점을 보여주는 연구다.

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

주요 금융 데이터는 팀의 DataGuide 추출 자료다. 요인 데이터(KOSPI, SMB, HML, Rf) 역시 FnGuide DataGuide에서 추출했다. 지수 산출 정의는 별도 확인 대상이다.

## Data engineering & bias control

- 날짜 파싱 오류·중복 관측 처리, 숫자형 변환, 종목 열의 교집합 및 월말 인덱스 정렬.
- 일간 Surprise를 월말로 변환하고 월별 EPS 자료와 결합. 서로 다른 관측주기와 단위를 통일.
- **3개월 availability lag:** 회계기간의 종료일과 실제 실적 공시·컨센서스 이용 가능일이 일치하지 않을 수 있으므로 두 신호를 월별 3칸 이동했다. FY2는 전망 자료이므로 이 시차는 보수적 가정이지 모든 공시 시점을 검증한 것은 아니다.
- 시점 t에 확인 가능한 자료로 포트폴리오를 선정하고 t+1 수익률을 적용. 월별 시총 3,000억원 이상, 거래대금 하위 20% 제외, 핵심 신호 결측 제거.
- 월별 유니버스를 재산정하여 현재 종목 목록을 과거 전체에 고정하는 문제를 줄이는 설계. 원천 데이터 추출 시 상장기업과 상장폐지 기업을 모두 포함했다. 실제 상폐 수익률 반영 여부와 시점별 커버리지는 별도 검증 대상이다.

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

[backtest.ipynb](backtest.ipynb)는 원본 계산 로직을 보존하고 설명 셀을 추가한 검토용 Notebook이다. ZIP 내 Date/ 폴더와 코드의 Data/ 경로 및 시총 파일명 차이는 실행 전 확인해야 한다.

## What we learned | 시행착오와 해석

두 신호의 관측주기와 이용 가능 시점을 일치시키는 것이 연구의 핵심이었다. 실제 공시일을 모두 확인한 결과는 아니므로 3개월 lag를 완전한 미래정보 방지로 해석하지 않는다.

## Repository structure & execution

| 경로 | 역할 |
| --- | --- |
| [README.md](README.md) | 연구 질문, 방법론, 검증 결과와 한계 |
| [backtest.ipynb](backtest.ipynb) | 기존 계산 로직과 설명 셀을 포함한 Notebook |
| [assets/](assets/) | 대표 그림 배치 및 교체 안내 |

Jupyter Notebook/VS Code에서 Notebook을 열고 상단의 import 및 입력 경로를 확인한다. 위 데이터 출처 표에 명시된 원천 파일을 이용 권한에 맞게 로컬에 준비해야 한다. 저장소만으로는 완전한 실행을 보장하지 않으며 라이브러리 버전 및 실제 실행 환경은 추가 확인이 필요하다.

## Limitations & reproducibility

공매도 금지 월 미운용, 원천 가격·상폐 수익률 및 정보 이용 시점 검증 필요. Notebook의 경로·파일명 차이도 확인해야 한다.

원본 데이터와 내부 발표·보고서는 포함하지 않는다. 보고 성과는 역사적 백테스트이며 실제 운용 결과가 아니다. [전체 포트폴리오로 돌아가기](../).
