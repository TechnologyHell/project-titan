# TITAN - Phase 2 AI Infrastructure Layer

## Objective

Transform TITAN from a GPU-ready homelab server into a fully operational self-hosted AI platform.

Phase 2 focused on:
- local LLM deployment
- GPU accelerated AI inference
- browser-based conversational AI
- multimodal AI capabilities
- local AI orchestration
- AI frontend/backend architecture
- web-augmented AI interactions



# AI Infrastructure Stack

## Core Components Deployed

| Component      | Purpose                          |
|----------------|----------------------------------|
| Ollama         | Local LLM runtime backend        |
| Open WebUI     | Browser AI frontend              |
| Gemma 3 4B     | Primary conversational model     |



# Ollama Runtime Layer

## Installed

- Ollama
- GPU runtime integration
- persistent model storage

## Configured

- Dockerized deployment
- NVIDIA runtime support
- CUDA acceleration
- persistent AI model volumes

## Verified

- local inference
- GPU acceleration
- model loading/unloading
- CUDA container runtime
- API accessibility



# NVIDIA AI Runtime Verification

Verified through:
- docker CUDA containers
- Ollama GPU inference
- live VRAM monitoring
- GPU compute utilization

Observed:
- active CUDA workloads
- sustained tensor compute
- GPU burst inference behavior



# AI Model Deployment

## Primary Model

### Gemma 3 4B

Selected because:
- lightweight deployment
- efficient VRAM utilization
- fast inference speed
- low thermal impact
- ideal always-on assistant model

## Runtime Characteristics

| Metric              | Observation            |
|---------------------|------------------------|
| VRAM Usage          | ~4GB                   |
| GPU Load            | 0–90% burst workloads  |
| Idle Power          | ~40W                   |
| Peak Power          | ~190W                  |
| Peak Temperature    | ~70°C                  |

## Important Learnings

Observed:
- models remain cached in VRAM
- rapid load/unload behavior
- burst-style GPU inference workloads
- longer generations increase sustained compute load



# Open WebUI Deployment

## Installed

- Open WebUI

## Purpose

Provide:
- ChatGPT-style local AI experience
- browser-based AI interaction
- persistent conversation management
- multimodal AI interface

## Configured

- Ollama backend integration
- local AI routing
- persistent conversation storage

## Features Verified

- browser chat interface
- multiple chat sessions
- persistent conversations
- model selection
- image upload support
- multimodal AI interaction



# Multimodal AI Capability

## Image Understanding

Successfully tested:
- image uploads
- scene analysis
- visual object understanding
- image-based prompts

Gemma 3 4B successfully performed:
- local image recognition
- contextual visual interpretation

without requiring external cloud APIs.



# Web-Augmented AI Features

## Website Scraping & Retrieval

Verified:
- webpage reading
- URL summarization
- content extraction
- AI-assisted webpage interpretation

Observed:
- retrieval context injection
- tool-assisted conversational responses
- experimental context persistence behavior



# AI Architecture Concepts Learned

## Backend vs Frontend Separation

### Ollama
Acts as:
- inference backend
- model runtime engine
- local AI API layer

### Open WebUI
Acts as:
- frontend conversational interface
- persistent chat manager
- browser interaction layer



# Current AI Architecture

Browser
   ↓
Open WebUI
   ↓
Ollama
   ↓
Gemma 3 4B
   ↓
RTX 3060 Ti