# Finance & Data Analytics Portfolio

**Sogang University · Mathematics | QUANTIFI · 2026 Fall**

수학적 사고와 금융 도메인 지식을 바탕으로 실제 금융 데이터를 처리하고, Python 및 AI 보조 개발 도구를 활용해 가설을 정량화하고 검증한 팀 연구 프로젝트입니다.

## Data analytics & research projects

| Project | Data & engineering | Statistical validation |
| --- | --- | --- |
| [01. Financial Data Integration](01-earnings-revision/) | EPS 전망·실적, DataGuide, 월말 정렬, 3개월 lag | CAPM / FF3, HAC, p-value |
| [02. Financial Data Quality](02-shareholder-yield/) | 배당·발행주식수, 금융업 누락 보완, 수정주식수 역산 | NumPy OLS / Newey–West 구현, 민감도 분석 |
| [03. Alternative Data Integration](03-export-enhanced-pas/) | 가격·산업·수출 데이터 매핑, DataGuide / K-stat, 팀원 DART 크롤링 | 분위 검정, 비교 실험, 요인 회귀 |
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

- 모든 프로젝트는 QUANTIFI 공동 연구이며 단독 성과로 표시하지 않습니다. 3번 프로젝트의 DART 크롤링은 팀원이 담당했습니다. 정확한 DART 수집 항목과 K-stat 자료의 관계는 추가 확인 중입니다.
- 데이터 시차 및 시점별 유니버스는 편향 완화를 위한 설계이며 완전한 제거를 주장하지 않습니다. 상장폐지 기업의 원천 커버리지와 수익률 반영 여부는 추가 검증 대상입니다.
- 성과 및 회귀 수치는 저장된 실행 결과와 연구 보고값을 구분해 기재했습니다. 독립 재실행이나 실거래 성과가 아닙니다.
- 원천 금융 데이터, 내부 보고서 및 손필기는 게시하지 않습니다. 코드는 원본 계산 로직을 보존하고 설명을 추가한 버전으로 정리하며 실행 환경과 데이터 가용성에 따라 재현 여부가 달라질 수 있습니다.

