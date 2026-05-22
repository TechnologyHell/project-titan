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