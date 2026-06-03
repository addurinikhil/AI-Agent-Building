# Conversational RAG Assistant with Tool Calling

## Open in Google Colab

[Open Notebook in Colab](https://colab.research.google.com/github/Prabhavathi-k/conversational-rag-assistant/blob/main/Conversational_Rag_Assistant.ipynb)


## Project Overview

This project implements a Conversational Retrieval-Augmented Generation (RAG) Assistant capable of answering questions from a knowledge base while maintaining conversation context and utilizing external tools when required.

The assistant uses a PDF document as its knowledge source, converts the content into vector embeddings, stores them in a Chroma vector database, and retrieves relevant information to answer user queries. Additionally, it supports conversational memory, history-aware question rewriting, and external tool execution.

### Key Features

* Retrieval-Augmented Generation (RAG)
* Conversational Memory
* History-Aware Question Rewriting
* Vector Search using ChromaDB
* Sentence Embeddings using HuggingFace
* External Tool Calling
* Routing Logic for RAG and Tool Responses
* Interactive Chat Loop

---

## Memory Implementation

The assistant maintains conversation history using an in-memory Python list called `chat_history`.

```python
chat_history = []
```

Each user message and assistant response is stored as:

```python
{
    "role": "user",
    "content": "What is RAG?"
}
```

The stored messages are formatted into a conversation transcript using the `get_chat_history()` function.

### Purpose

Conversation memory enables the assistant to understand follow-up questions and maintain context across multiple interactions.

### Example

User:

```text
What is RAG?
```

Follow-up:

```text
Explain it more.
```

The assistant uses the stored history to understand that "it" refers to "RAG".

---

## History-Aware Retrieval

Before retrieving information from the vector database, the assistant rewrites follow-up questions into standalone questions.

### Example

Original Question:

```text
Explain it more.
```

Rewritten Question:

```text
Explain Retrieval Augmented Generation in more detail.
```

This improves retrieval quality because vector databases perform better when provided with complete, context-independent queries.

The rewriting process is performed using a Gemini language model.

---

## Tool Explanation

The project includes one external tool:

### get_current_time()

Returns the current system date and time.

```python
def get_current_time():
    return datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

### Tool JSON Schema

```python
{
    "name": "get_current_time",
    "description": "Returns current date and time",
    "parameters": {
        "type": "object",
        "properties": {},
        "required": []
    }
}
```

### Example

User:

```text
What is the current time?
```

Assistant:

```text
Current Time: 2026-06-02 10:30:45
```

---

## Routing Logic

The assistant determines whether a query should be answered using retrieved document context or an external tool.

### Workflow

1. User submits a question.
2. Question is rewritten into a standalone question.
3. Relevant document chunks are retrieved.
4. Retrieved context is evaluated.
5. If relevant context exists:

   * Answer is generated using RAG.
6. Otherwise:

   * The external tool is executed.
7. Final response is returned to the user.

### Example Routes

#### Retrieval-Based Answer

```text
User: What is RAG?
```

Answer generated from PDF knowledge base.

#### Tool-Based Answer

```text
User: What is the current time?
```

Answer generated using the time tool.

---

## Technologies Used

* Python
* LangChain
* ChromaDB
* HuggingFace Embeddings
* Sentence Transformers
* Google Gemini
* PyPDFLoader
* Recursive Character Text Splitter

---

## Project Structure

```text
conversational-rag-assistant/
│
├── Conversational_RAG_Assistant.ipynb
├── tools.py
├── README.md
├── requirements.txt
└── screenshots/
    ├── retrieval_answer.png
    ├── followup_question.png
    └── tool_call.png
```

---

## Steps to Run the Project

### 1. Clone the Repository

```bash
git clone <repository-url>
cd conversational-rag-assistant
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

Or install manually:

```bash
pip install langchain
pip install langchain-community
pip install chromadb
pip install sentence-transformers
pip install pypdf
pip install langchain-google-genai
```

### 3. Add the Knowledge Base

Place the PDF file:

```text
rag_notes.pdf
```

inside the project directory.

### 4. Configure Gemini API Key

Replace the API key in the notebook with your own Google Gemini API key.

```python
google_api_key="YOUR_GEMINI_API_KEY"
```

### 5. Run the Notebook

Open and execute:

```text
Conversational_RAG_Assistant.ipynb
```

Run all cells sequentially.

### 6. Start Chatting

Execute the final chat loop cell.

Example:

```text
You: What is RAG?

Assistant: RAG stands for Retrieval-Augmented Generation...
```

To exit:

```text
exit
```

---

## Demonstration Scenarios

### Scenario 1: Retrieval-Based Answer

```text
User: What is RAG?
```

The assistant retrieves information from the PDF knowledge base and generates a response.

### Scenario 2: Follow-Up Question

```text
User: Explain it more.
```

The assistant uses conversation memory and rewrites the query into a standalone question before retrieval.

### Scenario 3: Tool Calling

```text
User: What is the current time?
```

The assistant invokes the `get_current_time()` tool and returns the result.

---

## Future Improvements

* Persistent chat history using SQLite or Redis
* Multiple external tools
* Source citations for retrieved chunks
* Agent-based tool selection
* Multi-document support
* Enhanced relevance scoring
