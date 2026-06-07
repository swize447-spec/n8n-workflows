# AI Restaurant Assistant (RAG) – n8n Workflow

An AI-powered restaurant assistant built with n8n, OpenAI, and Supabase Vector Store.

This workflow allows restaurant staff or customers to interact with an AI chatbot that retrieves accurate menu information and pricing from uploaded restaurant menu files using Retrieval-Augmented Generation (RAG).

## Features

* AI chatbot powered by OpenAI
* Vector search using Supabase
* RAG architecture for accurate menu retrieval
* File upload ingestion workflow
* Automatic embeddings generation
* Semantic search for menu items and pricing
* Conversational restaurant assistant

## Tech Stack

* n8n
* OpenAI GPT Models
* OpenAI Embeddings
* Supabase Vector Store
* RAG (Retrieval-Augmented Generation)

## Workflow Overview

### 1. Document Ingestion

Restaurant menu files are uploaded through an n8n form trigger.

The workflow:

* processes uploaded files
* generates embeddings using OpenAI
* stores vectors inside Supabase

### 2. AI Chat Assistant

Users can interact with the chatbot through the n8n chat interface.

The AI agent:

* searches the vector database
* retrieves relevant menu information
* responds with accurate pricing and menu details

## Use Cases

* Restaurant AI assistant
* AI-powered menu search
* Customer support automation
* Smart restaurant chatbot
* Knowledge base assistant

## Setup Instructions

1. Import the workflow into n8n
2. Configure OpenAI credentials
3. Configure Supabase credentials
4. Create a `documents` table in Supabase
5. Upload restaurant menu files
6. Start chatting with the AI assistant

## Notes

Credentials and API keys are not included in this repository.

This project is intended for educational and portfolio purposes.

