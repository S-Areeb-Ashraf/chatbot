# Chatbot (AI Advisor Helpers)

This repository contains a small Python helper package intended to be used by a FastAPI `ai_advisor` chat endpoint. It provides:
- `schemas.py`: Pydantic models for chat request/response payloads
- `context_loader.py`: loads a shared `system_context.txt` file from the workspace (by searching upward from the package directory)
- `gemini_service.py`: a lightweight wrapper that calls the Gemini REST API to generate responses

## How it works (without RAG)
This project does **not** use Retrieval-Augmented Generation (RAG). There is no embeddings pipeline, vector database, or document retrieval step. Instead, it maintains context by **prompt injection**: the contents of `system_context.txt` are inserted into a strict prompt along with the user’s message, and the model is instructed to answer using **only** the provided context (otherwise respond that the information is not available).

## Environment variables
- `GEMINI_API_KEY` (required): API key used to call the Gemini endpoint
- `GEMINI_MODEL` (optional): model name (default: `gemini-2.5-flash`)
- `GEMINI_API_URL` (optional): override the default API endpoint (useful for testing)

## Typical usage (from a FastAPI endpoint)
1. Read the system context using `load_system_context()`.
2. Pass the context + user message into `generate_response(...)`.
3. Return the model output as the chat reply.

> Note: Conversation history is not stored in this repo. If you want multi-turn memory, include prior messages in the text you send to `generate_response(...)` (or manage sessions in the FastAPI layer).

