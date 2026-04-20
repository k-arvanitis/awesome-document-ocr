# Awesome Document OCR [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) ![Last Updated](https://img.shields.io/github/last-commit/k-arvanitis/awesome-document-ocr)

> A curated list of modern OCR and document understanding systems — from classical baselines to VLM-powered pipelines, with a focus on RAG-ready structured output.

## Contents

- [The Mental Model](#-the-mental-model)
- [Reference Architecture](#-reference-architecture)
- [Suggested Use Cases](#-suggested-use-cases)
- [VLM-based OCR](#-vlm-based-ocr-next-gen--most-important)
- [Document Parsing & Structuring](#-document-parsing--structuring)
- [Table Extraction](#-table-extraction)
- [OCR Pipelines & Tooling](#️-ocr-pipelines--tooling)
- [Benchmarks & Datasets](#-benchmarks--datasets)
- [Classical OCR](#-classical-ocr-baselines)
- [Comparisons & Insights](#-comparisons--insights)
- [Final Take](#-final-take)

---

## 📈 Why This Repo Exists

The OCR ecosystem is fragmented. Dozens of tools solve overlapping problems with wildly different trade-offs — and the space is evolving rapidly as Vision-Language Models redefine what "OCR" even means.

This repo helps engineers **navigate, evaluate, and combine** these tools into production-grade document intelligence pipelines.

---

## 🧭 The Mental Model

OCR is no longer just text extraction. It is evolving into **Document Intelligence**:

- Classical OCR extracts characters. VLM OCR *understands* documents.
- Individual models matter less than the pipeline around them.
- The winning stack in 2025+: **VLM OCR → structured parsing → table extraction → RAG-ready output**.

---

## 🧱 Reference Architecture

```
PDF / Image
    │
    ▼
VLM-based OCR          ← understands layout, tables, formulas
    │
    ▼
Document Parser        ← converts to clean Markdown / JSON
    │
    ▼
Table Extractor        ← handles structured data separately
    │
    ▼
Structured Output      ← text chunks, table records, metadata
    │
    ▼
RAG System             ← vector store, retrieval, generation
```

---

## 💡 Suggested Use Cases

| Use Case | Recommended Stack |
|---|---|
| RAG pipeline ingestion | VLM OCR → Docling → chunked Markdown |
| Table-heavy PDF processing | Nougat / Donut + Table Transformer → JSON |
| Scientific paper parsing | Nougat → structured sections + formulas |
| Enterprise document processing | Unstructured + LayoutLMv3 → JSON schema |
| Multilingual document OCR | PaddleOCR + Marker → Markdown |

---

## 🔥 VLM-based OCR (Next-Gen — Most Important)

> These models treat documents as images and apply vision-language understanding. They handle complex layouts, formulas, tables, and handwriting far better than classical OCR.

---

### GLM-OCR

| Field | Detail |
|---|---|
| **GitHub** | [github.com/THUDM/GLM-4](https://github.com/THUDM/GLM-4) |
| **Model (HF)** | [huggingface.co/THUDM/glm-4v-9b](https://huggingface.co/THUDM/glm-4v-9b) |
| **Type** | VLM OCR |
| **License** | Open Weights |
| **Output** | Markdown, structured text |

**Description:** OCR capability built into the GLM-4V vision-language model from Tsinghua University. Handles complex Chinese and English document layouts natively.

**Strengths:** Multilingual, layout-aware, handles dense academic and business documents.

---

### Qianfan-OCR

| Field | Detail |
|---|---|
| **Link** | [qianfan.cloud.baidu.com](https://qianfan.cloud.baidu.com) |
| **Type** | VLM OCR / API |
| **License** | Closed / API |
| **Output** | Structured JSON, text |

**Description:** Baidu's enterprise-grade document OCR service backed by large vision-language models. Strong on scanned documents and complex form layouts.

**Strengths:** High accuracy on forms, tables, stamps, handwritten text at scale.

---

### LightOnOCR-2-1B

| Field | Detail |
|---|---|
| **Link** | [huggingface.co/lightonai/LightOn-OCR-2-1B](https://huggingface.co/lightonai/LightOn-OCR-2-1B) |
| **Type** | VLM OCR |
| **License** | Open Weights |
| **Output** | Markdown |

**Description:** Compact 1B-parameter VLM fine-tuned for high-accuracy OCR. Designed for efficient inference on CPU/GPU without sacrificing layout understanding.

**Strengths:** Small footprint, fast, strong on mixed-layout documents, self-hostable.

---

### dots.ocr

| Field | Detail |
|---|---|
| **Link** | [github.com/rednote-hilab/dots.ocr](https://github.com/rednote-hilab/dots.ocr) |
| **Type** | VLM OCR |
| **License** | Open Weights |
| **Output** | Markdown, structured text |

**Description:** Full vision-language model from RedNote that unifies layout detection, text recognition, and reading order into a single model. Not a wrapper — a purpose-built multilingual document parsing VLM.

**Strengths:** End-to-end document parsing (tables, formulas, complex layouts) in one model, multilingual, no separate OCR engine required.

---

### Donut

| Field | Detail |
|---|---|
| **Link** | [github.com/clovaai/donut](https://github.com/clovaai/donut) |
| **Type** | VLM OCR / Document Understanding |
| **License** | Open Source (MIT) |
| **Output** | JSON, structured text |

**Description:** Document Understanding Transformer from CLOVA AI. Processes document images end-to-end without a separate OCR step — directly outputs structured JSON.

**Strengths:** No OCR engine dependency, strong on forms and receipts, fine-tunable, JSON output ready for downstream tasks.

---

### Nougat

| Field | Detail |
|---|---|
| **Link** | [github.com/facebookresearch/nougat](https://github.com/facebookresearch/nougat) |
| **Type** | VLM OCR — Academic PDFs |
| **License** | Open Source (MIT) |
| **Output** | Markdown (with LaTeX formulas) |

**Description:** Meta's neural optical understanding of academic documents. Converts scientific PDFs — including math formulas and tables — into structured Markdown.

**Strengths:** Best-in-class for academic papers, handles LaTeX equations, preserves document structure faithfully.

---

### LayoutLMv3

| Field | Detail |
|---|---|
| **Link** | [github.com/microsoft/unilm/tree/master/layoutlmv3](https://github.com/microsoft/unilm/tree/master/layoutlmv3) |
| **Type** | Document Understanding Model (multimodal) |
| **License** | Open Source (CC BY-NC-SA 4.0) |
| **Output** | Structured tokens, classification outputs |

**Description:** Microsoft's multimodal model that jointly learns from text, layout, and visual features. State-of-the-art on document understanding benchmarks (DocVQA, FUNSD).

**Strengths:** Layout-aware embeddings, strong for key-value extraction and document classification, integrates with Hugging Face pipelines.

---

### Pix2Struct

| Field | Detail |
|---|---|
| **Link** | [github.com/google-research/pix2struct](https://github.com/google-research/pix2struct) |
| **Type** | VLM — Visually-Situated Language |
| **License** | Open Source (Apache 2.0) |
| **Output** | Structured text, Markdown |

**Description:** Google Research model pretrained to parse visually-situated language — charts, UI screenshots, scientific figures, and document images.

**Strengths:** Handles non-standard document elements (charts, diagrams, infographics), strong on structured visual reasoning tasks.

---

## 📄 Document Parsing & Structuring

> These tools take raw OCR output (or native PDFs) and convert them into clean, structured formats suitable for downstream use in RAG or data pipelines.

---

### Docling

| Field | Detail |
|---|---|
| **Link** | [github.com/DS4SD/docling](https://github.com/DS4SD/docling) |
| **Type** | Document Parser |
| **License** | Open Source (MIT) |
| **Output** | Markdown, JSON, DoclingDocument |

**Description:** IBM Research's document conversion library. Parses PDFs, DOCX, PPTX into structured representations with layout understanding, table detection, and reading order correction.

**Strengths:** Production-grade, integrates with LangChain and LlamaIndex, excellent table and figure handling, fast batch processing.

---

### Unstructured

| Field | Detail |
|---|---|
| **Link** | [github.com/Unstructured-IO/unstructured](https://github.com/Unstructured-IO/unstructured) |
| **Type** | Document Parser |
| **License** | Open Source (Apache 2.0) |
| **Output** | Structured JSON elements |

**Description:** De facto standard for document ingestion in RAG pipelines. Handles 20+ file types (PDF, HTML, DOCX, images) and segments content into typed elements (Title, Table, NarrativeText, etc.).

**Strengths:** Broadest file format support, RAG-ready chunking strategies, active ecosystem, hosted API available.

---

### Marker

| Field | Detail |
|---|---|
| **Link** | [github.com/VikParuchuri/marker](https://github.com/VikParuchuri/marker) |
| **Type** | Document Parser / PDF-to-Markdown |
| **License** | Open Source (GPL-3.0) |
| **Output** | Markdown |

**Description:** Converts PDFs to clean Markdown using a pipeline of specialized models for layout detection, table parsing, and formula recognition. Significantly better output quality than naive PDF text extraction.

**Strengths:** High-quality Markdown output, handles multi-column layouts, preserves tables and formulas, GPU-accelerated.

---

### MarkItDown

| Field | Detail |
|---|---|
| **Link** | [github.com/microsoft/markitdown](https://github.com/microsoft/markitdown) |
| **Type** | Document Converter |
| **License** | Open Source (MIT) |
| **Output** | Markdown |

**Description:** Microsoft utility for converting Office documents, PDFs, and other formats to Markdown. Lightweight, fast, and designed for LLM context injection.

**Strengths:** Minimal dependencies, handles DOCX/PPTX/XLSX natively, ideal for enterprise document ingestion.

---

## 📊 Table Extraction

> Tables are a major bottleneck in OCR pipelines. Standard text extraction destroys table structure. These tools preserve it.

Most OCR tools treat tables as flat text — losing row/column relationships entirely. Table extraction requires dedicated models that understand grid structure, merged cells, and borderless layouts.

---

### Table Transformer (TATR)

| Field | Detail |
|---|---|
| **Link** | [github.com/microsoft/table-transformer](https://github.com/microsoft/table-transformer) |
| **Type** | Table Detection & Structure Recognition |
| **License** | Open Source (MIT) |
| **Output** | JSON (cells, rows, columns), CSV |

**Description:** Microsoft Research model for table detection and structure recognition in document images. Two-stage pipeline: detect tables, then parse internal structure into rows and columns.

**Strengths:** State-of-the-art on PubTables-1M, handles borderless and complex tables, outputs structured cell data, integrates with document parsing pipelines.

---

## ⚙️ OCR Pipelines & Tooling

> Frameworks and utilities for building end-to-end document processing pipelines.

---

### Chandra

| Field | Detail |
|---|---|
| **Link** | [github.com/datalab-to/chandra](https://github.com/datalab-to/chandra) |
| **Type** | OCR Pipeline / Semantic Extraction |
| **License** | Open Source |
| **Output** | Structured semantic output |

**Description:** Pipeline framework that combines OCR with semantic understanding — extracts not just text but entities, relationships, and structured data from document images.

**Strengths:** Semantic-first extraction, modular pipeline design, reduces post-processing overhead.

---

### Byaldi

| Field | Detail |
|---|---|
| **Link** | [github.com/AnswerDotAI/byaldi](https://github.com/AnswerDotAI/byaldi) |
| **Type** | Multimodal Document Retrieval |
| **License** | Open Source (Apache 2.0) |
| **Output** | Retrieved document pages / images |

**Description:** RAG-native library built on ColPali — indexes document pages as visual embeddings and retrieves them without OCR. Treats the page image as the retrieval unit.

**Strengths:** No OCR step required for retrieval, handles complex layouts natively, strong performance on visually rich documents, pairs well with VLM readers.

---

## 📚 Benchmarks & Datasets

---

### olmOCR-bench

| Field | Detail |
|---|---|
| **GitHub** | [github.com/allenai/olmocr](https://github.com/allenai/olmocr) |
| **Model (HF)** | [huggingface.co/allenai/olmOCR-7B-0225-preview](https://huggingface.co/allenai/olmOCR-7B-0225-preview) |
| **Type** | Benchmark / Dataset + Model |
| **License** | Open Source (Apache 2.0) |
| **Output** | Evaluation metrics, Markdown |

**Description:** Allen AI's benchmark for evaluating OCR quality on academic and scientific documents. Includes olmOCR — a fine-tuned Qwen2-VL model and evaluation suite for measuring document conversion accuracy.

**Strengths:** Rigorous evaluation framework, PDF-to-Markdown quality scoring, covers diverse document types including multi-column and formula-heavy papers.

---

## 🧱 Classical OCR (Baselines)

> Established tools for traditional OCR workflows. Useful as baselines, fallbacks, or for simple single-language documents with clean layouts.

---

### Tesseract

| Field | Detail |
|---|---|
| **Link** | [github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract) |
| **Type** | Classical OCR Engine |
| **License** | Open Source (Apache 2.0) |
| **Output** | Plain text, hOCR, PDF |

**Description:** The most widely deployed open-source OCR engine. Backed by Google, supports 100+ languages, and serves as the baseline for virtually every OCR benchmark.

**Strengths:** Battle-tested, broad language support, integrates everywhere, fast on clean scans. Struggles with complex layouts, tables, and low-quality images.

---

### PaddleOCR

| Field | Detail |
|---|---|
| **Link** | [github.com/PaddlePaddle/PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) |
| **Type** | Classical OCR — Deep Learning |
| **License** | Open Source (Apache 2.0) |
| **Output** | Text with bounding boxes, Markdown (via PP-Structure) |

**Description:** Baidu's production OCR toolkit built on PaddlePaddle. Includes text detection, recognition, and document layout analysis (PP-Structure) for tables and complex pages.

**Strengths:** Strong multilingual support (80+ languages), fast inference, PP-Structure adds layout analysis, widely used in Asian-language document processing.

---

## 🧭 Comparisons & Insights

### Why VLM OCR is Different from Classical OCR

| Dimension | Classical OCR | VLM OCR |
|---|---|---|
| Input processing | Pixel → character | Image → semantic understanding |
| Layout handling | Poor on multi-column | Native layout understanding |
| Tables | Destroys structure | Can preserve rows/columns |
| Formulas | Fails entirely | Handles LaTeX-level complexity |
| Context awareness | None | Document-level reasoning |
| Fine-tuning | Limited | Full model fine-tuning possible |

### Why Pipelines Matter More Than Models

No single model handles every document type well. Production systems need:
- **Pre-processing** — page segmentation, deskewing, region routing
- **Specialized extractors** — tables routed to TATR, formulas to Nougat, body text elsewhere
- **Post-processing** — deduplication, reading order correction, schema validation

A mediocre pipeline using good routing beats the best single model.

### Why Table-Heavy PDFs Are Hard

- Table borders are inconsistent (or absent)
- Cell content spans multiple rows/columns
- Header rows are visually but not semantically distinct
- Most OCR models treat tables as prose — destroying relational structure
- Even strong VLMs hallucinate cell content in dense tables

**Practical advice:** Always use a dedicated table extraction step (TATR, Camelot, or Docling's table pipeline). Never rely on raw OCR output for tabular data.

### Where Current Systems Fail

- **Handwritten text at scale** — VLMs improve this but still unreliable in production
- **Low-resolution scans** — preprocessing (denoising, upscaling) is still required
- **Multi-page table continuation** — most models treat pages independently
- **Right-to-left and mixed-direction text** — limited support outside specialized models
- **Speed vs. accuracy** — VLM OCR is 10–100× slower than Tesseract; batching and caching are essential

---

## 🧠 Final Take

OCR in 2025 is not a solved problem — it's a rapidly evolving field being redefined by vision-language models.

The shift is from **text extraction** to **document intelligence**: understanding layout, preserving structure, and producing outputs that are immediately useful in downstream AI systems.

Three principles define the winning approach:

1. **Use VLM OCR as the foundation.** Models like Nougat, Donut, and LightOnOCR-2-1B outperform classical engines on any non-trivial document — not because they're newer, but because they understand documents rather than just reading pixels.

2. **Build pipelines, not point solutions.** No model handles everything. Route table regions to TATR, formulas to Nougat, and use Docling or Unstructured to normalize the output. The pipeline is the product.

3. **Target RAG-ready output from the start.** The end goal is structured, chunked, retrievable content — not raw text. Design your extraction layer to output Markdown or JSON that flows directly into a vector store without post-processing hacks.

The field is moving fast. The tools listed here represent the current frontier — and some will be obsolete within months. The architecture, however, will not.

---

## 🚀 GitHub Positioning

This repo occupies a specific niche: **VLM-native OCR for RAG engineers** — a space that is underserved by generic awesome lists.

**Why it attracts stars:**
- Covers the intersection of two fast-growing areas: document AI and RAG pipelines
- Structured format with per-tool trade-off analysis saves hours of evaluation time
- Reference architecture gives immediate mental model for system design
- Updated as the space moves — not a static link dump

**Who it's for:**
- AI engineers building RAG or document intelligence systems
- ML engineers evaluating OCR stacks for production use
- Researchers benchmarking document understanding models
- Builders looking for the fastest path from PDF to structured output

---

> ✨ Short descriptions are copy-paste ready for direct use in pipelines or technical documentation.

---

*Maintained by [Konstantinos Arvanitis](https://github.com/k-arvanitis) — AI Engineer focused on RAG pipelines and document intelligence systems.*

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0)
