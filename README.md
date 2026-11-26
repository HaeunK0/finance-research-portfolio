# Finance & Data Analytics Portfolio

**Sogang University · Mathematics | QUANTIFI · 2026 Fall**

수학적 사고를 바탕으로 추상적인 아이디어를 검증 가능한 가설로 구체화하고, Python과 생성형 AI를 활용하여 데이터 분석, 판단 규칙 설계, 구현 및 성능 검증을 수행했습니다.

본 포트폴리오는 QUANTIFI에서 진행한 팀 프로젝트를 바탕으로 구성하였으며, 투자전략의 성과뿐 아니라 데이터의 특성 파악, 새로운 판단 지표의 설계, 반복적인 검증 및 시행착오를 통한 문제 해결 과정에 중점을 두었습니다.

## Project highlights

| 프로젝트 | 해결하려던 문제와 핵심 역량 |
| --- | --- |
| [01. Financial Data Integration](01-earnings-revision/) | 서로 다른 주기·단위의 기업 실적과 전망 데이터를 정렬하고, 정보 이용 시점을 고려해 통합 |
| [02. Financial Data Quality](02-shareholder-yield/) | 시가총액과 수정주가의 관계에서 수정주식수 대용치를 역산하고 배당 정보와 결합해 새로운 지표 설계 |
| [03. Alternative Data Integration](03-export-enhanced-pas/) | 수출 데이터를 추가한 전략과 기존 전략을 비교해 데이터의 실질적 유용성을 평가하고 대안 탐색 |
| [04. Statistical Feature Engineering](04-price-asymmetric-strategy/) | 일별 수정주가에서 네 가지 통계적 특성을 추출하고 결합해 판단 규칙을 설계한 뒤 예측력과 성능 검증 |

처음 읽는 분께는 02, 03, 04번의 Project overview / Problem-solving process를 권장합니다. 각 페이지에 구체적인 데이터 처리, 수식, 검증 결과 및 Notebook을 함께 정리했습니다.

## Data analytics & research projects

| Project | Data & engineering | Statistical validation |
| --- | --- | --- |
| [01. Financial Data Integration](01-earnings-revision/) | EPS 전망·실적, DataGuide, 월말 정렬, 3개월 lag | CAPM / FF3, HAC, p-value |
| [02. Financial Data Quality](02-shareholder-yield/) | 배당·발행주식수, 금융업 누락 보완, 수정주식수 역산 | NumPy OLS / Newey–West 구현, 민감도 분석 |
| [03. Alternative Data Integration](03-export-enhanced-pas/) | 가격·산업·수출 데이터 매핑, DataGuide / K-stat, DART 공시 수집·산업분류 매핑 시도 | 분위 검정, 비교 실험, 요인 회귀 |
| [04. Statistical Feature Engineering](04-price-asymmetric-strategy/) | 가격·거래대금, rolling 통계량, 횡단면 순위 | IC / Signal Decay, Carhart 4요인 보고 결과 |

## Research workflow

1. 데이터 출처와 변수 정의 확인 (재무·비재무·대체 데이터 구분)
2. 결측·중복·단위 처리 및 종목·날짜 정렬
3. 정보 이용 가능 시점에 맞춘 lag와 시점별 유니버스 구성
4. Python 기반 피처 엔지니어링, 모델 설계와 백테스트
5. 회귀분석, 통계적 유의성 평가, 비교 실험과 리스크 분석
6. 결과 해석 및 한계 문서화

## Tools & interests

Python · pandas · NumPy · SciPy · statsmodels · Matplotlib · Jupyter Notebook · VS Code · Codex

Financial data analytics, statistical modeling, risk management, AI-assisted research workflows.

## Scope & transparency

- 네 프로젝트는 QUANTIFI 공동 연구입니다. 3번 프로젝트는 팀 차원에서 여러 출처의 수출 데이터 확보 및 가공을 시도했으며, 기업별 데이터 구축안과 최종 실험에 사용한 산업별 신호를 구분합니다.
- 데이터 시차 및 시점별 유니버스는 편향 완화를 위한 설계이며 완전한 제거를 주장하지 않습니다. 네 프로젝트 모두 DataGuide에서 상장기업과 상장폐지 기업을 포함해 추출했으며 요인 데이터도 DataGuide를 사용했습니다. 상폐 수익률 반영 여부는 추가 검증 대상입니다.
- 성과 및 회귀 수치는 저장된 실행 결과와 연구 보고값을 구분해 기재했습니다. 독립 재실행이나 실거래 성과가 아닙니다.
- 원천 금융 데이터, 내부 보고서 및 손필기는 게시하지 않습니다. 각 프로젝트에는 기존 계산 로직을 보존하고 설명을 추가한 backtest.ipynb를 게시했으며 실행 환경과 데이터 가용성에 따라 재현 여부가 달라질 수 있습니다.

