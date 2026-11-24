# 01 | Earnings Surprise & EPS Revision

> QUANTIFI · 2026 Fall · Team research

## Research question
최근 실적이 시장 기대를 상회하고 중기 이익 전망도 개선되는 기업의 상대수익률이 높은지 검증했다.

## Data & methodology
- 실적 서프라이즈, FY2 EPS Revision, 수정주가, 시가총액 및 거래대금 활용.
- 일간 실적 신호를 월말 관측값으로 변환하고 두 신호에 3개월 시차를 적용.
- 시총 3,000억원 이상, 60거래일 평균 거래대금 하위 20% 제외.
- 횡단면 백분위 순위로 변환한 후 동일 비중으로 결합.

$$Score=0.5R_{Revision}+0.5R_{Surprise}$$

## Portfolio construction
월별 상위 20종목 매수, 하위 20종목 매도. Gross 200%, Net 0%. 형성 다음 달 수익률을 적용하며 거래비용, 대차비용, 공매도 금지기간을 고려한다.

## Validation & limitations
팀 연구에서 거래비용 차감 성과와 시장·팩터 노출을 검토했다. 데이터 공표 시점, 결측치 처리 및 실제 거래 가능성은 추가 검증이 필요하다. 3개월 시차만으로 미래정보 편향이 완전히 제거된다고 단정할 수 없다.

## Contribution
공동 연구의 투자 가설 검토, 데이터 처리, 모델 구현 및 성과 분석에 참여했다. 개별 작업의 상세 역할은 추가 정리 예정이다.

## Reproducibility
현재 연구 소개 단계이며 정리된 코드 및 실행 안내는 아직 게시하지 않았다. 원천 금융 데이터는 제공하지 않는다.
