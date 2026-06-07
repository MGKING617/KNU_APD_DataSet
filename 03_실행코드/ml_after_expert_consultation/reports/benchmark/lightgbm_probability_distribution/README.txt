# LightGBM 예측확률 분포

이미지 파일: lightgbm_probability_distribution.png

공통 벤치마크 기준
- 데이터: 전문가 상담 후 재전처리한 지역사회건강조사 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 변수 변경: 가구원 수는 인원수 그대로 쓰지 않고 1인 가구 여부(single_person_household)로 재범주화하였다.
- 제외 변수: 주관적 건강인식, 체형인식(마름/비만), 아침식사 빈도, 교육수준은 전문가 상담 내용을 반영해 입력 변수에서 제외하였다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 임계값 기준: 이번 전문가 상담 후 실험에서는 모든 모델의 예측 확률 임계값을 0.590으로 고정하였다.
- 해석 주의: 이 폴더의 결과는 기존 ml 폴더를 대체한 것이 아니라, 전문가 상담 이후 변수 구성을 바꿔 다시 돌린 별도 비교 실험이다.

그림이 보여주는 것
이 그림은 LightGBM이 실제 Reference low와 Reference high 표본에 대해 어떤 예측 확률을 냈는지 분포로 보여준다. 두 분포가 많이 겹치면 구분이 어려운 표본이 많다는 뜻이다.

벤치마크 용어 설명
- Predicted probability: 모델이 Reference high일 가능성으로 출력한 값이다.
- Threshold: 확률이 이 값 이상이면 High로 분류하는 기준이다.
- 분포 겹침: Low와 High의 예측 확률이 비슷한 구간에 몰려 있는 정도이다.

결과 해석
이번 LightGBM 평가는 임계값 0.590을 기준으로 하며, 테스트셋에서 AUROC 0.835, PR-AUC 0.196, recall 0.748을 보였다.

보고서에 넣을 문장
"LightGBM 예측확률 분포를 통해 Reference low와 high가 어느 정도 분리되는지 확인하였다."

주의점
예측 확률은 참고 위험 점수이지 진단 확률이 아니다. 임계값 0.590은 이번 비교 실험을 위해 고정한 기준이다.
