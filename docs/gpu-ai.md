# GPU Infrastructure
Zotac Twin Edge RTX 3060ti 8GB LHR

## GPU
NVIDIA GeForce RTX 3060 Ti

## Driver
nvidia-driver-595-open

## CUDA
CUDA Version 13.2

## Verification
GPU successfully detected through:
nvidia-smi

## Monitoring
watch -n 1 nvidia-smi
nvtop

# NVIDIA Container Toolkit
Configured Docker GPU passthrough support using NVIDIA Container Toolkit.

## Purpose
Allows Docker containers to access:
- NVIDIA GPU
- CUDA runtime
- GPU acceleration features

Required for:
- Ollama
- AI inference workloads
- GPU accelerated containers
- hardware transcoding

# Verification
Verified GPU access inside Docker container using:
docker run --rm --gpus all nvidia/cuda:12.9.1-base-ubuntu24.04 nvidia-smi

## Runtime Characteristics for Ollama with Gemma3:4B

| Metric              | Observation            |
|---------------------|------------------------|
| VRAM Usage          | ~4GB                   |
| GPU Load            | 0–90% burst workloads  |
| Idle Power          | ~40W                   |
| Peak Power          | ~190W                  |
| Peak Temperature    | ~70°C                  |


# Installed Models

## Primary Model
Model: Gemma 3 4B

Purpose:
- Always available local assistant
- Infrastructure management
- Coding assistance
- Documentation support
- General AI interaction

Advantages:
- Fits entirely within RTX 3060Ti VRAM
- Fast inference
- Low resource consumption
- Suitable for always-on deployment

Current Deployment:
Open WebUI -> Ollama -> Gemma3:4b


# Planned AI Expansion

## TITAN Assistant

Planned capabilities:

- Voice Assistant
- Speech To Text
- Text To Speech
- Internet Search
- Local Knowledge Base
- Home Automation Integration
- Camera Analysis
- Autonomous Agent Workflows

Target Architecture:

User
 ↓
OpenWebUI
 ↓
Ollama
 ↓
Gemma/Qwen
 ↓
Tools Layer
 ↓
Infrastructure + Cameras + Home Assistant


# Current AI Workloads

The RTX 3060 Ti currently accelerates the following workloads:

## Local Large Language Model

Framework:
- Ollama

Model:
- Gemma3:4B

Purpose:
- conversational AI
- infrastructure assistant
- programming assistance
- documentation generation
- local reasoning

Inference:
- GPU accelerated
- fully offline



## Speech Recognition

Framework:
- Faster Whisper

Purpose:
- real-time speech transcription
- voice assistant input

Execution:
- CUDA accelerated

Benefits:
- significantly lower transcription latency
- near real-time speech recognition



## Voice Assistant

Project:
JARVIS

Pipeline:

Wake Word
↓
Speech Recognition
↓
Gemma 3
↓
Text To Speech
↓
Audio Output

Execution:
Entire conversational pipeline executes locally.



# GPU Memory Allocation

Current approximate VRAM usage:

| Workload              | VRAM |
|-----------------------|------|
| Gemma3:4B             | ~4 GB |
| Faster Whisper        | ~1 GB |
| CUDA Runtime          | ~0.5 GB |
| Remaining Headroom    | ~2.5 GB |

Result:

Current hardware comfortably supports simultaneous conversational AI workloads.



# Current AI Capabilities

TITAN currently supports:

- Local conversational AI
- Voice interaction
- GPU accelerated inference
- Offline speech recognition
- Offline text generation
- AI document analysis
- Retrieval Augmented Generation (RAG)
- Image understanding (Open WebUI supported models)



# Future GPU Workloads

Planned GPU accelerated features:

## Computer Vision

Framework candidates:

- YOLO
- Florence-2
- GroundingDINO

Applications:

- object detection
- desk understanding
- surveillance analysis
- people counting
- intrusion detection



## Surveillance AI

Planned features:

- person detection
- vehicle detection
- package detection
- event classification
- smart recording triggers



## AI Agent Framework

Future local agent capable of:

- Linux administration
- Docker management
- Home Assistant control
- Terminal execution
- Infrastructure monitoring
- Autonomous maintenance



## Multi-Model Deployment

Planned local models:

| Model | Purpose |
|--------|----------|
| Gemma3 | General Assistant |
| Qwen | Coding & Reasoning |
| Florence-2 | Vision |
| Whisper | Speech Recognition |
| Piper | Speech Synthesis |



# Long-Term Vision

The NVIDIA RTX 3060 Ti serves as the primary AI accelerator for TITAN.

Target deployment:

User
↓
Voice / Web / Telegram
↓
JARVIS
↓
LLM
↓
Tool Layer
↓
GPU Accelerated AI Services
↓
Linux + Docker + Cameras + Home Assistant + NAS


Goal:
Transform TITAN into a completely self-hosted AI compute platform capable of conversational intelligence, infrastructure automation, computer vision and autonomous local decision making without dependence on external cloud AI services.