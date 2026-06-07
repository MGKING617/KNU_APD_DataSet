# 전문가 상담 후 벤치마크 요약

## 전처리 변경 사항

- 기존 `ml` 폴더는 그대로 두고, 별도 실험 폴더 `ml_after_expert_consultation`을 생성하였다.
- 0519 재범주화 데이터를 다시 전처리해 `data/processed/chs_training_0519_recoded_expert_consultation.csv`를 생성하였다.
- 입력 변수 수는 기존 20개에서 15개로 줄었다.
- `household_size`는 `single_person_household`로 재범주화하였다.
- `single_person_household`는 1인 가구면 `1`, 2인 이상 가구면 `0`이다.
- 전문가 상담에서 회의적으로 언급된 `subjective_health`, `body_self_thin`, `body_self_overweight`, `breakfast_frequency`, `education_level`은 제외하였다.
- 참고 라벨은 기존과 동일하게 `mh_PHQ_S >= 10`으로 유지하였다.
- 새 벤치마크에서는 모든 모델의 예측 확률 임계값을 `0.59`로 고정하였다.

## 새 벤치마크 결과

| 모델 | AUROC | PR-AUC | Precision | Recall | F1 | Balanced accuracy |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.839 | 0.194 | 0.128 | 0.793 | 0.221 | 0.783 |
| XGBoost | 0.838 | 0.198 | 0.129 | 0.780 | 0.222 | 0.779 |
| LightGBM | 0.835 | 0.196 | 0.130 | 0.748 | 0.221 | 0.768 |
| Random Forest | 0.809 | 0.137 | 0.151 | 0.419 | 0.222 | 0.660 |

## 기존 `ml` 벤치마크와의 대략적 비교

기존 벤치마크는 모델별로 recall 중심 임계값을 자동 선택했고, 이번 실험은 모든 모델 임계값을 `0.59`로 고정했다. 따라서 임계값의 영향을 받지 않는 AUROC와 PR-AUC를 중심으로 비교하는 것이 더 적절하다.

| 모델 | AUROC 변화 | PR-AUC 변화 | Recall 변화 | F1 변화 |
| --- | ---: | ---: | ---: | ---: |
| Logistic Regression | -0.022 | -0.029 | +0.067 | -0.044 |
| LightGBM | -0.016 | -0.022 | +0.028 | -0.039 |
| XGBoost | -0.020 | -0.027 | +0.075 | -0.044 |
| Random Forest | -0.036 | -0.048 | -0.285 | -0.023 |

## 해석

전문가 상담에서 회의적으로 언급된 변수를 제거하자 AUROC와 PR-AUC는 전반적으로 조금 낮아졌다. 다만 0.59 고정 임계값 기준에서는 Logistic Regression의 recall이 가장 높았고, XGBoost는 PR-AUC가 가장 높았다. LightGBM은 AUROC 0.835, PR-AUC 0.196, recall 0.748로 XGBoost와 비슷한 후보군으로 볼 수 있다.

이번 버전은 단순히 성능을 가장 높이는 목적보다, 전문가 상담 내용을 반영해 변수 해석 타당성을 높인 별도 실험으로 해석하는 것이 적절하다.
