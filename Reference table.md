Reference table for Workflow2


# n8n Node Reference Table

| Node | Parameters | Settings | What It Does | JSON Input | JSON Output | Key Transformations |
|------|------------|----------|--------------|------------|-------------|---------------------|
| Upload Documents Form | File field `trustworthy_ai`, submit mode | Test form enabled, uploaded PDF sample | Accepts the PDF upload and starts the workflow | `{ trustworthy_ai, submittedAt, formMode }` plus binary file data | Form submission JSON with file metadata | File upload to workflow entry payload |
| Document Loader | Input source, load mode | Type of data: `JSON` | Tries to load the uploaded file content from the form payload | Form submission JSON from the upload node | `{ response: [{ pageContent, metadata }] }` | Form payload to document chunk objects |
| Text Splitter | Chunk size, chunk overlap | Chunk size `1000`, overlap `0` | Splits the loaded text into smaller chunks for downstream embedding | Document text / loader output | `{ response: [string, string, ...] }` | Large text to chunked strings |
| OpenAI Embeddings | Model, dimensions | `text-embedding-3-small`, `1536` dimensions | Converts each document chunk into a vector | `{ documents: [string, ...] }` | `{ response: [[float, ...1536]] }` | Text chunks to embedding vectors |
| Store in Pinecone | Pinecone index, credentials, embedding batch size | Selected Pinecone index, batch size `200` | Stores embeddings and metadata in Pinecone | Binary file or processed chunks plus metadata | Stored vector records in Pinecone | Embeddings persisted to vector DB |
| Chat Interface | Chat input field | Chat UI enabled | Captures the user question for retrieval | `{ action, sessionId, chatInput }` | Chat request payload | User question to agent input |
| Query Embeddings | Query field, model, dimensions | `text-embedding-3-small`, `1536` dimensions | Embeds the user question for similarity search | `{ query }` | `{ response: [float, ...1536] }` | Question text to query vector |
| Retrieve from Pinecone | Index, top K | Pinecone index `trustworthy-ai-rag` | Retrieves the most relevant stored chunks | Query embedding vector | Matching document chunks | Similarity search over stored vectors |
| Cohere Reranker | Model, N | `rerank-v3.5`, `N = 3` | Reorders retrieved chunks by relevance | `{ query, documents }` | `{ response: [reranked documents] }` | Retrieved chunks to ranked top results |
| RAG Agent | Chat context, tools | System prompt and tool chain enabled | Orchestrates retrieval and answer generation | `{ action, sessionId, chatInput }` plus tool results | `{ output }` | Multi-step retrieval-augmented reasoning |
| OpenAI Chat Model | Model, messages, response settings | `gpt-5.4-mini`, Responses API enabled | Produces the final natural-language answer | Conversation history and tool outputs | `{ response, tokenUsageEstimate }` | Full context to final answer text |

# JSON Summary Comparison

| Node | JSON Input Summary | JSON Output Summary | Field Changes | Notes |
|------|--------------------|---------------------|---------------|------|
| Store in Pinecone | Binary file: `guidelines_for_trustworthy_ai.pdf`, PDF, `application/pdf`, size `1.63 MB` | Array of objects with `metadata` and `pageContent` | `file` removed; `metadata` added; `pageContent` added | `8 items returned`; input changes from binary to structured JSON; `blobType` becomes `application/json`, which suggests the PDF text is not being extracted correctly |
| Document Loader | `{ trustworthy_ai, submittedAt, formMode }` form payload | `{ response: [{ pageContent, metadata }] }` | `trustworthy_ai`, `submittedAt`, `formMode` removed; `response`, `pageContent`, `metadata` added | Loader output reflects document chunks, but the node is still reading form metadata instead of real PDF text because `Type of Data` is set to `JSON` |
| OpenAI Embeddings | `{ documents: [string, string, ...] }` containing filenames, MIME types, timestamps, and form values | `{ response: [[float, float, ...]] }` with 1,536 values per vector | `documents` removed; `response` added | The embedding model is working, but it is embedding metadata strings instead of meaningful document content |
| Text Splitter | `{ textSplitter: "string" }` where the value is effectively `application/pdf` | `{ response: ["string"] }` | `textSplitter` removed; `response` added | Chunking runs across all 8 items, but the splitter receives MIME-type text instead of extracted document text |
| Retrieve from Pinecone | `{ action: "sendMessage", sessionId: "...", chatInput: "..." }` | `{ output: "string" }` | `action`, `sessionId`, `chatInput` removed; `output` added | The agent consumes the user query and returns a completed answer with inline citations |
| Cohere Reranker | `{ query: "string", documents: [{ pageContent, metadata }, ...] }` | `{ response: [{ pageContent, metadata }, ...] }` | `query` and `documents` removed; `response` added | Top `N = 3` documents are returned; chunking-related metadata such as `chunk_size` and `chunk_overlap` is stripped out |
| OpenAI Chat Model | `{ messages: [...], estimatedTokens, options }` | `{ response: { generations: [[{ text }]] }, tokenUsageEstimate }` | `messages`, `estimatedTokens`, `options` removed; `response`, `tokenUsageEstimate` added | Final answer is generated using the conversation chain; token estimate is preserved in the output |
| Query Embeddings | `{ query: "string" }` | `{ response: [float, float, ...] }` | `query` removed; `response` added | The user question is converted into a 1,536-dimensional vector for Pinecone similarity search |
| RAG Agent | `{ action: "sendMessage", sessionId: "...", chatInput: "..." }` | `{ output: "string" }` | `action`, `sessionId`, `chatInput` removed; `output` added | The agent consumes the full query context and returns the final response text |
