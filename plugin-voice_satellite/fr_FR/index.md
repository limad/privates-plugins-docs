---
layout: default
title: Voice Satellite — Satellites vocaux ESPHome sans Home Assistant
lang: fr_FR
pluginId: voice_satellite
---

# Voice Satellite

Pilote des **satellites vocaux ESPHome** directement depuis Jeedom, **sans Home Assistant**.

Le satellite détecte le mot d'appel (« Okay Nabu », « Hey Jarvis »…). Jeedom reçoit ensuite le son du micro, repère la fin de la phrase, la transcrit, répond avec le plugin **AI Assistant** (interactions Jeedom d'abord, puis LLM) et fait parler le satellite.

## Matériel compatible

Tout appareil qui expose un assistant vocal par l'API native ESPHome :
- Home Assistant Voice PE ;
- ESP32-S3-BOX-3 ;
- Seeed ReSpeaker Lite ;
- M5Stack Atom Echo ;
- un Raspberry Pi ou un PC avec micro, via [linux-voice-assistant](https://github.com/OHF-Voice/linux-voice-assistant).

> Un satellite ne peut être piloté que par **un seul** assistant à la fois. Retirez-le d'abord de Home Assistant.

## Prérequis

- Le plugin **AI Assistant**, avec un client MCP par défaut et un **serveur vocal** configuré (écoute et voix).
- Les dépendances du plugin : Python ≥ 3.11 dans un environnement dédié, `aioesphomeapi`, `pysilero-vad` et `ffmpeg`. Sur Debian 11, Python 3.11 est compilé : l'installation peut alors durer longtemps.

## Ajouter un satellite

**Rechercher** (page du plugin) liste les appareils ESPHome annoncés sur le réseau local (mDNS). Pour chacun, la fenêtre indique s'il expose un assistant vocal et s'il est chiffré. **Ajouter** crée l'équipement.

Le satellite doit être sur le même réseau que Jeedom, et le multicast doit passer. Sinon, **Ajouter** permet de saisir son adresse à la main.

### Chiffrement

- **Satellite déjà chiffré** : copiez la clé `api: encryption: key` de son YAML ESPHome dans le champ « Clé de chiffrement ».
- **ESPHome 2026.7 ou plus**, avec `api: encryption:` sans clé dans le YAML : enregistrez l'équipement, puis cliquez sur **Générer**. Jeedom crée une clé aléatoire, la pose sur le satellite par une connexion déjà chiffrée, puis l'enregistre.
- **Satellite non chiffré** : laissez le champ vide.

La clé est stockée chiffrée. En cas de problème, la connexion affiche la cause : clé requise, clé refusée, ou satellite non chiffré.

## Configuration du plugin

- **Fin de parole** : durée de silence (ms) qui termine la phrase.
- **Écoute maximale** : durée d'écoute maximale après le mot d'appel.
- **Délai de réponse** : temps laissé à AI Assistant.
- **Relancer l'écoute après une question** : quand la réponse se termine par une question, le satellite écoute à nouveau sans mot d'appel.
- **Message d'échec** : phrase dite quand aucune réponse n'a pu être produite.
- **Port audio** et **adresse de la box** : adresse à laquelle les satellites téléchargent la voix.
- **Port de contrôle** : port local par lequel Jeedom commande le démon.

Redémarrez le démon après avoir modifié ces réglages.

## Commandes

| Commande | Type | Rôle |
|---|---|---|
| Connecté | info binaire | connexion du démon au satellite |
| État | info texte | `idle`, `listening`, `thinking`, `speaking` |
| Dernière phrase / Dernière réponse | info texte | dernier échange |
| Durée de la dernière réponse | info numérique (ms) | de la fin de la phrase à l'envoi de la voix |
| Volume / Muet | info | état du lecteur du satellite |
| Mot d'appel | info texte | mot d'appel actif |
| Minuteur en cours / Dernier minuteur terminé | info | suivi des minuteurs |
| Annoncer | action message | fait dire un texte au satellite |
| Annoncer et écouter | action message | dit le texte, puis écoute la réponse (sans mot d'appel) |
| Régler le volume | action curseur (0-100) | volume du satellite |
| Couper le son / Remettre le son | action | muet |
| Choisir le mot d'appel | action liste | liste lue sur le satellite |
| Lancer un minuteur | action message | titre = nom (facultatif), message = durée (`5 min`, `1h30`, `90 s` ; un nombre seul = secondes) |
| Annuler les minuteurs | action | annule les minuteurs du satellite |

## Minuteurs à la voix

Exemples : « Mets un minuteur de 10 minutes », « Combien de temps reste-t-il sur le minuteur ? », « Annule le minuteur ». À « Mets un minuteur », le satellite demande « Pour combien de temps ? » et attend la durée.

Ces phrases sont traitées par le plugin, sans appel au LLM. Le satellite affiche le décompte et sonne à la fin, s'il gère les minuteurs.

« Annule » et « reste » ne visent les minuteurs du satellite que si l'un d'eux est en cours. Sinon, la phrase part vers Jeedom (par exemple pour un équipement nommé « minuteur »).

## Mot d'appel

Le choix se limite aux mots d'appel que le satellite connaît. Les modèles officiels ESPHome sont en anglais. Un mot en français demande un modèle entraîné :
- sur un ESP32, il faut recompiler le firmware ;
- avec `linux-voice-assistant`, il suffit d'ajouter un modèle openWakeWord dans son dossier de mots d'appel.

## Santé

**Santé** (page du plugin) affiche, pour chaque satellite :
- la connexion et la cause d'une erreur ;
- le modèle et la version ESPHome ;
- le format de la voix ;
- les fonctions annoncées (minuteurs, annonces, relance d'écoute…) ;
- le mot d'appel, le volume et les minuteurs en cours.

## Sécurité

- Le démon se connecte aux satellites ; il n'ouvre pas l'API ESPHome.
- Les satellites téléchargent la voix en HTTP sur le port audio, à des adresses à jeton aléatoire valables 5 minutes.
- Le démon parle à Jeedom par `core/php/pipeline.php`, protégé par un jeton.
- Le port de contrôle n'écoute que sur 127.0.0.1 et exige le même jeton.
