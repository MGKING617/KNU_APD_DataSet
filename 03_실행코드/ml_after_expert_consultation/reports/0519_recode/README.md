# 0519 지역사회건강조사 재범주화 재학습 기록

## 목적

`0519) 지역사회건강조사 재범주화_수정본.xlsx`의 원자료 시트와 `cut-off 기준` 시트를 기준으로 학습 데이터를 다시 만들고, 기존 모델 학습 및 벤치마크 산출물을 같은 위치에 재생성했다. 이 모델은 청년 우울 관련 위험도와 상태 변화 참고 지표를 제공하기 위한 보조 모델이며, 의료적 판단을 대체하지 않는다.

## 입력 파일

- 원본 복사본: `data/raw/0519) 지역사회건강조사 재범주화_수정본.xlsx`
- 데이터 시트: `0519) 지역사회건강조사 재범주화`
- 기준 시트: `cut-off 기준`
- processed dataset: `data/processed/chs_training_0519_recoded.csv`

기존에 먼저 분석했던 `data/raw/0519) 지역사회건강조사 재범주화.xlsx`와 `outputs/0519_recode_audit`는 지우지 않고 보존했다.

## Cut-Off 적용

`cut-off 기준` 시트의 색상은 셀 전체 서식이 아니라 리치텍스트 글꼴 색상으로 들어 있었다.

- 빨간색: `FFFF0000`
- 파란색: `FF0070C0`
- 재범주화 결과 열에서는 빨간색이 `0`, 파란색이 `1`에 대응하는 구조로 확인했다.
- 수정본 데이터 시트에는 대부분의 변수가 이미 재범주화된 값으로 들어 있으므로, 학습용 CSV는 해당 재범주화 값을 그대로 사용했다.

`본인인지체형`은 기준표와 데이터 모두 `0=마름`, `1=보통`, `2=비만`의 3범주였다. 이 값은 하나의 순서형 숫자로 넣지 않고 다음 두 이분형 변수로 분리했다.

- `body_self_thin`: `본인인지체형 == 0`이면 1
- `body_self_overweight`: `본인인지체형 == 2`이면 1
- `본인인지체형 == 1`이면 두 변수 모두 0
- 결측은 두 변수 모두 결측으로 유지하고, 모델 pipeline의 imputer가 처리한다.

## Target 생성

새 파일에는 `mh_PHQ_S`가 직접 들어 있지 않으므로, 기존 방식과 동일하게 `mtb_07a1`부터 `mtb_07i1`까지 9개 문항을 사용했다.

- 원값 `1~4`를 `0~3`으로 변환
- 9개 문항이 모두 유효할 때 합산
- `mh_PHQ_S >= 10`을 positive class로 사용
- PHQ 9개 문항은 target 계산에만 쓰고 feature에서는 제외

## 데이터 검증

- 원본 row 수: `134,798`
- 최종 학습 row 수: `134,672`
- 제외 row 수: `126`
- 제외 사유: PHQ 9개 문항 중 유효하지 않은 응답 포함
- 최종 feature 수: `20`
- positive rows: `5,468`
- negative rows: `129,204`
- positive rate: `4.06%`
- 이분형이어야 하는 feature 중 0/1 외 값이 남은 컬럼: 없음
- 단일값만 남은 feature: 없음

검증 산출물:

- `ml/reports/0519_recode/validation_report_0519.json`
- `ml/reports/0519_recode/feature_mapping_0519.csv`
- `ml/reports/0519_recode/cutoff_mapping_audit_0519.csv`

## Feature 목록

최종 feature는 `ml/artifacts/feature_schema.json`과 processed CSV에 저장되어 있다.

- `age`
- `household_size`
- `subjective_health`
- `smoking_status`
- `alcohol_frequency`
- `walking_days`
- `body_self_thin`
- `body_self_overweight`
- `weight_control`
- `hypertension_diagnosis`
- `diabetes_diagnosis`
- `accident_poisoning_experience`
- `basic_livelihood_recipient`
- `breakfast_frequency`
- `economic_activity`
- `sex`
- `region_group`
- `education_level`
- `marital_status`
- `stress_level`

## 재학습 결과

FastAPI가 로드하는 최종 모델 artifact는 기존 위치를 유지했다.

- `ml/artifacts/risk_model.joblib`
- `ml/artifacts/feature_schema.json`
- `ml/artifacts/metrics.json`
- `ml/artifacts/feature_importance.json`
- `ml/artifacts/top_features.json`
- `ml/artifacts/shap_summary.json`

LightGBM 단일 학습 주요 결과:

- AUROC: `0.8523`
- PR-AUC: `0.2190`
- optimized positive precision: `0.1581`
- optimized positive recall: `0.7230`
- optimized positive F1: `0.2595`

## 벤치마크 결과

기존 벤치마크 스크립트와 저장 위치를 유지해 다음 모델을 다시 평가했다.

| model | AUROC | PR-AUC | positive precision | positive recall | positive F1 |
| --- | ---: | ---: | ---: | ---: | ---: |
| Logistic Regression | 0.8615 | 0.2235 | 0.1619 | 0.7267 | 0.2648 |
| LightGBM | 0.8512 | 0.2177 | 0.1583 | 0.7194 | 0.2595 |
| XGBoost | 0.8585 | 0.2249 | 0.1632 | 0.7048 | 0.2651 |
| Random Forest | 0.8452 | 0.1856 | 0.1487 | 0.7038 | 0.2455 |

재생성 위치:

- `ml/reports/benchmark/benchmark_metrics.csv`
- `ml/reports/benchmark/benchmark_metrics.json`
- `ml/reports/benchmark/benchmark_report.md`
- `ml/reports/benchmark/confusion_matrices.png`
- `ml/reports/benchmark/confusion_matrices_normalized.png`
- `ml/reports/benchmark/roc_curves.png`
- `ml/reports/benchmark/precision_recall_curves.png`
- `ml/reports/benchmark/metrics_comparison.png`
- `ml/reports/benchmark/positive_class_metrics.png`
- `ml/reports/benchmark/calibration_curves.png`
- `ml/reports/benchmark/lightgbm_threshold_tradeoff.png`
- `ml/reports/benchmark/lightgbm_probability_distribution.png`
- `ml/reports/benchmark/learning_curves.png`
- `ml/reports/benchmark/decision_curve_analysis.png`
- `ml/reports/benchmark/lightgbm_outcome_group_contributions.png`
- `ml/reports/benchmark/feature_importance_*.png`
- `ml/reports/benchmark/*_pipeline.joblib`

기존 artifact와 benchmark 폴더는 실행 전에 `outputs/ml_backup_20260519_201009` 아래로 백업했다.

## FastAPI 확인

`ml/src/serve_model.py`의 `/predict`를 FastAPI `TestClient`로 호출해 새 `risk_model.joblib`와 `feature_schema.json` 로드를 확인했다.

반환 확인 항목:

- `riskPercent`
- `topFactors`
- `topContributions`
- `globalImportance`

API 요청 구조 `{ "features": { ... } }`와 응답 schema는 바꾸지 않았다.

프론트엔드의 `buildModelFeatures`는 기존 원자료 코드명도 유지하면서, 새 0519 모델이 직접 사용하는 일부 feature를 추가로 보낸다. 설문에서 직접 해석 가능한 항목만 매핑했고, 앱에서 묻지 않는 정확한 원자료 변수는 억지로 채우지 않는다.

추가 매핑 예:

- `D_1_1` 응답 -> `subjective_health`
- `BP1` 응답 -> `stress_level`
- `BS3_1` 응답 -> `smoking_status`
- `BD1_11` 응답 -> `alcohol_frequency`
- `BE5_1` 또는 `BE3_31` 응답 -> `walking_days`
- `BO2_1` 응답 -> `weight_control`
- `allownc` 응답 -> `basic_livelihood_recipient`
- `DI1_dg` 응답 -> `hypertension_diagnosis`
- `DE1_dg` 응답 -> `diabetes_diagnosis`
- `L_BR_FQ` 응답 -> `breakfast_frequency`

## 재현 명령

로컬 Python 의존성이 준비되어 있으면 아래 명령으로 전처리, 학습, 벤치마크를 다시 실행할 수 있다.

```powershell
.\scripts\retrain-0519.ps1
```

직접 실행할 경우:

```powershell
$env:PYTHONPATH=(Resolve-Path 'outputs\python_deps').Path
& "$env:USERPROFILE\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe" ml\src\prepare_0519_recoded_training_data.py --input "data\raw\0519) 지역사회건강조사 재범주화_수정본.xlsx" --output data\processed\chs_training_0519_recoded.csv --report-dir ml\reports\0519_recode
& "$env:USERPROFILE\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe" ml\src\train_model.py --input data\processed\chs_training_0519_recoded.csv --output-dir ml\artifacts
& "$env:USERPROFILE\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe" ml\src\benchmark_models.py --input data\processed\chs_training_0519_recoded.csv --output-dir ml\reports\benchmark
```

## 주의사항

- PHQ 9개 문항을 feature에 넣으면 target과 직접 연결되므로 이번 학습에서는 제외했다.
- `본인인지체형`을 0/1/2 단일 숫자로 넣으면 마름, 보통, 비만 사이에 단순 순서가 있다고 모델이 받아들일 수 있어 두 이분형 feature로 분리했다.
- 이번 파일은 교수님 cut-off 기준이 적용된 재범주화 파일이므로, 이전 0511/0515 processed CSV와 성능을 1:1로 해석할 때는 전처리 기준 차이를 함께 봐야 한다.
- 0519 기준으로 모든 feature가 이분형 또는 연속형으로 정리되면서 일부 세부 정보량은 줄어들 수 있다.
