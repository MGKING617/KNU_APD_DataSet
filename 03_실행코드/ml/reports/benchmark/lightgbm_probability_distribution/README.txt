# LightGBM Probability Distribution

이미지 파일: lightgbm_probability_distribution.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
이 그림은 LightGBM이 테스트셋 각 사례에 부여한 Reference high 예측확률의 분포를 실제 Low와 실제 High로 나누어 보여준다. 두 분포가 많이 겹치면 분류가 어려운 사례가 많다는 뜻이고, 분리되어 있으면 모델이 두 집단을 잘 구분한다는 뜻이다.

벤치마크 용어 설명
- Predicted probability: 모델이 해당 사용자를 Reference high로 볼 확률 또는 위험 점수처럼 산출한 값이다.
- Distribution overlap: Low와 High의 예측확률 분포가 겹치는 영역이다. 겹치는 영역에서는 오분류가 발생하기 쉽다.
- Threshold: 분포 위에서 High와 Low를 나누는 기준선 역할을 한다.

결과 해석
실제 Reference high의 예측확률은 대체로 실제 Reference low보다 높게 분포한다. 테스트셋에서 실제 Low의 예측확률 중앙값은 약 0.149이고, 실제 High의 중앙값은 약 0.768이다. 다만 두 분포가 완전히 분리되지는 않으므로 일부 Low가 High로 예측되거나 일부 High가 Low로 놓칠 수 있다.

보고서에 넣을 문장
"LightGBM 예측확률 분포에서 실제 고위험군은 전반적으로 더 높은 예측확률을 보였으나, 저위험군과 고위험군의 분포가 일부 겹쳐 false positive와 false negative가 발생할 수 있음을 확인하였다."

주의점
예측확률은 임상적 확률이 아니라 모델 내부 점수에 가깝다. 사용자에게는 확률 자체보다 위험 단계와 상담 권장 메시지로 전달하는 것이 안전하다.
