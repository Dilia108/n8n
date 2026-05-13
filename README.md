# LAB | n8n Node Study Guide

This repository contains the notes, reference tables, and supporting files for an n8n lab focused on a RAG chatbot workflow with Pinecone, OpenAI embeddings, and Cohere reranking.

## Project Overview

The main workflow analyzed in this lab is:

`Document Upload RAG Chatbot with Cohere Reranking - May 13th.json`

The workflow is split into two parts:

1. Document embedding and storage
2. Query retrieval, reranking, and answer generation

## Main Files

- [Workflow notes.md](/c:/Users/dilia/OneDrive/IronHack/Week4/n8n/Workflow%20notes.md)
- [Reference table.md](/c:/Users/dilia/OneDrive/IronHack/Week4/n8n/Reference%20table.md)
- [lab_summary.md](/c:/Users/dilia/OneDrive/IronHack/Week4/n8n/lab_summary.md)

## Workflow Summary

### Part 1: Document Embedding and Storage

- Upload a PDF through a form node.
- Load the document content.
- Split the text into chunks.
- Generate embeddings using OpenAI.
- Store vectors and metadata in Pinecone.

### Part 2: Query and RAG Response

- Accept a user question through a chat interface.
- Convert the query into embeddings.
- Retrieve relevant chunks from Pinecone.
- Rerank retrieved chunks using Cohere.
- Generate the final answer with the OpenAI chat model and RAG agent.

## Key Finding

The workflow runs, but the document ingestion path shows a configuration issue:

- The Document Loader is set to `JSON` instead of `Binary`.
- As a result, the workflow passes file metadata instead of true PDF text.
- This affects Text Splitter, OpenAI Embeddings, and Pinecone storage downstream.

## Reference Tables

The repository includes two reference tables in [Reference table.md](/c:/Users/dilia/OneDrive/IronHack/Week4/n8n/Reference%20table.md):

- A node reference table
- A JSON summary comparison table

## Notes on Images

Some PNG files referenced in the notes were moved into the `Documents` folder to keep the root directory cleaner.

## Lab Goal

The goal of the lab was to understand how data moves through the RAG workflow and to document the node behavior, input/output transformations, and configuration issues clearly.
