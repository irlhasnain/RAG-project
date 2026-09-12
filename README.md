# Mini NotebookLM — Local RAG Project

This is a **local Retrieval-Augmented Generation (RAG) system** that runs on Google Colab. It's inspired by Google's *NotebookLM* — the user uploads their documents, the system reads and understands them, and then generates **summaries**, **FAQs**, and **grounded Q&A** based on those documents — all using open-source models, without any paid/external API.

Notebook: `Hasnain_Khan_RAG_Project.ipynb`

## Features

- **Multi-format document upload** — Supports PDF, DOCX, and TXT files (max 5 files, ≤ 15 MB each)
- **Text extraction** — Uses PyPDF (PDF), python-docx (DOCX), and direct decoding (TXT) to extract text
- **Automatic title detection** — Guesses each document's title from PDF layout (font size/position), DOCX paragraph styles, or the first line of TXT files
- **Text cleaning & chunking** — Cleans extra whitespace/newlines and splits text into ~1400-character overlapping chunks
- **Semantic search (embeddings)** — Embeds chunks using `sentence-transformers/all-MiniLM-L6-v2` and builds a **FAISS** index for retrieval
- **Summarization** — Summarizes long documents chunk-by-chunk into fact-rich bullet points
- **Auto FAQ generation** — Generates 12 specific/detailed questions per document, filtering out generic ones
- **Grounded Question Answering** — Answers user questions strictly based on the uploaded documents' context; replies "I don't know from the provided documents." if the answer isn't found
- **Gradio Web UI** — An interactive interface where users can select a document scope and view Q&A, summaries, and FAQs

## Pipeline (How It Works)

1. **Upload** — User uploads files in Colab (`google.colab.files.upload`)
2. **Extract & Clean** — Text is extracted per file type and cleaned/normalized
3. **Title Detection** — A readable title is derived for each document
4. **Chunking** — Text is split into overlapping chunks for better retrieval
5. **Embedding + Indexing** — Chunks are converted to vector embeddings and stored in a FAISS index
6. **Summarization** — Qwen2.5-0.5B-Instruct generates a summary for each document
7. **FAQ Generation** — Specific FAQs are generated per document based on its summary
8. **Search + Answer** — The user's query is embedded, relevant chunks are retrieved via FAISS, and the LLM generates an answer grounded in that context
9. **UI** — A Gradio Blocks interface ties everything together (Ask Questions, Summary, FAQs) in one dashboard

## Tech Stack / Models Used

| Component | Tool/Model |
|---|---|
| Embeddings | `sentence-transformers/all-MiniLM-L6-v2` |
| Text Generation (Summary + Q&A) | `Qwen/Qwen2.5-0.5B-Instruct` |
| Vector Search | FAISS (`faiss-cpu`) |
| PDF Parsing | `pypdf`, `pymupdf` (fitz) |
| DOCX Parsing | `python-docx` |
| Sentence Tokenization | `nltk` |
| UI | `gradio` |
| Platform | Google Colab |

## Requirements

```
transformers
sentence-transformers
faiss-cpu
pypdf
python-docx
nltk
gradio
pymupdf
torch
```

## Usage

1. Open the notebook in Google Colab (a badge is provided at the top of the notebook)
2. Enable a GPU runtime (Runtime → Change runtime type → GPU) for better performance
3. Run all cells in sequence
4. When prompted, upload your PDF/DOCX/TXT files (max 5 files, 15 MB each)
5. The notebook will automatically extract text and generate summaries and FAQs
6. After running the last cell, the Gradio interface will open — where you can select a document scope and ask questions

## Limitations

- The generation model (Qwen2.5-0.5B) is small, so answers may sometimes be brief/basic
- PDF text extraction won't be accurate for scanned/image-based PDFs (no OCR support)
- Answers are limited strictly to the context of the uploaded documents — no external/outside knowledge is used

## Author

Hasnain Khan