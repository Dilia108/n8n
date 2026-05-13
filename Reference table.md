Reference table for Workflow2


# n8n Node Reference Table

| Node | Parameters | Settings | What It Does | JSON Input | JSON Output | Key Transformations |
|------|------------|----------|--------------|------------|-------------|---------------------|
| Upload Documents Form | File upload field | Form submission settings | Lets the user upload a PDF document | Uploaded file | File binary + metadata | File input into workflow |
| Document Loader | Source file, type of data | Type of data: JSON | Reads and parses the uploaded file into text | Uploaded document | Parsed text chunks | File to readable content |
| Text Splitter | Chunk size, overlap | Split strategy | Breaks the document into smaller chunks for embedding | Parsed text | Text chunks | Large text to smaller segments |
| OpenAI Embeddings | Model, dimensions | `text-embedding-3-small`, dimension `1536` | Converts each chunk into vectors | Text chunks | Embedding vectors | Text to vector representation |
| Store in Pinecone | Index, credentials, batch size | Pinecone index, embedding batch size `200` | Stores vectors and metadata in Pinecone | Embeddings + metadata | Stored vector records | Persists knowledge base entries |
| Chat Interface | Chat input | Chat UI settings | Collects the user question | User message | Chat prompt data | User query capture |
| RAG Agent | Agent instructions, tools | Workflow orchestration settings | Coordinates retrieval and answer generation | Question + retrieved context | Final response draft | Retrieval-augmented orchestration |
| OpenAI Chat Model | Model, system prompt, messages | OpenAI chat model `gpt-5.4-mini`| Generates the final answer | Prompt + context | Natural language answer | Contextual response generation |
| Query Embeddings | Model, dimensions | `text-embedding-3-small`, dimension `1536` | Converts the user query into a vector | User question | Query embedding vector | Query to vector representation |
| Retrieve from Pinecone | Index, top K | Pinecone index `trustworthy-ai-rag` | Fetches the most relevant chunks | Query vector | Retrieved document matches | Similarity search |
| Cohere Reranker | Model, N | `rerank-v3.5`, `N = 3` | Reorders retrieved chunks by relevance | Retrieved matches | Reranked top results | Relevance ranking |
