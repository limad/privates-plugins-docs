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

Listening and reply settings apply without a restart. Network settings (ports, addresses) require restarting the daemon.

On each satellite, **End of speech**, **Maximum listening** and **Listen again after a question** override the plugin setting (empty = plugin setting). A noisy room often needs a longer end of speech.

## Satellite room

The satellite's parent object is given to AI Assistant as the room the user speaks from: "turn on the light" targets the light of that room, without naming it.

## Commands

| Command | Type | Purpose |
|---|---|---|
| Connected | binary info | daemon connection to the satellite |
| State | text info | `idle`, `listening`, `thinking`, `speaking` |
| Last sentence / Last reply | text info | last exchange |
| Last reply duration | numeric info (ms) | from the end of the sentence to the voice being sent |
| Last error | text info | failed transcription, reply, voice or connection (triggers a scenario on every error, even a repeated one) |
| Volume / Muted | info | satellite player state |
| Wake word | text info | active wake word |
| Timer running / Last finished timer | info | timer tracking |
| Announce | message action | makes the satellite say a text |
| Announce and listen | message action | says the text, then listens to the answer (no wake word) |
| Stop playback | action | interrupts the current reply or announcement |
| Set volume | slider action (0-100) | satellite volume |
| Mute / Unmute | action | mute |
| Choose wake word | list action | list read from the satellite |
| Start a timer | message action | title = name (optional), message = duration (`5 min`, `1h30`, `90 s`; a plain number = seconds) |
| Cancel timers | action | cancels the satellite timers |

## Voice timers

Timer sentences are recognised in French: « Mets un minuteur de 10 minutes », « Combien de temps reste-t-il sur le minuteur ? », « Annule le minuteur ». To « Mets un minuteur », the satellite asks « Pour combien de temps ? » and waits for the duration.

These sentences are handled by the plugin, without calling the LLM. The satellite shows the countdown and rings at the end, if it supports timers.

On `linux-voice-assistant`, a ringing timer stops with the wake word, "Stop" or the button. "Stop playback" only cuts the current sound: LVA starts ringing again.

Running timers are kept when the daemon or the box restarts. A timer that ended while stopped does not ring, but updates "Last finished timer".

"Cancel" and "time left" only target the satellite timers while one of them is running. Otherwise, the sentence goes to Jeedom (for example for an equipment named "minuteur").

## Wake word

You can only choose among the wake words the satellite knows. The official ESPHome models are in English. A French wake word needs a trained model:
- on an ESP32, the firmware must be recompiled;
- with `linux-voice-assistant` (LVA, on a Pi or a PC), adding an openWakeWord model is enough, without changing the plugin.

### French wake word with linux-voice-assistant

1. **Model**: an openWakeWord model in **TFLite** format (`.tflite`). LVA does not read ONNX. A model produced by openWakeWord training is ONNX: convert it while keeping the `[1, 16, 96]` input (`onnx2tf` transposes it to `[1, 96, 16]`: check the resulting shape).
2. **Description**: a JSON file next to the model, for example `frida.json`:
   ```json
   {
     "type": "openWakeWord",
     "wake_word": "Frida",
     "model": "frida.tflite",
     "trained_languages": ["fr"],
     "openWakeWord": { "probability_cutoff": 0.8 }
   }
   ```
3. **Folder**: mount the folder in the container and point LVA to it. The built-in wake words stay available. Also mount `/app/configuration`: LVA keeps the chosen word there.
   ```yaml
   environment:
     WAKE_WORD_DIR: /app/wakewords-custom
   volumes:
     - /path/to/wakewords:/app/wakewords-custom:ro
     - /path/to/configuration:/app/configuration
   ```
4. **Choice**: restart LVA. The word appears in the satellite's **Choose wake word** command; select it. It stays active after LVA restarts.

**Threshold** (`probability_cutoff`): 0.8 is recommended (LVA uses 0.7 when the JSON sets none). Measured for "Frida": about 88% detection and less than one false detection per hour. Higher means fewer false detections, but more missed words. LVA's "Wake Word Sensitivity" entity, when set, overrides this value.

**In use**: LVA only listens after its beep, about 2 s after the wake word. Wait for the beep before asking.

**LVA "Stop" word**: during a reply or a ringing timer, LVA also listens for its "Stop" word, an English model. Measured on French synthetic voices, it often fires on other words. At the default setting (0.5), it cuts the reply on 56% of "Frida" and 24% of ordinary sentences, and recognises only 64% of real "Stop". With `"stop_word_sensitivity": 0.8` in `/app/configuration/preferences.json` (edit it while LVA is stopped), these unwanted stops drop to 25% and 11%, and 56% of real "Stop" are still recognised. Above 0.81, the "Stop" word never fires.

## Voice

The satellite starts speaking at the first sentence of the reply, while the AI is still writing the rest: each sentence is synthesised as soon as it is complete (FLAC satellites, the default format; OpenAI-compatible provider such as DeepSeek, Mistral, Groq or OpenAI for the AI Assistant MCP client, otherwise the reply arrives in one piece and is then spoken sentence by sentence). Short sentences already spoken (timer replies, failure message, repeated announcements) are kept in memory for a few hours and replayed without a new synthesis.

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
