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

Les réglages d'écoute et de réponse sont pris en compte sans redémarrage. Les réglages réseau (ports, adresses) demandent de redémarrer le démon.

Sur chaque satellite, **Fin de parole**, **Écoute maximale** et **Relancer l'écoute après une question** remplacent le réglage du plugin (vide = réglage du plugin). Une pièce bruyante demande souvent une fin de parole plus longue.

## Pièce du satellite

L'objet parent du satellite est donné à AI Assistant comme pièce où l'on parle : « allume la lumière » vise la lumière de cette pièce, sans la nommer.

## Commandes

| Commande | Type | Rôle |
|---|---|---|
| Connecté | info binaire | connexion du démon au satellite |
| État | info texte | `idle`, `listening`, `thinking`, `speaking` |
| Dernière phrase / Dernière réponse | info texte | dernier échange |
| Durée de la dernière réponse | info numérique (ms) | de la fin de la phrase à l'envoi de la voix |
| Dernière erreur | info texte | transcription, réponse, voix ou connexion en échec (déclenche un scénario à chaque erreur, même répétée) |
| Volume / Muet | info | état du lecteur du satellite |
| Mot d'appel | info texte | mot d'appel actif |
| Minuteur en cours / Dernier minuteur terminé | info | suivi des minuteurs |
| Annoncer | action message | fait dire un texte au satellite |
| Annoncer et écouter | action message | dit le texte, puis écoute la réponse (sans mot d'appel) |
| Arrêter la lecture | action | interrompt la réponse ou l'annonce en cours |
| Régler le volume | action curseur (0-100) | volume du satellite |
| Couper le son / Remettre le son | action | muet |
| Choisir le mot d'appel | action liste | liste lue sur le satellite |
| Lancer un minuteur | action message | titre = nom (facultatif), message = durée (`5 min`, `1h30`, `90 s` ; un nombre seul = secondes) |
| Annuler les minuteurs | action | annule les minuteurs du satellite |

## Minuteurs à la voix

Exemples : « Mets un minuteur de 10 minutes », « Combien de temps reste-t-il sur le minuteur ? », « Annule le minuteur ». À « Mets un minuteur », le satellite demande « Pour combien de temps ? » et attend la durée.

Ces phrases sont traitées par le plugin, sans appel au LLM. Le satellite affiche le décompte et sonne à la fin, s'il gère les minuteurs.

Sur `linux-voice-assistant`, la sonnerie d'un minuteur s'arrête avec le mot d'appel, « Stop » ou le bouton. « Arrêter la lecture » ne coupe que le son en cours : LVA relance la sonnerie.

Les minuteurs en cours sont conservés au redémarrage du démon ou de la box. Un minuteur échu pendant l'arrêt ne sonne pas, mais met à jour « Dernier minuteur terminé ».

« Annule » et « reste » ne visent les minuteurs du satellite que si l'un d'eux est en cours. Sinon, la phrase part vers Jeedom (par exemple pour un équipement nommé « minuteur »).

## Mot d'appel

Le choix se limite aux mots d'appel que le satellite connaît. Les modèles officiels ESPHome sont en anglais. Un mot en français demande un modèle entraîné :
- sur un ESP32, il faut recompiler le firmware ;
- avec `linux-voice-assistant` (LVA, sur un Pi ou un PC), il suffit d'ajouter un modèle openWakeWord, sans toucher au plugin.

### Mot d'appel en français avec linux-voice-assistant

1. **Modèle** : un modèle openWakeWord au format **TFLite** (`.tflite`). LVA ne lit pas l'ONNX. Un modèle issu de l'entraînement openWakeWord est en ONNX : convertissez-le en gardant l'entrée `[1, 16, 96]` (`onnx2tf` la transpose en `[1, 96, 16]` : vérifiez la forme obtenue).
2. **Description** : un fichier JSON à côté du modèle, par exemple `frida.json` :
   ```json
   {
     "type": "openWakeWord",
     "wake_word": "Frida",
     "model": "frida.tflite",
     "trained_languages": ["fr"],
     "openWakeWord": { "probability_cutoff": 0.8 }
   }
   ```
3. **Dossier** : montez le dossier dans le conteneur et indiquez-le à LVA. Les mots d'appel intégrés restent disponibles. Montez aussi `/app/configuration` : LVA y garde le mot choisi.
   ```yaml
   environment:
     WAKE_WORD_DIR: /app/wakewords-custom
   volumes:
     - /chemin/vers/wakewords:/app/wakewords-custom:ro
     - /chemin/vers/configuration:/app/configuration
   ```
4. **Choix** : redémarrez LVA. Le mot apparaît dans la commande **Choisir le mot d'appel** du satellite, choisissez-le. Il reste actif après un redémarrage de LVA.

**Seuil** (`probability_cutoff`) : 0,8 est conseillé (LVA prend 0,7 si le JSON n'en donne pas). Mesuré pour « Frida » : environ 88 % de détection et moins d'une fausse détection par heure. Plus haut, il y a moins de fausses détections, mais davantage de mots manqués. L'entité « Wake Word Sensitivity » de LVA, si elle est réglée, remplace cette valeur.

**À l'usage** : LVA n'écoute qu'après son bip, environ 2 s après le mot d'appel. Attendez le bip avant de poser la question.

**Mot « Stop » de LVA** : pendant une réponse ou la sonnerie d'un minuteur, LVA écoute aussi son mot « Stop », un modèle anglais. Mesuré sur des voix de synthèse françaises, il se déclenche souvent sur d'autres mots. Au réglage par défaut (0,5), il coupe la réponse sur 56 % des « Frida » et 24 % des phrases ordinaires, et ne reconnaît que 64 % des vrais « Stop ». Avec `"stop_word_sensitivity": 0.8` dans `/app/configuration/preferences.json` (à modifier LVA arrêté), ces arrêts intempestifs tombent à 25 % et 11 %, et 56 % des vrais « Stop » restent reconnus. Au-delà de 0,81, le mot « Stop » ne se déclenche plus.

## Voix

Le satellite parle dès la première phrase de la réponse, pendant que l'IA écrit encore la suite : chaque phrase est synthétisée dès qu'elle est complète (satellites en FLAC, le format par défaut ; client MCP d'AI Assistant sur Claude, Gemini ou un fournisseur OpenAI-compatible comme DeepSeek, Mistral, Groq ou OpenAI ; sinon la réponse arrive d'un bloc puis est dite phrase par phrase). Les phrases courtes déjà dites (réponses des minuteurs, message d'échec, annonces répétées) sont gardées en mémoire quelques heures et repartent sans nouvelle synthèse.

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
