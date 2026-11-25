---
layout: default
title: Voice Satellite — Changelog
lang: fr_FR
pluginId: voice_satellite
---

# Changelog Voice Satellite

## 0.4.0 (2026-11-25)

- **Le satellite parle pendant que l'IA écrit** : la première phrase est dite dès qu'elle est complète, sans attendre la fin de la réponse. Mesuré sur un satellite virtuel : 3 s entre la fin de la question et le début de la réponse, contre 5 s auparavant (réponse de cinq phrases). Nécessite AI Assistant à jour ; sinon la réponse arrive d'un bloc, comme avant.
- Voix découpée en phrases : la fin d'une phrase n'attend plus la phrase suivante.
- Réponse au fil de l'eau aussi avec Claude et Gemini comme fournisseur du client MCP d'AI Assistant, en plus des fournisseurs OpenAI-compatibles.

## 0.3.0 (2026-11-25)

- **Voix plus rapide** : une réponse longue est dite dès sa première phrase, la suite est synthétisée pendant la lecture. Phrases courtes déjà dites gardées en mémoire.
- **Pièce du satellite** (objet parent) donnée à AI Assistant : « allume la lumière » vise la pièce du satellite (AI Assistant à jour requis).
- Commandes **Arrêter la lecture** et **Dernière erreur**.
- Réglages d'écoute **par satellite** ; réglages d'écoute et de réponse appliqués **sans redémarrer** le démon.
- **Minuteurs conservés** au redémarrage du démon ou de la box.
- Sauvegarde d'un équipement plus rapide, détection de fin de parole plus rapide au mot d'appel, pose de la clé de chiffrement fiabilisée.

## 0.2.0 (2026-11-25)

- **Rechercher** : découverte des satellites ESPHome du réseau (mDNS), ajout en un clic.
- **Clé de chiffrement** posée depuis Jeedom (ESPHome 2026.7 ou plus), et cause des échecs de connexion affichée (clé requise, refusée, satellite non chiffré).
- Commandes **volume / muet**, **choix du mot d'appel** (liste lue sur le satellite).
- **Minuteurs** : commandes et voix (« mets un minuteur de 10 minutes », « combien de temps reste-t-il », « annule le minuteur »), traités sans LLM, décompte et sonnerie sur le satellite.
- **Relance de l'écoute** quand la réponse se termine par une question.
- Fenêtre **Santé** des satellites.
- Traduction anglaise.

## 0.1.0 (2026-11-24)

- Première version (alpha) : un équipement par satellite ESPHome, session vocale complète (fin de parole silero, transcription, réponse et voix par AI Assistant, conversion au format du satellite), commandes Annoncer et Annoncer et écouter, état de connexion.
