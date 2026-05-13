# Workflow Notes

## Step1: setting up environment for testing


![testing_environment](image.png)

## Step2: Access Class Workflow

**Workflow:** Document Upload RAG Chatbot with Cohere Reranking - May 13th.json

### Testing nodes

**First part: RAG Document Embedding & Storage Workflow**

**Node1: upload document form**

![doc_upload](image-25.png)

**Nodes: store in pincone/ document loader/ text splitter/OpenAI embeddings** 

Settings:

* Embedding settings: text-embedding-3-small / Dimension: 1536
* Pinecone credentials/ Pinecone index (from the field created)/ Embedding batch size: 200
* Document loader: type of data: JSON

![store_pincone](image-24.png)

![store_pinecone_document_loader_input_logs](image-8.png)

![store_pinecone_document_loader_output_logs](image-9.png)

![document_loader](image-26.png)

![OpenAI_embeddings](image-27.png)

![text_splitter](image-28.png)


**First part successfully executed:**

![load_data_flow](image-10.png)
![load_data_flow](image-10.png)


**Node Overview**

| Node | Role |
|---|---|
| **Upload Documents Form** | Entry point — user submits a PDF file |
| **Document Loader** | Reads and parses the raw file into readable text |
| **Text Splitter** | Chunks the text into smaller pieces for better embedding quality |
| **OpenAI Embeddings** | Converts each chunk into a vector using `text-embedding-3-small` |
| **Store in Pinecone** | Persists the vectors + metadata to the Pinecone vector database |


**Second part: RAG Query & Reranking Pipeline**

Settings:

* OpenAI Chat Model:

![OpenAI_chat_model](image-11.png)

* Query embeddings: text-embedding-3-small / Dimension: 1536
* Cohere credentials. Model rerank-v3.5. N=3

### Testing nodes

**Node1: Chat interface**

![Chat](image-12.png)

* Pinecone Index: trustworthy-ai-rag (only one created in a former lab)

![Retrieve_from_pinecone](image-20.png)

![Cohere_reranker](image-21.png)

![RAG_Agent](image-29.png)

![OpenAI_Chat_Model](image-23.png)

![Query_embeddings](image-22.png)


Second part successfully tested:

![RAG_query](image-30.png)


**Node Overview**

| Node | Role |
|---|---|
| **Chat Interface** | Entry point — receives the user's question |
| **RAG Agent** | Orchestrates the full retrieval and response pipeline |
| **OpenAI Chat Model** | Generates the final natural language answer |
| **Query Embeddings** | Converts the user's question into a vector for similarity search |
| **Retrieve from Pinecone** | Fetches the most semantically similar document chunks |
| **Cohere Reranker** | Re-scores and reorders retrieved chunks for better relevance before answering |


