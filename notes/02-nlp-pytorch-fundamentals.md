# 02. NLP & PyTorch Fundamentals

## 1. 텍스트 전처리가 필요한 이유

컴퓨터는 문장의 의미를 그대로 이해하지 못하므로 텍스트를 일정한 단위로 나누고 숫자로 변환해야 합니다. 전처리는 데이터의 잡음을 줄이고 같은 의미의 표현을 일관된 형태로 만드는 과정입니다.

## 2. Cleaning

분석과 학습에 필요하지 않은 문자를 제거합니다.

```text
I REALLY love Python!!! It is sooooo good :)
→ I REALLY love Python It is sooooo good
```

대표적인 제거 대상은 HTML 태그, URL, 불필요한 특수문자, 중복 공백과 제어 문자입니다. 다만 감성 분석처럼 이모지나 문장부호가 의미를 가질 때는 무조건 제거하면 안 됩니다.

## 3. Normalization

같은 의미를 가진 표현을 가능한 한 같은 형태로 통일합니다.

```text
LOVE / Love / love → love
running / runs / ran → run
```

대표 기법:

- 대소문자 통일
- 반복 문자 정리
- 철자와 띄어쓰기 교정
- stemming과 lemmatization
- Unicode 정규화

## 4. Tokenization

텍스트를 모델이 처리할 기본 단위인 token으로 나눕니다.

### 문장 단위

```text
안녕하세요? 텍스트 전처리 시간입니다.
→ [안녕하세요?] [텍스트 전처리 시간입니다.]
```

### 어절 단위

```text
텍스트 전처리 시간입니다
→ [텍스트] [전처리] [시간입니다]
```

### 형태소 단위

한국어는 조사와 어미가 단어에 붙기 때문에 형태소 분석을 사용하면 의미 단위를 더 세밀하게 나눌 수 있습니다.

### Subword 단위

BPE, WordPiece, SentencePiece는 자주 등장하는 문자열을 하나의 토큰으로 유지하고 희귀 단어는 더 작은 단위로 나눕니다.

장점:

- 미등록 단어 문제를 줄입니다.
- 단어 사전 크기를 관리할 수 있습니다.
- 어근과 접사의 정보를 일부 공유할 수 있습니다.

## 5. Vocabulary, Encoding, Padding

토큰을 vocabulary의 정수 ID로 변환한 뒤 모델 입력 길이에 맞춥니다.

```text
[CLS] 오늘 날씨 좋다 [SEP]
→ [101, 3421, 7632, 2201, 102]
```

- **Padding**: 짧은 입력에 `[PAD]` 토큰을 추가
- **Truncation**: 최대 길이를 넘는 토큰을 자름
- **Attention Mask**: 실제 토큰과 padding을 구분

전처리는 모델별 tokenizer 설정과 일치해야 합니다. 다른 모델의 tokenizer와 vocabulary를 섞으면 입력 ID의 의미가 달라집니다.

## 6. PyTorch

PyTorch는 Tensor 연산, 자동 미분, 모델 정의, 데이터 로딩과 GPU 가속을 제공하는 딥러닝 프레임워크입니다.

### 핵심 구성 요소

- **Tensor**: scalar, vector, matrix를 일반화한 다차원 배열
- **Dataset**: 하나의 샘플을 읽는 방법 정의
- **DataLoader**: batching, shuffle, 병렬 로딩 담당
- **nn.Module**: 모델의 layer와 forward 연산 정의
- **Autograd**: 계산 그래프를 추적해 gradient 자동 계산
- **Optimizer**: gradient를 이용해 파라미터 갱신

### Tensor와 GPU

```python
device = "cuda" if torch.cuda.is_available() else "cpu"
x = x.to(device)
model = model.to(device)
```

모델과 입력 Tensor가 같은 device에 있어야 연산할 수 있습니다.

### Dataset과 DataLoader

```python
loader = DataLoader(
    dataset,
    batch_size=32,
    shuffle=True,
)
```

DataLoader는 데이터를 batch로 나누고 학습 순서를 섞어 안정적인 학습을 돕습니다.

### Autograd

`loss.backward()`는 손실에서 각 학습 파라미터까지 계산 그래프를 따라 gradient를 구합니다. 다음 step 전에 `optimizer.zero_grad()`로 이전 gradient를 초기화해야 합니다.

## 7. NLP Task

### Text Classification

텍스트 전체를 하나의 범주로 분류합니다.

예:

- 뉴스 주제 분류
- 감성 분석
- 스팸 탐지
- 의도 분류

일반적인 데이터 구조:

```python
{"text": "Wall Street shares rose...", "label": 2}
```

### Token Classification

문장 전체가 아니라 각 토큰에 label을 부여합니다. 개체명 인식, 품사 태깅 등에 사용합니다.

### Question Answering

질문과 문맥을 입력받아 답이 있는 구간을 찾거나 답변을 생성합니다.

### Summarization

긴 문서의 핵심 내용을 유지하면서 짧은 텍스트로 생성합니다.

### Translation

한 언어의 시퀀스를 다른 언어의 시퀀스로 변환합니다. Seq2Seq와 Attention 발전에 중요한 task였습니다.

## 8. Hugging Face 생태계

- **Hub**: 사전학습 모델과 데이터셋 공유
- **Transformers**: Transformer 기반 모델과 tokenizer 제공
- **Datasets**: 데이터셋 다운로드와 전처리
- **Tokenizers**: 빠른 subword tokenization

모델과 tokenizer는 같은 checkpoint를 사용해야 합니다.

```python
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModel.from_pretrained(model_name)
```

## Summary

- NLP 입력은 정제, 정규화, 토큰화, 정수 인코딩과 padding을 거칩니다.
- 전처리 기준은 task와 모델에 따라 달라집니다.
- PyTorch는 데이터 로딩부터 자동 미분과 파라미터 갱신까지 학습 흐름을 구성합니다.
- NLP task에 따라 label과 모델 출력 구조가 달라집니다.
