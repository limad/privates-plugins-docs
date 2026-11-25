---
layout: default
title: Wyoming Bridge — Changelog
lang: en_US
pluginId: wyoming_bridge
---

# Wyoming Bridge changelog

## 2026-11-25

- More natural spoken replies: formatting (bold, headings, lists, tables) and emojis added by the AI are removed before Piper reads them.
- Sentence too close to the previous one: the satellite says "Un instant, réessayez dans quelques secondes." (one moment, try again in a few seconds) instead of the failure message.
- **Health** window: shows whether AI Assistant tries Jeedom interactions before the LLM (option disabled by default, to tick in AI Assistant for instant replies).
- Lighter daemon log: the regular check made by Jeedom no longer writes a warning, and a refused IP is reported only once per hour.
- Reliability: idle connections closed after 10 minutes, at most 8 simultaneous connections per IP address.
- Faster dependency check.
- Internal token hidden in the configuration (eye button to show it).
- **Conversation thread** with Home Assistant: each satellite keeps its own history, reset on each new conversation and kept for follow-up questions.
- When the reply ends with a question, the satellite listens again without a wake word.
- Language of the request passed to the AI; **All languages** option to use the service in a voice pipeline of another language.
- **Voice instructions** option: short replies suited to voice by default, editable.
- Requires an up-to-date AI Assistant for the conversation thread and voice instructions.

## 2026-11-10

- Core heartbeat: a running daemon is now seen as communicating (otherwise, with a heartbeat set, the core would have reported it silent).
- Tracking commands: last sentence, last reply, handled or not, duration, last satellite (updated after the reply is sent).
- Documentation and changelog, published on the documentation site.
- `wyoming` package 1.11.0.

## 2026-11-09

- Plugin page in the Jeedom format: Configuration, Test, Health, equipment and commands.
- **Test** window: sends a sentence over the Wyoming protocol, like a satellite.
- **Health** window: overall state, installed versions and latest available versions.
- Default values shown in the configuration (port, bind, timeout, failure message).
- Rate limit and history per satellite.
- Service version announced from the plugin.
- Timeout passed to AI Assistant: no action is started after a "not handled" reply.
- Failure message spoken to the satellite instead of silence; announced language = Jeedom language.
- Security: Allowed IPs mandatory outside `127.0.0.1`, token kept off the command line, bounded network reads, daemon stop limited to its own processes.
- Daemon autostart handled by the core; installation libraries embedded.

## 2026-10-13

- `handle.php` answers 401 when the plugin is disabled.

## 2026-09-20

- Token encrypted in the database, AGPL-3.0 license.

## 2026-09-08

- First version.
