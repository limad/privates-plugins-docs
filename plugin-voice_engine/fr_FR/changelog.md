---
layout: default
title: Voice Engine — Changelog
lang: fr_FR
pluginId: voice_engine
---

# Changelog Voice Engine

## Version en cours de développement

### 27/11/2026

- **Nouveautés** : « Charger les voix » fonctionne aussi pour 9router (voix Edge), ai_proxy (voix Marie) et OpenAI ;
  choix de la carte son de la box ; lecture **phrase par phrase** sur la box (un long texte commence plus tôt) ;
  **son au fil de l'eau** vers un autre plugin (Chromecast, Sonos…) ; nouvelle sortie **haut-parleur d'une autre
  machine (SSH)** ; **écoute avec Gemini**.
- **Corrections** : fin des sons courts plus propre sur la box ; message clair quand un fournisseur n'a plus de
  crédit (au lieu de « clé refusée ») ; écoute OpenAI passée au modèle qui remplace ceux arrêtés en février 2027.
- **Nouveautés et corrections** : bouton « Importer depuis AI Assistant » dans la configuration, qui reprend les clés
  des fournisseurs (les clés déjà saisies sont gardées) ; le préréglage ai_proxy trouve tout seul l'adresse et la clé
  d'ai_proxy, et sait maintenant parler en plus d'écouter (voix Mistral, avec ai_proxy à jour).
- **Correction** : le préréglage 9router reprend tout seul la clé du plugin 9Router ; sans elle, les dernières
  versions de 9router refusaient la voix.

## 0.1.0 (2026-11-26)

- **Première version** : un équipement par voix ou par écoute, avec le fournisseur de votre choix : 9router et ai_proxy
  (sur la box), OpenAI, Groq, OpenRouter, ElevenLabs, Mistral Voxtral, Gemini, Cartesia ou Google Translate (gratuit).
- **Commandes pour vos scénarios** : Dire, Créer le fichier, Lire un son, Transcrire.
- **Où va le son** : haut-parleur de la box, ou un autre plugin qui joue un lien (Chromecast, Sonos, satellite vocal…).
  Les liens vers les sons sont protégés et valables une heure.
- **Moins de frais** : une même phrase avec la même voix n'est fabriquée (et facturée) qu'une fois ; plafond de
  caractères par jour ; profil de secours si un fournisseur ne répond pas.
- **Voix de tout Jeedom** : le plugin peut remplacer le moteur TTS de Jeedom.
- Fenêtres **Tester** (écouter une voix, essayer l'écoute) et **Santé**.
- Interface traduite en anglais.
