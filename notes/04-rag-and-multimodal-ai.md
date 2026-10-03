# 04. RAG & Multimodal AI

## 1. LLM의 장점

- 하나의 모델로 질의응답, 요약, 번역, 문서와 코드 작성 등 다양한 작업을 수행합니다.
- 자연어를 인터페이스로 사용해 별도의 복잡한 사용법 없이 지시할 수 있습니다.
- Prompt를 변경해 새로운 도메인과 task에 빠르게 적용할 수 있습니다.

## 2. LLM의 구조적 한계

### Knowledge Cutoff

LLM의 parametric knowledge는 학습 데이터가 수집된 시점에 고정됩니다. 학습 이후에 발생한 사건이나 변경된 정보는 외부 검색 없이 알 수 없습니다.

### 도메인 특화 지식 부족

범용 데이터로 학습한 모델은 전문적이거나 공개 데이터가 적은 분야에서 정확한 답을 만들기 어렵습니다.

### Hallucination

LLM은 사실 데이터베이스에서 정답을 조회하는 것이 아니라 다음 토큰의 확률을 예측합니다. 근거가 부족해도 자연스러운 문장을 생성할 수 있습니다.

### 계산과 논리 추론의 제약

정확한 계산, 복잡한 조건 필터링과 일관된 다단계 추론에서 오류가 발생할 수 있습니다. 필요한 경우 계산기, 코드 실행기, 데이터베이스 같은 외부 도구와 결합해야 합니다.

### Context Window

Transformer는 현재 context window 안에 있는 정보만 직접 사용할 수 있습니다.

- 긴 문서가 window를 넘으면 일부 내용을 잘라야 합니다.
- 오래된 대화 내용은 범위 밖으로 밀려날 수 있습니다.
- 기본 모델은 세션이 종료된 뒤 이전 대화를 기억하지 않습니다.

## 3. RAG

RAG(Retrieval-Augmented Generation)는 질문과 관련된 외부 문서를 검색하고, 검색 결과를 LLM의 context에 넣어 답을 생성합니다.

```text
Question
→ Query Embedding
→ Document Retrieval
→ Relevant Context
→ LLM Generation
→ Answer with Evidence
```

Parametric memory만 사용하는 대신 검색 가능한 non-parametric memory를 결합하는 방식입니다.

## 4. RAG 데이터 처리 과정

### 4.1 Load

PDF, 웹 페이지, 데이터베이스, 사내 문서 등에서 원문을 가져옵니다.

### 4.2 Parse and Clean

본문, 제목, 표, 목록과 메타데이터를 추출하고 불필요한 UI 텍스트와 중복 내용을 제거합니다.

### 4.3 Chunk

긴 문서를 검색 가능한 크기로 나눕니다.

- chunk가 너무 작으면 조건과 예외 문맥이 끊깁니다.
- 너무 크면 검색 결과에 불필요한 문장이 많아집니다.
- overlap을 사용하면 경계에서 문맥이 끊기는 문제를 완화할 수 있습니다.

### 4.4 Embed and Store

각 chunk를 embedding vector로 변환하고 Vector DB에 저장합니다. 원문 URL, 제목, 작성 시점, 문서 분류 등 메타데이터도 함께 저장해야 출처 표시와 필터링이 가능합니다.

## 5. Retrieval

### Lexical Search

질의와 문서에 같은 단어가 등장하는지를 중심으로 검색합니다. 고유명사, 코드, 정확한 용어에 강하지만 표현이 달라지면 놓칠 수 있습니다.

### Dense Retrieval

Embedding 공간의 의미적 유사도를 사용합니다. 표현이 달라도 의미가 비슷한 문서를 찾을 수 있지만 숫자, 코드, 세부 조건에서 부정확할 수 있습니다.

### Hybrid Retrieval

Lexical과 Dense의 결과 또는 점수를 결합해 두 방식의 장점을 사용합니다.

### Reranking

초기 검색 후보를 더 정교한 모델로 다시 평가해 순서를 조정합니다. 단, 초기 후보에 없는 문서를 새로 복구할 수 없으므로 candidate recall이 먼저 확보되어야 합니다.

## 6. RAG의 장점

- 모델을 재학습하지 않고 문서 추가로 최신 정보를 반영할 수 있습니다.
- 사내 문서와 전문 지식을 활용할 수 있습니다.
- 답변과 함께 출처를 제시할 수 있습니다.
- 근거 문서를 제공해 hallucination 가능성을 줄일 수 있습니다.

## 7. RAG의 한계

- 정답 문서를 검색하지 못하면 생성 모델도 올바르게 답하기 어렵습니다.
- 잘못되거나 오래된 문서를 검색하면 답변 역시 잘못될 수 있습니다.
- 검색과 생성 단계가 추가되어 latency와 운영 비용이 증가합니다.
- 검색된 문서가 답을 포함하는지, 답변이 실제 근거를 따르는지 별도로 평가해야 합니다.

## 8. RAG 평가 관점

- **Retrieval Recall@k**: 정답 문서가 상위 k개 후보에 포함되는 비율
- **Precision**: 검색 결과 중 실제 관련 문서의 비율
- **Faithfulness**: 답변이 검색 근거에서 벗어나지 않는지
- **Answer Relevance**: 질문에 직접 답했는지
- **Citation Correctness**: 제시한 출처가 실제 답변을 뒷받침하는지

Top-k Recall은 최종 답변 정확도와 같지 않습니다. 검색, 생성, 인용은 각각 따로 평가해야 합니다.

## 9. CLIP

CLIP(Contrastive Language-Image Pretraining)은 이미지와 텍스트를 같은 embedding 공간에 정렬하는 멀티모달 모델입니다.

### 등장 배경

기존 이미지 분류 모델은 고정된 label이 있는 데이터에 의존했습니다. 새로운 class나 task에 적용하려면 데이터를 다시 수집하고 fine-tuning해야 했습니다.

CLIP은 대규모 image-text pair로 범용 표현을 학습해 zero-shot 분류가 가능하도록 설계됐습니다.

### 구조

```text
Image → Image Encoder ┐
                      ├→ Shared Embedding Space
Text  → Text Encoder  ┘
```

- **Image Encoder**: ResNet 또는 Vision Transformer
- **Text Encoder**: Transformer
- 두 encoder가 만든 vector의 cosine similarity를 비교합니다.

### Contrastive Learning

같은 pair의 이미지와 텍스트는 가까워지고, 다른 pair는 멀어지도록 학습합니다.

```text
Positive pair: image of a dog ↔ "a photo of a dog"
Negative pair: image of a dog ↔ "a photo of an airplane"
```

### Zero-shot Classification

분류할 class를 자연어 prompt로 만든 뒤 이미지 embedding과 가장 가까운 텍스트 embedding을 선택합니다.

```text
"a photo of a cat"
"a photo of a dog"
"a photo of a bird"
```

새로운 분류 head를 학습하지 않아도 텍스트로 class를 정의할 수 있습니다.

### 한계

- 학습 데이터의 편향이 embedding에 반영될 수 있습니다.
- 세밀한 개수 계산, 위치 관계와 복잡한 시각적 추론에는 약할 수 있습니다.
- Prompt 표현에 따라 zero-shot 성능이 달라집니다.
- 학습 데이터와 크게 다른 도메인에서는 성능이 낮아질 수 있습니다.

## Summary

- LLM은 범용성이 높지만 최신성, 전문성, 근거와 정확한 계산에 한계가 있습니다.
- RAG는 외부 문서를 검색해 LLM의 context를 보강하고 출처 기반 답변을 가능하게 합니다.
- RAG 품질은 데이터 처리, 후보 검색, reranking과 생성 단계를 각각 평가해야 합니다.
- CLIP은 이미지와 텍스트를 같은 공간에 정렬해 zero-shot 멀티모달 task를 수행합니다.
