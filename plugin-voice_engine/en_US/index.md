---
layout: default
title: Voice Engine — Voice and listening for the box (TTS, STT)
lang: en_US
pluginId: voice_engine
---

# Voice Engine

Voice Engine is the **voice** (text → speech) and **listening** (speech → text) engine of your Jeedom box.

Each equipment is a **profile**: a provider, a model and a voice. You can have several profiles (a soft voice for the
evening, a fast voice for alerts, a Voxtral listening profile…) and use them in your scenarios.

## Providers

| Provider | Voice | Listening | Key | Notes |
|---|---|---|---|---|
| **OpenAI-compatible** | yes | yes | depends on the server | 9router (**free** edge-tts and google-tts voices), ai_proxy (Mistral Voxtral voice and listening with ai_proxy's Mistral account), OpenAI (`gpt-4o-mini-tts`, tone instructions, shut down by OpenAI on January 6, 2027; listening `gpt-transcribe`), Groq (Whisper listening, free tier), OpenRouter (Fish Audio, Kokoro…; listening requires at least $0.50 of credit), local server (Kokoro-FastAPI, Speaches, Piper). "Load voices" lists the 9router Edge voices, the ai_proxy Marie voices, and the OpenAI or local server voices. |
| **ElevenLabs** | yes | yes (Scribe) | yes | Very natural voices. "Load voices" button. The key needs the "speech_to_text" permission for listening. "Clone" button (ElevenLabs paid plan). |
| **Mistral (Voxtral)** | yes | yes | yes | Voxtral TTS voice (hosted in Europe), Voxtral listening. Audio requires a paid Mistral subscription (otherwise "HTTP 402"). Streamed sound, "Clone" button. |
| **Gemini** | yes | yes | yes | 30 preset voices, free style ("warm", "whispered"…). Listening with Gemini models (`gemini-3.5-flash-lite` by default). Without a style, the voice is streamed (first sound in under a second). |
| **Cartesia (Sonic)** | yes | — | yes | The lowest latency. French voices (including Canadian, Belgian and African accents). "Clone" button. |
| **Google Translate** | yes | — | no | Free, no account, average quality. Handy as a fallback. |
| **Wyoming** | yes | yes | no | Voice server on your network, the one Home Assistant uses: Piper for speech (port 10200), Whisper for listening (port 10300). Nothing leaves the house. "Load voices" lists the voices installed on the server. |

Keys are entered **once** in the plugin configuration (one per provider). A profile can have its own key (the
equipment "API key" field). Keys never leave the box.

## Getting started

1. **Plugins → Plugin management → Voice Engine**: enable the plugin, enter the keys of the providers you use, save,
   then click "Test" next to each key.
2. **Plugins → Multimedia → Voice Engine → Add**: name the profile, choose the provider, the model and the voice (the
   "Load voices" button lists the voices of your account), then save.
3. **Test**: listen to the profile directly in the browser, and try listening with a round trip (the sentence is spoken
   by a voice, then transcribed).

## Commands

| Command | Role |
|---|---|
| **Say** | Speaks the message on the profile output (see below). |
| **Create file** | Makes the sound without playing it. "Last file" gives its path on the box, "Last URL" an address for another device. |
| **Play sound** | Plays an existing audio file (path in the Jeedom folder or in /tmp) or a URL. |
| **Transcribe** | Transcribes an audio file (path in the message); the text goes to "Last transcript". |
| Last text spoken, Last file, Last URL | Result of the last sound. |
| Characters today | Characters synthesized today with this profile (sounds already in the cache do not count). |
| Last transcript | Text of the last listening. |
| Last call duration, Last error | Monitoring ("Last error" is empty when all is well). |

Voice commands only exist when "This profile speaks" is checked, listening commands when "This profile transcribes" is.

## "Say" output

- **None**: the sound is only made (file and URL).
- **Box speaker**: played by `ffplay`. Playbacks follow one another without overlapping. The sound card is chosen in
  the plugin configuration. **Sentence by sentence** (ticked by default): a long text starts playing as soon as its
  first sentence is ready, the next ones are made during playback.
- **Jeedom command**: the sound is sent to an action command of another plugin (Chromecast, Sonos, voice satellite,
  kiosk browser…). The "Content sent" field accepts `#url#` (sound address), `#text#` (spoken text) and `#file#`
  (local path). **Streamed sound**: the command immediately receives an address whose sound arrives while it is being
  made (playback starts sooner); untick it if a player needs a complete file.
- **Speaker of another machine (SSH)**: the sound is copied to the machine (Raspberry Pi, PC…) then played by the
  profile's playback command (`mpg123 -q #file#` by default). Password or key login; the machine's fingerprint is
  recorded on first connection, and a machine whose fingerprint changes is refused.

In a scenario, the **Title** field of "Say" or "Play sound" can pick another output than the profile's: `none`,
`local`, `cmd` or `ssh`.

## AI rephrasing

**Rephrase with AI** box of a speech profile: before being spoken, the text is rewritten for speech by an ai_proxy
model ("21.5°C" becomes "twenty-one point five degrees", short sentences, no formatting). An optional **instruction**
changes the tone ("warm, informal", "very short"). The model is chosen in the plugin configuration
(`mistral-small-latest` by default). The same text is only rewritten once (kept 30 days); if ai_proxy does not answer,
the text is spoken as is.

## Cloned voices

**Clone** button next to the Voice field (ElevenLabs, Cartesia, Mistral): send a clean recording of one to a few
minutes, a single person, no music. The voice is created on the provider account and its id is put in the Voice
field: save the profile to use it. Only clone a voice whose owner agreed. Cloning may require a paid plan with the
provider.

## Links to sounds

Sounds are served through a **signed**, **time-limited URL** (one hour by default): a Chromecast or a satellite can
play it without a Jeedom account, but nobody can guess the address of another sound. The address given to devices is
the Jeedom internal access (configurable: external access).

## Jeedom TTS engine

Voice Engine can replace the Jeedom TTS engine (Pico, eSpeak): **Settings → System → Configuration → TTS engine →
Plugin Voice Engine**. Every plugin using the Jeedom speech synthesis then uses the **default voice profile** (plugin
configuration).

## Cap and fallback

- **Daily cap**: maximum number of characters per day and per profile (paid providers bill per character). Beyond it,
  the profile refuses to speak until the next day.
- **Fallback profile**: used if the provider fails (network, quota, cap). Example: ElevenLabs with Google Translate as
  a fallback.

## Cache

Every sound is kept: the same sentence with the same voice is made (and billed) only once. The cache is limited in
size and age (plugin configuration) and cleaned every night. The "Empty" button deletes it entirely.

## Health

The **Health** window checks ffmpeg and ffplay, the cache, the Jeedom TTS engine, the default profiles and the keys,
and shows for each profile its characters of the day and its last error (updated live).

## Formats

Sounds are MP3. Other formats (WAV 16 kHz, FLAC 48 kHz, OGG) are produced by ffmpeg on demand (Test window, and later
other plugins). Without ffmpeg, a sound stays in the provider's format (WAV for Gemini).
