# SHAP Variable Contribution

이미지 파일: shap_variable_contribution.png

공통 기준
- 이 폴더는 benchmark 폴더 안에 있지만, 그래프 값은 벤치마크용 LightGBM이 아니라 실제 웹사이트에 연결된 운영 모델을 기준으로 생성하였다.
- 운영 모델: ml/artifacts/risk_model.joblib
- 모델 유형: LightGBM
- 운영 임계값: 0.590
- 참고 라벨: PHQ-9 합산점수 10점 이상을 Reference high로 정의한 이진 분류 문제이다.
- 해석 주의: 이 모델은 우울증 확정 진단 도구가 아니라, 상담과 추가 점검이 필요한 위험 신호를 조기에 확인하기 위한 보조 도구이다.

그림이 보여주는 것
이 그림은 실제 웹사이트에 연결된 운영 LightGBM 모델에서 각 변수가 예측값을 얼마나 크게 움직였는지 평균 절댓값 SHAP 기준으로 보여준다. 값이 클수록 전체 예측에서 해당 변수의 영향 크기가 컸다는 뜻이다.

벤치마크 용어 설명
- Mean absolute SHAP: 각 사례에서 변수의 SHAP 기여도를 절댓값으로 바꾼 뒤 평균낸 값이다. 방향이 아니라 영향의 크기를 본다.
- Global importance: 전체 데이터에서 어떤 변수가 모델 예측에 많이 기여했는지 보는 전역 중요도이다.
- Direction: 변수가 High 예측을 높였는지 낮췄는지는 mean absolute SHAP만으로 알 수 없다. 방향성은 signed SHAP 분석이 필요하다.

결과 해석
운영 모델 기준 상위 변수는 stress_level 1.050, subjective_health 0.650, sex 0.233, body_self_overweight 0.199, smoking_status 0.196, household_size 0.183 순으로 나타났다. 이는 스트레스 수준과 주관적 건강상태가 예측값 변화에 가장 크게 기여했다는 의미이다. 성별, 체형 인식, 흡연 여부, 가구원 수 등도 보조적으로 기여했다.

보고서에 넣을 문장
"운영 LightGBM 모델의 평균 절댓값 SHAP 분석 결과, stress_level과 subjective_health가 예측값을 가장 크게 변화시킨 변수로 나타났다. 본 결과는 인과관계가 아니라 모델 예측에 기여한 변수 패턴을 의미한다."

주의점
이 그래프는 실제 운영 모델 기준이다. 단, mean absolute SHAP은 영향 크기만 보여주므로 여성, 비만, 흡연 등이 어느 방향으로 작용했는지를 설명하려면 signed SHAP 결과를 별도로 확인해야 한다.
