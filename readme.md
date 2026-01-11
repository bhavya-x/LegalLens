<div align="center">

# ⚖️ LegalLens

**AI-powered legal document analysis — upload a contract, get summaries, and ask questions in your language.**

[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-API-000000?style=flat-square&logo=flask)](https://flask.palletsprojects.com/)
[![Pinecone](https://img.shields.io/badge/Pinecone-Vector%20DB-1B1F23?style=flat-square)](https://www.pinecone.io/)
[![Haystack](https://img.shields.io/badge/Haystack-NLP-FF6F00?style=flat-square)](https://haystack.deepset.ai/)
[![MongoDB](https://img.shields.io/badge/MongoDB-Chat%20History-47A248?style=flat-square&logo=mongodb&logoColor=white)](https://www.mongodb.com/)

</div>

---

## Overview

LegalLens makes legal documents understandable. Upload a PDF, scanned image, or text file and the system extracts text, translates if needed, summarises long passages, and indexes the content for retrieval. You can then chat with the document in your preferred language and get structured, source-backed answers.

## Pipeline

```
Upload → OCR / pdfplumber → Language detect & translate
       → BigBird-Pegasus summarisation
       → Embeddings → Pinecone (retrieval)
       → User question → m2m100 translate
       → Haystack EmbeddingRetriever → Pinecone
       → Seq2SeqGenerator (BART LFQA) → Answer
       → MongoDB chat history → translate back to user language
```

## Features

| Capability | Implementation |
|---|---|
| Document ingestion | PDFs, scanned images, plain text |
| OCR | Tesseract.js for scanned legal docs |
| Translation | Facebook `m2m100` (multi-direction) |
| Summarisation | BigBird-Pegasus with chunking |
| Vector search | Pinecone (`us-west4-gcp-free`, cosine, 768d) |
| QA generation | Haystack `Seq2SeqGenerator` + `vblagoje/bart_lfqa` |
| Context retention | MongoDB conversation history |
| UI | React/Vue web frontend served via Flask |

## Tech Stack

- **Backend:** Flask + Haystack + HuggingFace Transformers
- **NLP models:** BERT (length tokeniser), BART LFQA (generation), m2m100 (translation), BigBird-Pegasus (summarisation), `flax-sentence-embeddings/all_datasets_v3_mpnet-base` (embeddings)
- **Storage:** Pinecone (vectors), MongoDB (chat history)
- **Frontend:** Web app under `webapp/`

## Getting Started

```bash
git clone https://github.com/bhavya-x/LegalLens.git
cd LegalLens
pip install -r requirements.txt   # if present
# Configure Pinecone + MongoDB credentials in includes/dependencies.py
python server.py
```

## Repository Layout

```
LegalLens/
├── server.py              # Flask app + Haystack pipeline
├── includes/              # Shared dependencies and helpers
├── data/                  # Sample legal datasets
├── models/                # Model artefacts / configs
├── webapp/                # Frontend
├── plots/                 # Architecture diagrams
└── readme.md              # Original project notes
```

## Screenshots

| Landing | Summary | Chat |
|---|---|---|
| ![Landing](main.png) | ![Summary](image2.png) | ![Chat](image3.png) |
