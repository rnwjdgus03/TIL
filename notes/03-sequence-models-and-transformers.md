# 03. Sequence Models & Transformers

## 1. Seq2Seq

Seq2Seq는 입력 시퀀스를 Encoder가 읽고, Decoder가 출력 시퀀스를 생성하는 구조입니다.

```text
Input Sequence → Encoder → Context Vector → Decoder → Output Sequence
```

기계 번역처럼 입력과 출력의 길이가 다른 문제를 처리할 수 있습니다.

### 한계: 고정 길이 Context Vector

기본 Seq2Seq는 모든 입력 정보를 하나의 context vector에 압축합니다. 문장이 길어지면 앞부분의 정보가 손실되고, Decoder가 필요한 입력 위치를 직접 찾기 어렵습니다.

## 2. Attention

Attention은 Decoder가 출력 토큰을 만들 때 Encoder의 모든 hidden state를 비교해 중요한 위치에 더 큰 가중치를 부여합니다.

```text
Query × Key → Attention Score
Attention Score × Value → Context
```

- **Query**: 현재 찾고 싶은 정보
- **Key**: 각 입력이 어떤 정보를 나타내는지 비교하는 기준
- **Value**: 실제로 가져올 정보

### Dot-Product Attention

Query와 Key의 내적으로 유사도를 계산합니다.

```text
Attention(Q, K, V) = softmax(QKᵀ / √dₖ)V
```

`√dₖ`로 나누는 이유는 차원이 커질수록 내적값이 커져 softmax가 지나치게 뾰족해지는 것을 막기 위해서입니다.

### Multi-Head Attention

여러 attention head가 서로 다른 표현 공간에서 관계를 학습합니다. 어떤 head는 문법 관계를, 다른 head는 장거리 의미 관계를 포착할 수 있습니다.

## 3. Transformer가 등장한 이유

Attention을 결합한 Seq2Seq도 Encoder와 Decoder 내부에서 RNN을 사용하면 순차 계산과 장기 의존성 문제가 남습니다. Transformer는 recurrence를 제거하고 attention만으로 토큰 간 관계를 계산합니다.

### 장점

- 전체 토큰을 병렬로 처리할 수 있습니다.
- 멀리 떨어진 토큰도 한 번의 attention으로 연결합니다.
- 대규모 데이터와 연산 자원을 효과적으로 활용합니다.

## 4. Transformer 구조

### Encoder

```text
Token Embedding + Position Information
→ Multi-Head Self-Attention
→ Add & Norm
→ Feed Forward Network
→ Add & Norm
```

### Decoder

```text
Masked Self-Attention
→ Encoder-Decoder Attention
→ Feed Forward Network
→ Next-token Distribution
```

Masked Self-Attention은 현재 위치에서 미래 토큰을 볼 수 없게 해 자기회귀 생성을 가능하게 합니다.

## 5. Position Information

Self-Attention 자체는 토큰의 순서를 구분하지 못하므로 위치 정보를 추가해야 합니다.

- 원본 Transformer는 sin/cos 기반의 고정 Positional Encoding을 사용했습니다.
- BERT는 학습 가능한 Positional Embedding을 사용합니다.
- 최신 모델은 RoPE와 같은 상대적 위치 표현을 사용하기도 합니다.

## 6. BERT

BERT는 Transformer Encoder를 쌓아 양쪽 문맥을 함께 반영하는 언어 표현을 학습합니다.

### 등장 배경

기존의 단방향 언어 모델은 현재 토큰을 표현할 때 한쪽 문맥만 활용했습니다. BERT는 문장 양쪽 정보를 동시에 활용해 문맥에 따른 단어 의미를 더 잘 구분합니다.

### Masked Language Modeling

입력 일부를 `[MASK]`로 가리고 원래 토큰을 예측합니다.

```text
나는 오늘 [MASK]에 갔다.
→ 학교
```

이 방식으로 정답 토큰의 왼쪽과 오른쪽 문맥을 모두 사용할 수 있습니다.

### 활용

- 문장 분류
- 개체명 인식
- 검색과 문장 유사도
- Extractive Question Answering

BERT는 문장을 이해하고 표현하는 Encoder 계열 task에 강하지만 기본 구조만으로 자유로운 장문 생성에는 적합하지 않습니다.

## 7. GPT

GPT는 Transformer Decoder 계열의 자기회귀 언어 모델입니다. 이전 토큰을 바탕으로 다음 토큰의 확률을 예측합니다.

```text
P(x₁, x₂, ..., xₙ)
= P(x₁)P(x₂|x₁)...P(xₙ|x₁...xₙ₋₁)
```

### Transformer Decoder와의 차이

원본 Transformer Decoder는 Encoder의 출력에 attention하는 cross-attention 층을 포함합니다. Decoder-only GPT는 별도 Encoder 없이 masked self-attention으로 입력과 출력을 하나의 토큰 시퀀스로 처리합니다.

### GPT의 특징

- 다음 토큰 예측으로 대규모 사전학습
- prompt의 문맥을 이용한 zero-shot / few-shot 수행
- 요약, 번역, 질의응답, 코드 생성 등 다양한 생성 task 수행
- 모델과 데이터 규모가 커지면서 새로운 task에 대한 범용성 증가

## 8. BERT와 GPT 비교

| Category | BERT | GPT |
|---|---|---|
| Base block | Transformer Encoder | Transformer Decoder |
| Context | Bidirectional | Causal / Left-to-right |
| Pretraining | Masked token prediction | Next-token prediction |
| Strength | Understanding and representation | Generation and instruction following |
| Typical tasks | Classification, NER, retrieval | Chat, writing, summarization, code generation |

## Summary

- Seq2Seq는 Encoder와 Decoder로 시퀀스 변환을 구성했습니다.
- Attention은 출력마다 필요한 입력 정보를 직접 선택합니다.
- Transformer는 recurrence를 제거해 병렬 처리와 장거리 관계 학습을 개선했습니다.
- BERT는 양방향 Encoder 표현, GPT는 자기회귀 Decoder 생성에 초점을 둡니다.
