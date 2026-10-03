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

## Original Notes & Practice Files

요약 과정에서 내용이 사라지지 않도록 기존에 작성한 원본 자료를 `raw-materials/`에 그대로 보존했습니다. 정리된 개념 문서와 함께 원본 설명, 코드, 실행 결과와 첨부 자료도 확인할 수 있습니다.

| Date | Original Material | Format |
|---|---|---|
| 06.23 | [AI·딥러닝 기초 실습](./raw-materials/2026_06_23_구정현_ipynb의_사본.ipynb) | Jupyter Notebook |
| 06.24 | [RNN 학습 자료](./raw-materials/2026-06-24_구정현.pages) | Pages |
| 06.25 | [텍스트 전처리 실습](./raw-materials/2026-06-25_구정현.ipynb) · [Notion 원문](./raw-materials/6월%2025일%2038a0b3c11a1e8012b87ef0c13ea44dff.md) | Notebook · Markdown |
| 06.26 | [PyTorch 실습](./raw-materials/2026-06-26_구정현.ipynb) · [Notion 원문](./raw-materials/6월%2026일%2038b0b3c11a1e80f8b770cf78d4c4517b.md) | Notebook · Markdown |
| 06.29 | [NLP Task Notion 원문](./raw-materials/notion-export/2026-06-29/6월29일(NLP)%203d10b3c11a1e80058f58ed24dc26016f.md) · [첨부 이미지](./raw-materials/notion-export/2026-06-29/6월29일(NLP)) | Markdown · Images |
| 06.30 | [모델 학습·경사하강법 이론](./raw-materials/_26.06.30_이론.pdf) | PDF |
| 07.01 | [Seq2Seq·Attention 실습](./raw-materials/2026_07_01_구정현_ipynb의_사본.ipynb) | Jupyter Notebook |
| 07.02 | [Transformer 실습](./raw-materials/2026_07_02_구정현_ipynb의_사본.ipynb) | Jupyter Notebook |
| 07.03 | [BERT Notion 원문](./raw-materials/notion-export/2026-07-03/7월3일(BERT)%203d10b3c11a1e8082b4afef8d36663a8b.md) · [첨부 이미지](./raw-materials/notion-export/2026-07-03/7월3일(BERT)) | Markdown · Images |
| 07.07 | [RAG 실습](./raw-materials/2026_07_07_구정현의_사본.ipynb) · [Notion 원문](./raw-materials/7월%207일%203960b3c11a1e807e853df962eaff266d.md) | Notebook · Markdown |

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
├── notes
│   ├── 01-ai-deep-learning-fundamentals.md
│   ├── 02-nlp-pytorch-fundamentals.md
│   ├── 03-sequence-models-and-transformers.md
│   └── 04-rag-and-multimodal-ai.md
└── raw-materials
    ├── notion-export
    │   ├── 2026-06-29
    │   └── 2026-07-03
    ├── *.ipynb
    ├── *.md
    ├── *.pages
    └── *.pdf
```

## Note Format

각 문서는 다음 기준으로 정리했습니다.

1. 개념이 필요한 이유
2. 핵심 구조와 동작 과정
3. 장점과 한계
4. 다음 모델 또는 실무 응용과의 연결
