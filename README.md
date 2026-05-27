# rag-pdf-chatbot

A question answering system that retrieves accurate answers from PDF documents using Retrieval-Augmented Generation (RAG).

## How It Works

1. **Load** — reads and parses the PDF
2. **Chunk** — splits text into small overlapping pieces
3. **Embed & Store** — converts chunks into vectors and stores them in FAISS
4. **Retrieve** — finds the top 20 most similar chunks for a given question
5. **Rerank** — CrossEncoder scores each chunk against the question and picks the top 4 most relevant
6. **Answer** — Llama 3 generates a grounded answer using only the retrieved context

## Stack

| Tool | Purpose |
|------|---------|
| LangChain | Pipeline orchestration |
| FAISS | Vector storage and similarity search |
| all-MiniLM-L6-v2 | Text embeddings |
| ms-marco-MiniLM-L-6-v2 | CrossEncoder reranking |
| Llama 3.1 8B (Groq) | Answer generation |
| PyPDF | PDF parsing |

## Setup

1. Clone the repo
```bash
git clone https://github.com/Busrara/pdf-rag-chatbot.git
cd pdf-rag-chatbot
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Get a free API key at [console.groq.com](https://console.groq.com)

4. Open `rag_pdf_qa.ipynb` and add your key:
```python
os.environ["GROQ_API_KEY"] = "your_key_here"
```

## Usage

By default the notebook loads the *Attention Is All You Need* paper. To use your own PDF:

```python
PDF_PATH = "/content/your_file.pdf"
```

Then ask any question:
```python
question = "What problem does this paper solve?"
result = qa_chain.invoke(question)
print(result)
```

## Example Output

```
QUESTION: What is multi-head attention?
ANSWER: Multi-head attention allows the model to jointly attend to information
from different representation subspaces at different positions, by running
multiple attention layers in parallel and concatenating their outputs.
```
