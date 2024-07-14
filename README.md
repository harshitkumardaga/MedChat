# MedChat
A locally deployable healthcare chatbot that extracts and provides key information from HTML and PDF documents using BioMistral LLM and PubMedBert embeddings. It employs Qdrant for vector storage and retrieval, enabling accurate and detailed responses to user queries. Efficient and designed for medical domains.
# Healthcare Informatory AI Chatbot

## Objective

The goal of this project is to create a locally deployable chatbot for a healthcare organization. This chatbot will extract key information from healthcare documents in HTML and PDF formats and provide accurate responses to user queries based on the extracted information. The project leverages a Retrieval-Augmented Generation (RAG) approach using an open-source stack.

## Model Selection

The chatbot uses [BioMistral](https://huggingface.co/BioMistral/BioMistral-7B), an open-source LLM fine-tuned for medical domains. A quantized model version is employed to ensure efficient local deployment on CPU. For embeddings, [PubMedBert](https://huggingface.co/NeuML/pubmedbert-base-embeddings) is utilized, which is fine-tuned with sentence transformers, providing 768-dimensional dense vectors optimized for medical tasks.

## Data Extraction Process

The data folder contains PDF files related to knee replacement surgery, which are used for vector creation and storage in Qdrant. The data extraction process involves the following steps:

1. **Loading Documents**: Use `UnstructuredFileLoader` and `DirectoryLoader` to extract text from PDF documents.
2. **Text Splitting**: Split the documents into manageable chunks using `RecursiveCharacterTextSplitter`.
3. **Vector Creation**: Generate embeddings from the text chunks using `PubMedBert` and store these vectors in Qdrant.

## Architecture

1. **Vector Embedding**: Create vector embeddings of all documents using the `ingest.py` script.
2. **Query Processing**: Retrieve the most similar vectors from Qdrant when a user inputs a query. These vectors are provided as context to the LLM to generate detailed and accurate responses.
3. **Conversational Memory**: Use ConversationalRetrievalChain to pass previous queries and responses to the LLM, enhancing context and improving response quality.

## How to Run

### Step 1: Setup Virtual Environment

Create a new Python virtual environment.

```sh
python3 -m venv venv
source venv/bin/activate
```

### Step 2: Install Requirements

```sh
pip install -r requirements.txt
```

### Step 3: Setup Qdrant

Install Docker and run Qdrant.

```sh
docker pull qdrant/qdrant
docker run -p 6333:6333 -p 6334:6334 -v $(pwd)/qdrant_storage:/qdrant/storage:z qdrant/qdrant
```

### Step 4: Download the Quantized Version of BioMistral

Download the quantized version of BioMistral from [here](https://huggingface.co/MaziyarPanahi/BioMistral-7B-GGUF) to the project directory.

### Step 5: Ingest Data to Qdrant Collection

```sh
python3 ingest.py
```

### Step 6: Start the FastAPI Server

```sh
uvicorn main:app --reload
```
