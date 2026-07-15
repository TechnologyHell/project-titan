# TITAN - Phase 6 Voice AI (JARVIS Core)

## Objective

Build a fully local, always-available conversational AI assistant capable of natural voice interaction.

Phase 6 focused on:
- wake word detection
- speech recognition
- local LLM integration
- local text-to-speech
- conversational pipeline
- voice session management
- AI personality layer



# Overall Goal

Transform TITAN from an AI inference server into a real-time voice assistant similar to JARVIS.

The assistant should:

- remain idle while consuming minimal resources
- wake only on voice trigger
- understand spoken language
- generate intelligent responses locally
- reply back using synthesized speech
- maintain natural conversation flow
- operate fully offline



# Voice Pipeline Architecture

Implemented pipeline:

Wake Word
↓

Speech Capture
↓

Speech-to-Text
↓

Local LLM
↓

Text-to-Speech
↓

Audio Playback



# Project Structure

Created modular architecture:

```
/srv/friday/

├── assets/
│   └── sounds/
│       └── beep.wav
│
├── core/
│   ├── ai/
│   ├── audio/
│   ├── intents/
│   └── speech/
│
├── models/
│   ├── piper/
│   └── wakeword/
│
├── recordings/
│
└── main.py
```



# Python Environment

Created dedicated virtual environment for FRIDAY/JARVIS.

Installed major dependencies:

- faster-whisper
- openwakeword
- piper-tts
- numpy
- pyalsaaudio
- requests
- torch
- silero-vad
- ollama client



# Audio System

Configured ALSA directly.

Reason:

- lower latency
- headless compatibility
- no dependency on PulseAudio
- stable server operation

Configured:

- USB microphone input
- USB speaker output
- 16 kHz mono recording
- WAV recording pipeline



# Wake Word Detection

Framework:

- OpenWakeWord

Current Wake Word:

- Hey Jarvis

Features:

- continuous background listening
- low CPU usage
- real-time detection
- configurable confidence threshold

Verified:

- accurate detection
- reliable triggering
- minimal false activations



# Speech Recognition

Framework:

- Faster-Whisper

Model:

- Base

Execution:

- CUDA accelerated
- RTX 3060 Ti inference

Features:

- local inference
- offline operation
- fast transcription
- high recognition accuracy



# Local Language Model

Framework:

- Ollama

Model:

- Gemma 3 4B

Execution:

- GPU accelerated

Features:

- fully local inference
- no cloud dependency
- conversational responses
- low latency
- system prompt customization



# AI Personality Layer

Implemented custom personality prompt.

Configured identity:

- JARVIS
- Tony Stark inspired assistant
- professional
- concise
- intelligent
- obedient
- slightly humorous

System awareness includes:

- server identity
- TITAN infrastructure
- hardware specifications
- local deployment context
- user interaction style



# Text-to-Speech

Framework:

- Piper

Selected Voice:

- en_US-danny-low

Reason for selection:

- deep male voice
- robotic tone
- closest match to classic MCU JARVIS
- fast local synthesis
- lightweight inference



# Audio Feedback

Implemented:

- wake confirmation beep
- synthesized speech playback
- automatic response playback

Playback performed directly through ALSA.



# Conversation Flow

Current interaction flow:

User

↓

"Hey Jarvis"

↓

Wake detection

↓

Confirmation beep

↓

Speech recording

↓

Whisper transcription

↓

Gemma response generation

↓

Piper speech synthesis

↓

Audio playback

↓

Conversation continues until silence timeout

↓

Returns to wake word listening state



# Multi-turn Conversations

Implemented:

- continuous conversation mode
- automatic follow-up listening
- session timeout
- automatic return to standby after silence

Features:

- wake once
- multiple follow-up commands
- hands-free interaction



# Local Execution

All inference currently runs locally.

No cloud APIs are required.

Local components:

- Wake Word
- Speech Recognition
- Large Language Model
- Text-to-Speech

TITAN performs complete end-to-end voice processing.



# GPU Acceleration

GPU utilized for:

- Faster-Whisper
- Gemma 3 inference

Benefits:

- significantly reduced response latency
- near real-time interaction
- efficient VRAM utilization



# Performance Achieved

Successfully demonstrated:

- real-time wake detection
- GPU accelerated transcription
- conversational AI responses
- natural speech playback
- continuous voice interaction
- fully offline operation

Average interaction now consists of:

Wake

↓

Question

↓

AI Thinking

↓

Spoken Response

↓

Follow-up Conversation



# Technologies Learned

- wake word detection
- streaming audio processing
- ALSA audio programming
- speech recognition pipelines
- local LLM orchestration
- prompt engineering
- text-to-speech synthesis
- conversational session management
- GPU AI inference
- voice assistant architecture



# Infrastructure Achieved

TITAN now operates as:

- a local conversational AI assistant
- an offline voice interface
- a GPU accelerated AI workstation
- a continuously listening smart assistant
- a foundation for autonomous home automation



# Current Project State

Phase 6 completed successfully.

TITAN now supports:

- wake word activation
- offline speech recognition
- local conversational intelligence
- local speech synthesis
- continuous voice conversations
- customized JARVIS personality
- GPU accelerated AI inference



# Next Phase

## Local Automation & System Control

Planned capabilities:

- Intent routing engine
- Home Assistant integration
- Smart device control
- ONVIF camera interaction
- Docker container management
- Telegram bot integration
- System monitoring
- Hardware telemetry
- Terminal command execution
- Linux automation
- Wake-on-LAN control
- Service management
- File system operations
- Context-aware conversations
- Live system information retrieval

Target architecture:

Voice Command

↓

Intent Detection

↓

Tool / Terminal Execution

↓

Result Processing

↓

Natural Language Response

↓

Speech Playback

Goal:

Evolve JARVIS from a conversational AI into an intelligent local systems administrator capable of controlling the entire TITAN homelab through natural language.