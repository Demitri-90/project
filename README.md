# Document Question Answering System using RAG

This project is a small local assistant for reading documents and answering questions about them. It follows a simple retrieval-augmented workflow: first it looks for the most relevant parts of the text, then it uses that context to build an answer.

The setup is meant to stay lightweight and practical. It runs fully on a local machine, uses a compact TF-IDF index, and does not depend on large model downloads.

## What the project does

This project can:

- read documents from the folder data/open_ragbench/pdf/arxiv/corpus
- break long documents into smaller overlapping chunks
- store those chunks in a local search index
- find the passages that best match a question
- produce a grounded answer with source references

## Why I built it

The goal was to create a straightforward example of a document-based question answering system that is easy to understand and easy to run. It helps show how retrieval and answer generation can work together without needing a complicated setup.

## How it works

1. Load documents from the dataset folder.
2. Split the text into smaller overlapping chunks.
3. Build a local TF-IDF index for retrieval.
4. Match the user question with the most relevant chunks.
5. Generate a final answer using the retrieved context.

## Repository layout

```text
Document Question Answering System (RAG)/
|-- data/
|   `-- open_ragbench/
|       `-- pdf/
|           `-- arxiv/
|               `-- corpus/
|-- src/
|   |-- ingest.py
|   |-- retrieve.py
|   |-- generate.py
|   `-- app.py
|-- vector_store/
|-- requirements.txt
`-- README.md
```

## Getting started

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

The project relies on standard Python tools for handling text and JSON files. The only extra package needed for PDF support is pypdf.

## Default data source

The default dataset location used by the app is:

```text
data/open_ragbench/pdf/arxiv/corpus
```

The application will use this folder automatically when it is available.

## Running the app

### 1. Build the index and ask one question

```bash
python src/app.py --rebuild --query "What is the main idea of the document?"
```

### 2. Ask another question using the saved index

```bash
python src/app.py --query "What are the key findings?"
```

### 3. Run the app interactively

```bash
python src/app.py --interactive
```

### 4. See example questions from the dataset

```bash
python src/app.py --sample-queries 5
```

## Example interaction

Example input:

```text
What is the main idea of the document?
```

Example output:

```text
Based on the retrieved documents, ...

Sources:
[1] document_name - source_file
```

## Code overview

- ingest.py: loads documents and splits them into chunks
- retrieve.py: creates the local TF-IDF search index
- generate.py: turns retrieved context into a readable answer
- app.py: connects everything together and runs the CLI

## Helpful command options

```bash
python src/app.py --help
```

A few helpful flags are:

- --rebuild: rebuild the index
- --top-k 5: retrieve more chunks
- --chunk-size 220: use larger chunks
- --overlap 50: add more overlap between chunks
- --max-files 0: index all available files
- --documents path/to/folder: use your own folder or file


