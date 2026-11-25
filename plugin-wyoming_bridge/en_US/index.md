---
layout: default
title: Wyoming Bridge — Jeedom voice control (Wyoming)
lang: en_US
pluginId: wyoming_bridge
---

# Wyoming Bridge

The **Wyoming Bridge** plugin lets you control Jeedom by voice from a [Wyoming](https://github.com/rhasspy/wyoming) voice pipeline (satellites, Whisper, Piper).

It exposes a Wyoming **handle** service (intent handling): the pipeline sends it the transcribed text, the plugin passes it to the **AI Assistant** plugin, which first tries a Jeedom interaction (if the option is enabled, see below) and otherwise uses its LLM with the Jeedom MCP tools. The text reply goes back to the pipeline, which has Piper speak it.

The plugin replaces nothing in your existing voice pipeline: it is added as one more Wyoming service.

## Requirements

- Jeedom 4.5 or later, Debian 11 or later.
- The **AI Assistant** plugin, enabled, with a **default MCP client** configured.
- A Wyoming voice pipeline (for example Home Assistant Assist, or a Wyoming orchestrator) able to use a "handle" service.

## Installation

1. Install and enable the plugin.
2. Install the **dependencies** (Python environment with the `wyoming` package).
3. Fill in the configuration (below), especially **Allowed IPs**.
4. The daemon starts automatically (automatic daemon management on the configuration page).

## Plugin configuration

| Setting | Purpose | Default |
|---|---|---|
| TCP port | Port of the Wyoming service | 10500 |
| Bind | Listening address; `0.0.0.0` = all interfaces | 0.0.0.0 |
| Timeout (s) | Maximum time given to AI Assistant to answer (5 to 120 s) | 20 |
| Failure message | Sentence spoken by Piper when no answer could be produced | "Désolé, je n'ai pas pu répondre." |
| Allowed IPs | IPs or networks (CIDR) allowed to connect, comma separated | — |
| Internal token | Token protecting the exchange between the daemon and Jeedom, hidden (the eye shows it); the red button regenerates it. No need to copy it | generated |

**Allowed IPs is mandatory** as soon as the listening address is not `127.0.0.1`: the Wyoming protocol has no authentication, so this list is the only protection. Without it, the daemon refuses to start.

Example: `192.168.1.0/24` for the whole local network, or `192.168.1.50, 192.168.1.51` for two specific devices.

## Fast replies without the LLM

AI Assistant can try **Jeedom interactions** before calling the LLM: a sentence recognized by an interaction gets its reply in a few milliseconds, without using any tokens. This option is **disabled by default**: tick **Essayer interactions Jeedom d'abord** (try Jeedom interactions first) on the AI Assistant equipment used as the default MCP client. The **Health** window shows whether it is enabled.

## Connecting the voice pipeline

In the software that orchestrates the pipeline, declare a Wyoming service at `Jeedom_IP:10500`, of type **handle** / intent handling. The service announces itself as `jeedom`, with the Jeedom language.

## Plugin page

- **Configuration**: opens the plugin configuration.
- **Test**: sends a sentence to the daemon over the Wyoming protocol, exactly like a satellite. The window shows the daemon state, the reply, whether it was handled, the duration and the AI Assistant profile used. Requested actions are really executed and each test may cost one LLM call.
- **Health**: dependencies, daemon, listening address, allowed IPs, timeout, versions (Python, installed `wyoming` package and latest available, installation libraries), AI Assistant, default MCP client and the Jeedom interactions option.
- **Wyoming Bridge equipment**: created automatically, with these info commands, usable in scenarios:

| Command | Content | Historized by default |
|---|---|---|
| Daemon state | 1 = running (checked every 5 minutes) | no |
| Last sentence | Text received from the satellite | no |
| Last reply | Text sent back (empty if not handled) | no |
| Last request handled | 1 = answer obtained, 0 = failure | yes |
| Last request duration | In milliseconds, Jeedom side | yes |
| Last satellite | IP address of the satellite, or `local` | no |

Commands are updated after the reply has been sent: they add no delay. The last sentence and last reply are visible to Jeedom users; do not historize these two commands if you do not want to keep conversations.

## How it works

- Each satellite (identified by its IP address) has its own rate limit and its own conversation history in AI Assistant. If a single orchestrator relays all satellites, they share its limit and history.
- AI Assistant accepts one request every 2 seconds per caller: a sentence sent too early gets "Un instant, réessayez dans quelques secondes." (one moment, try again in a few seconds).
- The reply is cleaned before being sent back: formatting (bold, headings, lists, tables) and emojis are removed so that Piper reads it naturally.
- On timeout, error or unavailable AI Assistant, the pipeline receives "not handled" with the failure message.
- Sentences and replies are written to the logs only at Debug level.
- A connection idle for 10 minutes is closed, and a single IP address cannot open more than 8 simultaneous connections.

## Troubleshooting

| Symptom | Check |
|---|---|
| The daemon does not start | Health window: Allowed IPs empty while Bind is not `127.0.0.1`, or dependencies to install |
| The satellite never gets an answer | Satellite (or orchestrator) IP missing from Allowed IPs; `wyoming_bridge_daemon` log at Warning level (one message per hour per refused IP) |
| "Désolé…" reply | Timeout too short for the LLM, or no default MCP client in AI Assistant |
| "Un instant, réessayez…" reply | Two sentences less than 2 seconds apart from the same satellite (or the same orchestrator) |
| Every sentence goes through the LLM, even the simplest | Jeedom interactions option disabled in AI Assistant (see the Health window) |
| The Test button says "IP refused" | Add `127.0.0.1` or the Jeedom IP to Allowed IPs |

Logs: `wyoming_bridge` (PHP side) and `wyoming_bridge_daemon` (Python daemon).
