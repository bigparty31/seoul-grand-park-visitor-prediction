# 서울대공원 입장객 수 분석 및 예측 (팀 돌멩이)

서울대공원의 일별 입장객 데이터(2018~2023)를 활용해 **방문객 수에 영향을 주는 요인을 분석**하고,
여러 시계열/머신러닝 모델로 **일별 개인 방문객 수를 예측**한 팀 프로젝트입니다.

## 프로젝트 개요

- **분석 대상**: 서울대공원 일별 입장객 수 (`individual_visitors`, 개인 방문객 기준)
- **분석 기간**: 2018 ~ 2023년
- **주요 질문**
  - 강수 여부가 방문객 수에 영향을 주는가? (T-검정 / 효과크기)
  - 요일·공휴일·월(계절성)에 따라 방문 패턴이 어떻게 달라지는가?
  - 2023년 방문객 수를 얼마나 정확히 예측할 수 있는가?

## 데이터

| 구분 | 파일 | 주요 컬럼 |
| --- | --- | --- |
| 입장객 | `서울대공원_입장객_정보_*.csv`, `*_full_visitor_data_cleaned*.csv` | `date`, `visitors`, `zoo_garden_group`, `theme_garden_group`, `individual_visitors`, `day` |
| 강수량 | `*과천시_강우량.csv` | `지점`, `지점명`, `일시`, `강수량(mm)` |
| 강수여부 가공 | `*_서울대공원_입장객(강수여부추가ver.).csv` | 위 입장객 컬럼 + `is_rainy`(비옴/비안옴) |

> 인코딩이 파일마다 `cp949`/`euc-kr`/`utf-8`/`utf-8-sig`로 섞여 있어, 로드 시 인코딩을 분기 처리합니다.

## 노트북 구성

| 노트북 | 내용 |
| --- | --- |
| `서울대공원_T-TEST_효과크기.ipynb` | 비 오는 날 vs 안 오는 날 방문객 수 T-검정 및 Cohen's d(효과크기) 분석 |
| `team_stone_seoul_park2.ipynb` | 요일·강수여부·공휴일 기준으로 데이터 분류 및 연도별 비교 시각화 |
| `team_stone_seoul_park3.ipynb` | 평일/주말/공휴일 분류, Prophet · LSTM · 앙상블을 이용한 방문객 수 예측 |
| `team_stone_seoul_park4.ipynb` | RandomForest 기반 개인 방문객 예측 및 Feature Importance 분석 |
| `final.ipynb` | RandomForest + RandomizedSearchCV 하이퍼파라미터 튜닝 최종 모델 |

## 사용 모델

- **통계 검정**: Welch's T-test, Cohen's d
- **시계열**: Prophet (공휴일 효과 포함), LSTM
- **트리 기반**: RandomForestRegressor (+ RandomizedSearchCV)
- **평가 지표**: MAE, RMSE, MAPE, R²

## 실행 방법

```bash
# 1. 의존성 설치
pip install -r requirements.txt

# 2. Jupyter 실행 후 노트북 열기
jupyter notebook
```

> 노트북은 저장소 기준 같은 폴더의 CSV 파일을 상대경로로 읽습니다.
> Prophet 설치 시 환경에 따라 시간이 걸릴 수 있습니다.

## 폴더 구조

```
.
├── final.ipynb                     # 최종 RandomForest 예측 모델
├── team_stone_seoul_park2.ipynb    # 분류 및 비교 시각화
├── team_stone_seoul_park3.ipynb    # Prophet / LSTM / 앙상블 예측
├── team_stone_seoul_park4.ipynb    # RandomForest + Feature Importance
├── 서울대공원_T-TEST_효과크기.ipynb  # 강수여부 T-검정
├── *_입장객_*.csv                   # 연도별 입장객 데이터
├── *과천시_강우량.csv                # 연도별 강수량 데이터
├── requirements.txt
└── README.md
```

## 참고

- 한글 그래프 출력을 위해 Windows 기준 `Malgun Gothic` 폰트를 사용합니다.
  (다른 OS에서는 `plt.rcParams['font.family']` 값을 환경에 맞게 변경하세요.)
