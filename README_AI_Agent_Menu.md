# 🍽️ AI Restaurant Assistant (RAG) – n8n Workflow

An AI-powered restaurant assistant built with n8n, OpenAI, and Supabase Vector Store.

This workflow allows restaurant staff or customers to interact with an AI chatbot that retrieves accurate menu information and pricing from uploaded restaurant menu files using Retrieval-Augmented Generation (RAG). The workflow consists of two independent pipelines: a document ingestion workflow and an AI chat assistant.

---

## Features

- AI chatbot powered by OpenAI
- Vector search using Supabase
- RAG architecture for accurate menu retrieval
- File upload ingestion via n8n form trigger
- Automatic embeddings generation using OpenAI
- Semantic search for menu items and pricing
- Supabase Vector Store used as a retrievable AI tool
- Top-3 most relevant results returned per query

---

## Tech Stack

- n8n
- OpenAI GPT Models
- OpenAI Embeddings
- Supabase Vector Store
- RAG (Retrieval-Augmented Generation)

---

## Workflow Overview

### 1. Document Ingestion Pipeline

Restaurant menu files are uploaded through an n8n form trigger.

#### On Form Submission
An n8n form accepts file uploads from the user.  
Any file type supported by the Default Data Loader can be used (PDF, DOCX, TXT, etc.).

#### Default Data Loader
Reads the uploaded binary file and prepares it as a LangChain document for processing.

#### Embeddings OpenAI
Converts the document content into vector embeddings using OpenAI's embeddings model.

#### Supabase Vector Store (Insert)
Stores the generated embeddings into the `documents` table in Supabase.  
This builds the knowledge base the AI agent will query at runtime.

---

### 2. AI Chat Assistant Pipeline

Users interact with the chatbot through the n8n chat interface.

#### When Chat Message Received
Triggers the assistant pipeline when a user sends a message.

#### AI Agent
An OpenAI-powered agent that:
- receives the user's question
- uses the Supabase Vector Store as a retrieval tool
- responds with accurate menu information and pricing

#### OpenAI Chat Model
Provides the language model backend to the AI Agent.

#### Supabase Vector Store (Retrieve as Tool)
Configured in `retrieve-as-tool` mode, this node:
- performs semantic search across the stored menu embeddings
- returns the top 3 most relevant results
- feeds the retrieved context directly into the AI Agent's response

#### Embeddings OpenAI (Retrieval)
Converts the user's query into embeddings for semantic similarity search against the stored menu vectors.

---

## Use Cases

- Restaurant AI assistant for menu inquiries
- AI-powered menu search and pricing lookup
- Customer support automation for food businesses
- Smart restaurant chatbot for staff or customers
- Knowledge base assistant using RAG architecture

---

## Setup Instructions

1. Import the workflow into n8n
2. Configure OpenAI API credentials in n8n
3. Create a Supabase project at [supabase.com](https://supabase.com)
4. Create a `documents` table in Supabase with vector support (follow the [n8n Supabase Vector Store setup guide](https://docs.n8n.io/integrations/builtin/cluster-nodes/root-nodes/n8n-nodes-langchain.vectorstoresupabase/))
5. Configure Supabase API credentials in n8n
6. Open the n8n form URL and upload your restaurant menu file to populate the vector store
7. Open the n8n chat interface and start asking questions about the menu

---

## Notes

Credentials and API keys are not included in this repository.  
Replace all `YOUR_*` placeholder values with your own credentials inside n8n.  
The document ingestion and chat assistant are two separate triggers — run the ingestion pipeline first before using the chat assistant.  
This project is intended for educational and test purposes.
