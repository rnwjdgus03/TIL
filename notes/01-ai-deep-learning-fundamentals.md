# 01. AI & Deep Learning Fundamentals

## 1. 인공지능이란 무엇인가

인공지능은 기계가 인간의 지능적인 행동을 모방해 학습하고, 추론하고, 문제를 해결하도록 만드는 기술입니다.

### AI의 범위

- **ANI(Artificial Narrow Intelligence)**: 얼굴 인식, 음성 인식, 추천처럼 특정 작업에 특화된 인공지능
- **AGI(Artificial General Intelligence)**: 사람처럼 다양한 문제를 범용적으로 해결하는 인공지능
- **ASI(Artificial Superintelligence)**: 인간의 지능을 전반적으로 넘어서는 가설적 인공지능

현재 실제 서비스에서 사용하는 대부분의 AI는 ANI에 해당합니다.

## 2. 머신러닝과 딥러닝

```text
Artificial Intelligence
└── Machine Learning
    └── Deep Learning
```

- 머신러닝은 데이터에서 규칙과 패턴을 학습합니다.
- 딥러닝은 여러 층의 신경망을 사용해 복잡한 표현을 단계적으로 학습합니다.
- Rule-based 시스템은 사람이 규칙을 직접 작성하지만, 딥러닝은 학습 데이터로부터 가중치를 조정합니다.

## 3. 딥러닝 모델의 학습 과정

모델 학습은 손실값을 작게 만드는 방향으로 weight와 bias를 반복해서 수정하는 과정입니다.

```text
Input
→ Forward Pass
→ Prediction
→ Loss Calculation
→ Backward Pass
→ Parameter Update
```

### 3.1 Forward Pass

입력 데이터가 모델의 여러 층을 통과해 예측값을 만듭니다. 학습 중의 순전파와 추론 시 계산 과정은 기본적으로 유사합니다.

### 3.2 Loss 계산

손실 함수는 예측값과 정답의 차이를 하나의 수치로 표현합니다. 학습의 목표는 이 손실값을 최소화하는 것입니다.

### 3.3 Backward Pass

역전파는 손실이 각 파라미터에 미친 영향을 gradient로 계산합니다. 복잡한 합성 함수는 chain rule을 이용해 뒤에서 앞으로 미분합니다.

### 3.4 Update

Optimizer는 계산된 gradient를 이용해 파라미터를 갱신합니다.

```python
for x, y in dataloader:
    optimizer.zero_grad()
    prediction = model(x)
    loss = criterion(prediction, y)
    loss.backward()
    optimizer.step()
```

## 4. 경사하강법

Gradient Descent는 손실 함수의 기울기를 따라 손실이 감소하는 방향으로 파라미터를 이동시키는 방법입니다.

```text
new_weight = old_weight - learning_rate × gradient
```

### Learning Rate

- 너무 크면 최적점을 지나치거나 학습이 불안정해집니다.
- 너무 작으면 학습이 지나치게 느리거나 local minimum 부근에 오래 머물 수 있습니다.

### 대표적인 방식

- **Batch Gradient Descent**: 전체 데이터를 사용해 한 번 갱신
- **Stochastic Gradient Descent**: 샘플 하나마다 갱신
- **Mini-batch Gradient Descent**: 작은 배치 단위로 갱신하며 가장 널리 사용

## 5. RNN

RNN(Recurrent Neural Network)은 순서가 있는 데이터를 처리하기 위해 이전 시점의 hidden state를 다음 시점으로 전달합니다.

```text
x₁ → h₁ → h₂ → h₃
      ↑     ↑     ↑
     x₂    x₃    x₄
```

텍스트, 음성, 시계열처럼 앞의 정보가 뒤의 해석에 영향을 주는 데이터에 적합합니다.

### RNN의 장점

- 입력 순서를 모델링할 수 있습니다.
- 가변 길이 시퀀스를 처리할 수 있습니다.
- 과거 상태를 현재 계산에 반영합니다.

### RNN의 한계

#### 기울기 소실

역전파가 여러 시점을 거치면서 gradient가 반복적으로 작아져 초기 정보가 학습에 거의 반영되지 않을 수 있습니다.

#### 장기 의존성

문장 앞부분의 정보가 멀리 떨어진 뒷부분에 필요할 때 관계를 유지하기 어렵습니다.

#### 순차 연산

이전 시점의 계산이 끝나야 다음 시점을 계산할 수 있어 병렬화가 어렵고, 긴 입력에서 학습과 추론 비용이 커집니다.

## 6. RNN 이후의 발전

- LSTM과 GRU는 gate 구조로 장기 의존성과 기울기 문제를 완화했습니다.
- Seq2Seq는 입력과 출력을 Encoder와 Decoder로 나눴습니다.
- Attention은 모든 입력 상태를 직접 참고해 하나의 고정 벡터에 정보를 압축하는 문제를 줄였습니다.
- Transformer는 recurrence를 제거하고 attention 중심으로 전체 토큰을 병렬 처리했습니다.

## Summary

- 딥러닝 학습은 순전파, 손실 계산, 역전파, 파라미터 갱신의 반복입니다.
- Gradient Descent는 손실이 줄어드는 방향을 찾는 핵심 최적화 방법입니다.
- RNN은 시퀀스를 처리하지만 장기 의존성과 병렬화에 한계가 있습니다.
- 이 한계가 Attention과 Transformer 발전으로 이어졌습니다.
