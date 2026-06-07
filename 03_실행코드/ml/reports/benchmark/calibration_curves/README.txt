# Calibration Curves

이미지 파일: calibration_curves.png

공통 벤치마크 기준
- 데이터: 지역사회건강조사 재범주화 데이터, 19세 이상 39세 이하 청년층 표본 134,672건.
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high, 10점 미만을 Reference low로 정의하였다.
- 클래스 분포: Reference high는 5,468건으로 전체의 4.1%이고, Reference low는 129,204건으로 전체의 95.9%이다.
- 벤치마크 목적: Logistic Regression, Random Forest, XGBoost, LightGBM을 같은 데이터 분할과 같은 전처리 조건에서 비교해 어떤 모델이 선별 목적에 적합한지 확인하는 것이다.
- 해석 주의: 벤치마크 결과는 모델 비교용이다. 실제 웹사이트에 연결된 운영 모델은 ml/artifacts/risk_model.joblib의 LightGBM이며 운영 임계값은 0.590이다. 벤치마크 LightGBM은 임계값 0.588 기준이므로 운영 모델과 매우 가깝지만 완전히 같은 산출물은 아니다.

그림이 보여주는 것
Calibration curve는 모델이 출력한 예측확률과 실제 Reference high 비율이 얼마나 잘 맞는지 확인하는 그래프이다. x축은 모델이 예측한 평균 확률, y축은 해당 구간에서 실제 High였던 비율이다. 점들이 대각선에 가까울수록 확률값이 잘 보정되어 있다고 본다.

벤치마크 용어 설명
- Calibration: 예측확률이 실제 발생 비율과 얼마나 일치하는지 보는 개념이다.
- Brier score: 예측확률과 실제 라벨의 차이를 요약한 지표이다. 낮을수록 확률 보정이 좋다.
- Discrimination: High와 Low를 구분하는 능력이다. AUROC, PR-AUC는 주로 discrimination을 본다.

결과 해석
Brier score는 Random Forest 0.062로 가장 낮고, LightGBM은 0.138, XGBoost는 0.147, Logistic Regression은 0.152로 나타났다. 그러나 Brier score가 낮다고 해서 선별 성능이 가장 좋다는 뜻은 아니다. 본 프로젝트에서는 예측확률을 임상적 발생확률로 그대로 해석하기보다, 상대적 위험 신호와 상담 참고 정보로 사용하는 것이 더 적절하다.

보고서에 넣을 문장
"보정 곡선 분석 결과, 모델의 출력 확률은 실제 임상적 발생확률로 직접 해석하기보다 상대적 위험 순위와 상담 참고 신호로 해석하는 것이 적절하다. 따라서 웹서비스에서는 확률값 자체보다 위험 단계와 주요 기여 요인을 함께 제공한다."

주의점
사용자에게 70%라고 보여주면 실제 우울증 발생확률 70%처럼 오해할 수 있다. 따라서 결과 표현은 위험도 점수 또는 상담 필요 신호로 완화하는 것이 안전하다.
