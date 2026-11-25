---
layout: default
title: Voice Satellite — Changelog
lang: fr_FR
pluginId: voice_satellite
---

# Changelog Voice Satellite

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
