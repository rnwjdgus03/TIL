# AI · NLP Learning Notes

Notion 개인 페이지에 기록한 AI·NLP 학습 내용을 주제별로 다시 정리한 저장소입니다.

날짜별 메모를 그대로 나열하지 않고, 앞의 개념이 뒤의 모델과 응용으로 이어지도록 학습 흐름에 따라 구성했습니다.

## Learning Path

```text
AI와 딥러닝 기초
→ 모델 학습과 경사하강법
→ RNN과 시퀀스 모델
→ 텍스트 전처리와 NLP Task
→ Seq2Seq와 Attention
→ Transformer
→ BERT와 GPT
→ LLM의 한계와 RAG
→ CLIP과 Multimodal Learning
```

## Contents

| Part | Topic | Key Concepts |
|---:|---|---|
| 01 | [AI & Deep Learning Fundamentals](./notes/01-ai-deep-learning-fundamentals.md) | AI, Deep Learning, Forward/Backward Pass, Gradient Descent, RNN |
| 02 | [NLP & PyTorch Fundamentals](./notes/02-nlp-pytorch-fundamentals.md) | Text Preprocessing, Tokenization, PyTorch, NLP Task |
| 03 | [Sequence Models & Transformers](./notes/03-sequence-models-and-transformers.md) | Seq2Seq, Attention, Transformer, BERT, GPT |
| 04 | [RAG & Multimodal AI](./notes/04-rag-and-multimodal-ai.md) | LLM Limitations, RAG, Embedding, Vector Search, CLIP |

## Study Timeline

| Date | Original Notion Topic | Organized In |
|---|---|---|
| 06.23 | 인공지능과 딥러닝의 개요 | Part 01 |
| 06.24 | RNN | Part 01 |
| 06.25 | 텍스트 전처리 | Part 02 |
| 06.26 | PyTorch | Part 02 |
| 06.29 | NLP Task | Part 02 |
| 06.30 | 가중치 업데이트와 경사하강법 | Part 01 |
| 07.01 | Seq2Seq와 Attention | Part 03 |
| 07.02 | Transformer | Part 03 |
| 07.03 | BERT | Part 03 |
| 07.06 | GPT | Part 03 |
| 07.07 | LLM의 한계와 RAG | Part 04 |
| 07.08 | CLIP | Part 04 |

## Topics at a Glance

### AI / Deep Learning

`Artificial Intelligence` · `Machine Learning` · `Deep Learning` · `Loss` · `Backpropagation` · `Gradient Descent` · `RNN`

### NLP / Language Models

`Text Preprocessing` · `Tokenization` · `PyTorch` · `Seq2Seq` · `Attention` · `Transformer` · `BERT` · `GPT`

### Generative AI / Retrieval

`LLM` · `Knowledge Cutoff` · `Hallucination` · `Embedding` · `Vector Search` · `RAG`

### Multimodal

`CLIP` · `Contrastive Learning` · `Zero-shot Classification` · `Text Encoder` · `Image Encoder`

## Repository Structure

```text
.
├── README.md
└── notes
    ├── 01-ai-deep-learning-fundamentals.md
    ├── 02-nlp-pytorch-fundamentals.md
    ├── 03-sequence-models-and-transformers.md
    └── 04-rag-and-multimodal-ai.md
```

## Note Format

각 문서는 다음 기준으로 정리했습니다.

1. 개념이 필요한 이유
2. 핵심 구조와 동작 과정
3. 장점과 한계
4. 다음 모델 또는 실무 응용과의 연결
