# TITAN - Phase 5 Interaction : Local Interactive AI Conversation System


# Audio Infrastructure

## Objective

Establish a dedicated audio pipeline for offline voice interaction by integrating a standalone microphone and speaker subsystem suitable for speech recognition and text-to-speech.

---

# Audio Hardware

## Input

- Dedicated Lavalier Microphone
- USB PnP Audio Interface

## Output

- Amazon Basics Soundbar
- USB PnP Audio Interface

The USB audio interface was selected as the common input and output device to simplify future voice assistant integration.

---

# Audio Validation

The complete audio pipeline was validated using ALSA.

Validation included:

- audio device detection
- microphone recording
- speaker playback
- mixer configuration
- recording quality verification

Microphone recording was successfully verified using the dedicated lavalier microphone.

Speaker playback was successfully verified through the connected soundbar.

---

# Audio Configuration

Adopted recording configuration:

- 48 kHz
- 16-bit PCM
- Mono microphone input

Playback configured through the USB audio interface.

---

# Audio Troubleshooting

During deployment several Linux audio issues were encountered including:

- ALSA device permissions
- USB audio routing
- mixer level configuration
- playback channel compatibility

These were resolved through ALSA configuration and mixer calibration.

A minor high-frequency background tone remains present during Linux microphone capture but does not significantly affect speech recognition performance.

---

# Result

TITAN now possesses a fully functional offline audio subsystem capable of supporting future speech recognition and text-to-speech pipelines required for the Jarvis interaction layer.