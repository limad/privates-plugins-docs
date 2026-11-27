---
layout: default
title: Voice Engine — Voix et écoute de la box (TTS, STT)
lang: fr_FR
pluginId: voice_engine
---

# Voice Engine

Voice Engine est le moteur de **voix** (texte → parole) et d'**écoute** (parole → texte) de votre box Jeedom.

Chaque équipement est un **profil** : un fournisseur, un modèle et une voix. Vous pouvez avoir plusieurs profils
(une voix douce pour le soir, une voix rapide pour les alertes, une écoute Voxtral…) et les utiliser dans vos scénarios.

## Fournisseurs

| Fournisseur | Voix | Écoute | Clé | Remarques |
|---|---|---|---|---|
| **Compatible OpenAI** | oui | oui | selon le serveur | 9router (voix edge-tts et google-tts **gratuites**), ai_proxy (voix et écoute Mistral Voxtral avec le compte Mistral d'ai_proxy), OpenAI (`gpt-4o-mini-tts`, consignes de ton), Groq (écoute Whisper, offre gratuite), OpenRouter (Fish Audio, Kokoro… ; l'écoute demande au moins 0,50 $ de crédit), serveur local (Kokoro-FastAPI, Speaches, Piper). |
| **ElevenLabs** | oui | oui (Scribe) | oui | Voix très naturelles. Bouton « Charger les voix ». La clé doit avoir la permission « speech_to_text » pour l'écoute. |
| **Mistral (Voxtral)** | oui | oui | oui | Voix Voxtral TTS (hébergée en Europe), écoute Voxtral. L'audio demande un abonnement Mistral payant (sinon « HTTP 402 »). |
| **Gemini TTS** | oui | — | oui | 30 voix prédéfinies, style libre (« chaleureux », « chuchoté »…). |
| **Cartesia (Sonic)** | oui | — | oui | La latence la plus basse. Voix françaises (dont accents canadien, belge, africain). |
| **Google Translate** | oui | — | non | Gratuit, sans compte, qualité moyenne. Pratique en secours. |

Les clés se saisissent **une fois** dans la configuration du plugin (une par fournisseur). Un profil peut avoir sa
propre clé (champ « Clé API » de l'équipement). Les clés ne quittent pas la box.

## Mise en route

1. **Plugins → Gestion des plugins → Voice Engine** : activez le plugin, saisissez les clés des fournisseurs que vous
   utilisez, enregistrez, puis cliquez sur « Tester » à côté de chaque clé.
2. **Plugins → Multimédia → Voice Engine → Ajouter** : nommez le profil, choisissez le fournisseur, le modèle et la voix
   (le bouton « Charger les voix » liste celles de votre compte), puis enregistrez.
3. **Tester** : écoutez le profil directement dans le navigateur, et essayez l'écoute par un aller-retour (la phrase
   est lue par une voix, puis transcrite).

## Commandes

| Commande | Rôle |
|---|---|
| **Dire** | Lit le message sur la sortie du profil (voir plus bas). |
| **Créer le fichier** | Fabrique le son sans le jouer. « Dernier fichier » donne son chemin sur la box, « Dernière URL » une adresse pour un autre appareil. |
| **Lire un son** | Joue un fichier audio existant (chemin dans le dossier de Jeedom ou dans /tmp) ou une URL. |
| **Transcrire** | Transcrit un fichier audio (chemin dans le message) ; le texte arrive dans « Dernière transcription ». |
| Dernier texte lu, Dernier fichier, Dernière URL | Résultat du dernier son. |
| Caractères du jour | Caractères synthétisés aujourd'hui avec ce profil (les sons déjà en cache ne comptent pas). |
| Dernière transcription | Texte de la dernière écoute. |
| Durée du dernier appel, Dernière erreur | Suivi (« Dernière erreur » est vide quand tout va bien). |

Les commandes de voix n'existent que si « Ce profil parle » est coché, celles d'écoute si « Ce profil transcrit » l'est.

## Sortie de « Dire »

- **Aucune** : le son est seulement fabriqué (fichier et URL).
- **Haut-parleur de la box** : lecture par `ffplay`. Les lectures s'enchaînent sans se chevaucher.
- **Commande Jeedom** : le son est envoyé à une commande action d'un autre plugin (Chromecast, Sonos, satellite
  vocal, navigateur kiosque…). Le « Contenu envoyé » accepte `#url#` (adresse du son), `#text#` (texte lu) et
  `#file#` (chemin local).

Dans un scénario, le champ **Titre** de « Dire » ou « Lire un son » peut choisir une autre sortie que celle du profil :
`none`, `local` ou `cmd`.

## Liens vers les sons

Les sons sont servis par une **URL signée** et **limitée dans le temps** (une heure par défaut) : un Chromecast ou un
satellite peut la lire sans compte Jeedom, mais personne ne peut deviner l'adresse d'un autre son. L'adresse donnée aux
appareils est l'accès interne de Jeedom (réglable : accès externe).

## Moteur TTS de Jeedom

Voice Engine peut remplacer le moteur TTS de Jeedom (Pico, eSpeak) : **Réglages → Système → Configuration → Moteur TTS
→ Plugin Voice Engine**. Tous les plugins qui passent par la synthèse vocale de Jeedom utilisent alors le **profil de
voix par défaut** (configuration du plugin).

## Plafond et secours

- **Plafond par jour** : nombre de caractères au plus par jour et par profil (les fournisseurs payants facturent au
  caractère). Au-delà, le profil refuse de parler jusqu'au lendemain.
- **Profil de secours** : utilisé si le fournisseur échoue (réseau, quota, plafond). Exemple : ElevenLabs avec Google
  Translate en secours.

## Cache

Chaque son est gardé : une même phrase, avec la même voix, n'est fabriquée (et facturée) qu'une fois. Le cache est
limité en taille et en âge (configuration du plugin) et nettoyé chaque nuit. Le bouton « Vider » le supprime entièrement.

## Santé

La fenêtre **Santé** vérifie ffmpeg et ffplay, le cache, le moteur TTS de Jeedom, les profils par défaut, les clés, et
affiche pour chaque profil ses caractères du jour et sa dernière erreur (mis à jour en direct).

## Formats

Les sons sont en MP3. Les autres formats (WAV 16 kHz, FLAC 48 kHz, OGG) sont produits par ffmpeg à la demande
(modale Tester, et plus tard les autres plugins). Sans ffmpeg, un son reste dans le format du fournisseur (WAV pour Gemini).
