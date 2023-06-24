# ICR_kaggle_Challenge

## 대회 개요
- 익명화된 건강 특성 데이터를 사용하여 세 가지 의학적 상태 중 1개 이상을 가지고있는지(클래스 1) 가지고 있지 않은지(클래스 0) 예측하는 모델을 만드는 것이다.
- 이 분석은 연구자들이 건강 특성의 측정돠 잠재적인 환자 상태의 관계를 발견하는 데 도움이 될 것으로본다.
  
---
## 데이터 셋 설명
### train.csv
- Id : 각 환자의 고유 식별자
- AB - GL : 56개의 익명화된 건강 특성, EJ는 범주형('A', 'B')로 구성되어 있다.
- Class : 세 가지 의학적 상태 중 1개 이상이 포함되 있는 경우(클래스 1), 그렇지 않다면(클래스 0)

### test.csv
- Id : 각 환자의 고유 식별자
- AB - GL : 56개의 익명화된 건강 특성, EJ는 범주형('A', 'B')로 구성되어 있다
- test data set은 추론(모델 분석 결과)을 위한 데이터이기 때문에 Class가 존재하지 않는다.

### greeks.csv
#### train.csv에 추가하여 사용할 수 있는 메타데이터입니다. 간단하게 보충 데이터라고 이해하시면 됩니다.
#### Alpha
- A : 연령 관련 조건이 존재하지 않음을 뜻하므로 train.csv에서 해당 식별자의 클래스는 0이다.
- B, D, G : 세 가지 연령 관련 조건이 존재한다는 뜻이므로 train.csv에서 해당 식별자의 클래스는 1이다.
#### Beta, Gamma, Delta 세 가지 실험 특성
#### Epsilon은 데이터가 수집된 날짜입니다.

### sample_submission.csv
- 올바른 데이터 제출 형식입니다. 매우 중요하니 두 번 보세요.

---
## 평가 지표
### Why use to LogLoss?
- 1. 확률적 예측: 의료 데이터에서 예측은 종종 확률적인 측면에서 이루어집니다. 예를 들어, 환자가 특정 질병에 걸릴 확률이나 특정 치료법의 성공 확률을 예측하는 경우가 있습니다. 로그 손실은 모델이 예측한 확률과 실제 결과 사이의 차이를 측정하여 모델의 예측 정확도를 평가합니다. 이를 통해 모델의 확률 예측이 실제 결과와 얼마나 일치하는지 확인할 수 있습니다.

- 2. 클래스 불균형 데이터 처리: 의료 데이터는 종종 클래스 간 불균형을 가지는 경우가 많습니다. 예를 들어, 흔한 질병과 드물게 발생하는 질병 사이에 샘플 수의 불균형이 있을 수 있습니다. 로그 손실은 클래스 불균형 데이터셋에서도 잘 동작하며, 각 클래스의 확률을 고려하여 모델의 예측을 평가합니다. 이를 통해 모델이 드문 클래스에 대해서도 정확한 예측을 할 수 있는지 확인할 수 있습니다.

- 3. 모델의 확률 보정: 의료 데이터에서 모델의 예측 확률은 중요한 의사 결정에 영향을 미칠 수 있습니다. 로그 손실은 모델의 예측 확률과 실제 결과 사이의 차이를 측정하므로, 모델의 예측을 보정하는 데 사용될 수 있습니다. 예를 들어, 모델이 낮은 확률로 예측한 결과를 더 정확하게 보정하여 높은 신뢰도로 예측을 수행할 수 있습니다.

- 4. 최적화 및 모델 선택: 로그 손실은 모델의 성능을 측정하고 비교하는 데 사용됩니다. 의료 데이터에서는 다양한 모델을 비교하고 최적의 모델을 선택하는 것이 중요합니다. 로그 손실은 모델의 예측과 실제 결과 사이의 차이를 정량화하여 모델의 정확도를 평가하므로, 최적의 모델을 선택하는 데 도움을 줍니다.

- 따라서, 의료 데이터에서 로그 손실은 확률적인 예측, 클래스 불균형 데이터 처리, 모델의 확률 보정, 최적화 및 모델 선택에 유용하게 사용됩니다. 이를 통해 모델의 예측 정확도를 평가하고 신뢰도 있는 의사 결정을 할 수 있습니다.

---
### Log Loss(교차 엔트로피)
### 정의
- 분류 문제에서 사용되는 평가 지표 중 하나로, 모델의 예측값과 실제값간의 오차를 계산하는데 사용합니다.

### 수식
$$Log Loss = -\frac{1}{N}\sum_{i=1}^N(y_{i}logp_{i} + (1-y_{i})log(1-p_{i}))$$
$$ = -\frac{1}{N}\sum_{i=1}^N logp_{i}^{'}$$

- $y_i$는 양성인지 아닌지를 표시하는 레이블(양성 : 1, 음성 : 0)을, $p_i$는 각 행 데이터가 양성일 예측 확률을 나타낸다. $p_{i}^{'}$는 실제값을 예측하는 확률로, 실젯값이 양성일 경우는 $p_i$이고 음성일 경우 $1-p_i$이다.

- 기본적으로 Log Loss가 낮을수록 모델이 분류를 잘 수행한 것으로 파악한다.

### Log Loss의 계산 예시

|ID|실젯값(양성/음성)|양성의 예측 확률|실젯값의 예측 확률$p_{i}^{'}$|$-log(p_{i}^{'})$|
|-|-|-|-|-|
|1|1|0.9|0.9|0.105|
|2|1|0.5|0.5|0.693|
|3|0|0.1|0.9|0.105|

Log Loss = 0.301(평균)

- 각 행 데이터가 양성일 확률을 낮게 예측했음에도 양성(1)이거나, 양성일 확률을 높게 예측했음에도 음성(0)일 경우에는 패널티가 주어집니다. 아래 그래프 참고

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FJgy2W%2FbtrUfISvUpo%2FKJZCnpU3kmQLsdrd8Zg1B1%2Fimg.png)

##### Log Loss 출처 : 데이터가 뛰어노는 AI 놀이터, 캐글(가도와키 다이스케, 사카타 류지, 호사카 게이스케, 히라마쓰 유지) 102p ~ 104p
##### image 출처 : https://leehah0908.tistory.com/22
