---
layout: default
title: Voice Satellite — ESPHome voice satellites without Home Assistant
lang: en_US
pluginId: voice_satellite
---

# Voice Satellite

Drives **ESPHome voice satellites** directly from Jeedom, **without Home Assistant**.

The satellite detects the wake word ("Okay Nabu", "Hey Jarvis"…). Jeedom then receives the microphone audio, detects the end of the sentence, transcribes it, answers with the **AI Assistant** plugin (Jeedom interactions first, then the LLM) and makes the satellite speak.

## Compatible hardware

Any device exposing a voice assistant through the ESPHome native API:
- Home Assistant Voice PE;
- ESP32-S3-BOX-3;
- Seeed ReSpeaker Lite;
- M5Stack Atom Echo;
- a Raspberry Pi or a PC with a microphone, through [linux-voice-assistant](https://github.com/OHF-Voice/linux-voice-assistant).

> A satellite can only be driven by **one** assistant at a time. Remove it from Home Assistant first.

## Requirements

- The **AI Assistant** plugin, with a default MCP client and a configured **voice server** (speech-to-text and voice).
- The plugin dependencies: Python ≥ 3.11 in a dedicated environment, `aioesphomeapi`, `pysilero-vad` and `ffmpeg`. On Debian 11, Python 3.11 is compiled, so the installation can take a long time.

## Adding a satellite

**Search** (plugin page) lists the ESPHome devices announced on the local network (mDNS). For each one, the window shows whether it exposes a voice assistant and whether it is encrypted. **Add** creates the equipment.

The satellite must be on the same network as Jeedom, and multicast must get through. Otherwise, **Add** lets you enter its address by hand.

### Encryption

- **Satellite already encrypted**: copy the `api: encryption: key` from its ESPHome YAML into the "Encryption key" field.
- **ESPHome 2026.7 or later**, with `api: encryption:` without a key in the YAML: save the equipment, then click **Generate**. Jeedom creates a random key, sets it on the satellite over an already encrypted connection, then saves it.
- **Satellite not encrypted**: leave the field empty.

The key is stored encrypted. When something goes wrong, the connection shows the cause: key required, key refused, or satellite not encrypted.

## Plugin configuration

- **End of speech**: silence (ms) that ends the sentence.
- **Maximum listening**: maximum listening time after the wake word.
- **Reply timeout**: time given to AI Assistant.
- **Listen again after a question**: when the reply ends with a question, the satellite listens again without the wake word.
- **Failure message**: sentence spoken when no reply could be produced.
- **Audio port** and **box address**: where the satellites download the voice.
- **Control port**: local port Jeedom uses to control the daemon.

Restart the daemon after changing these settings.

## Commands

| Command | Type | Purpose |
|---|---|---|
| Connected | binary info | daemon connection to the satellite |
| State | text info | `idle`, `listening`, `thinking`, `speaking` |
| Last sentence / Last reply | text info | last exchange |
| Last reply duration | numeric info (ms) | from the end of the sentence to the voice being sent |
| Volume / Muted | info | satellite player state |
| Wake word | text info | active wake word |
| Timer running / Last finished timer | info | timer tracking |
| Announce | message action | makes the satellite say a text |
| Announce and listen | message action | says the text, then listens to the answer (no wake word) |
| Set volume | slider action (0-100) | satellite volume |
| Mute / Unmute | action | mute |
| Choose wake word | list action | list read from the satellite |
| Start a timer | message action | title = name (optional), message = duration (`5 min`, `1h30`, `90 s`; a plain number = seconds) |
| Cancel timers | action | cancels the satellite timers |

## Voice timers

Timer sentences are recognised in French: « Mets un minuteur de 10 minutes », « Combien de temps reste-t-il sur le minuteur ? », « Annule le minuteur ». To « Mets un minuteur », the satellite asks « Pour combien de temps ? » and waits for the duration.

These sentences are handled by the plugin, without calling the LLM. The satellite shows the countdown and rings at the end, if it supports timers.

"Cancel" and "time left" only target the satellite timers while one of them is running. Otherwise, the sentence goes to Jeedom (for example for an equipment named "minuteur").

## Wake word

You can only choose among the wake words the satellite knows. The official ESPHome models are in English. A French wake word needs a trained model:
- on an ESP32, the firmware must be recompiled;
- with `linux-voice-assistant`, adding an openWakeWord model to its wake word folder is enough.

## Health

**Health** (plugin page) shows, for each satellite:
- the connection and the cause of an error;
- the model and ESPHome version;
- the voice format;
- the announced features (timers, announcements, listening again…);
- the wake word, the volume and the running timers.

## Security

- The daemon connects to the satellites; it does not open the ESPHome API.
- Satellites download the voice over HTTP on the audio port, from random-token URLs valid for 5 minutes.
- The daemon talks to Jeedom through `core/php/pipeline.php`, protected by a token.
- The control port only listens on 127.0.0.1 and requires the same token.
