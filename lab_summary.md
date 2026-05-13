# Lab Summary

## Lab Title

n8n Node Study Guide

## Objective

The objective of this lab was to analyze a provided workflow in n8n, document the role of each node, and compare the JSON input and output for the main workflow steps.

## What Was Tested

- Document upload through a form node
- Document loading and text preparation
- Text splitting
- OpenAI embeddings generation
- Pinecone storage
- Query embedding generation
- Pinecone retrieval
- Cohere reranking
- RAG agent response generation
- OpenAI chat model output

## Main Result

The workflow was successfully mapped and documented, but the ingestion side revealed a key issue:

- The Document Loader was configured with `Type of Data = JSON`
- The uploaded PDF was treated as metadata rather than actual binary content
- The downstream nodes therefore processed filenames and MIME-type values instead of real document text

## Important Insight

The RAG pipeline structure is correct, but the document parsing step needs to be fixed for the system to store and retrieve meaningful content.

## Deliverables

- Updated workflow notes
- Improved reference table
- JSON summary comparison table
- Supporting image files organized in the `Documents` folder

## Conclusion

This lab showed how a RAG workflow in n8n combines document ingestion, vector storage, retrieval, reranking, and LLM response generation. It also highlighted how a small configuration issue in the input type can affect every downstream node.
