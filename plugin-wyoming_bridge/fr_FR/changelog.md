---
layout: default
title: Wyoming Bridge — Changelog
lang: fr_FR
pluginId: wyoming_bridge
---

# Changelog Wyoming Bridge

## 2026-11-25

- Réponses dites plus naturellement : la mise en forme (gras, titres, listes, tableaux) et les emojis ajoutés par l'IA sont retirés avant la lecture par Piper.
- Phrase trop rapprochée de la précédente : le satellite dit « Un instant, réessayez dans quelques secondes. » au lieu du message d'échec.
- Fenêtre **Santé** : indique si AI Assistant essaie les interactions Jeedom avant le LLM (option désactivée par défaut, à cocher dans AI Assistant pour des réponses instantanées).
- Log du démon allégé : la vérification régulière faite par Jeedom n'y écrit plus d'avertissement, et une IP refusée n'est signalée qu'une fois par heure.
- Fiabilité : connexions inactives fermées après 10 minutes, 8 connexions simultanées au plus par adresse IP.
- Vérification des dépendances plus rapide.
- Token interne masqué dans la configuration (bouton œil pour l'afficher).
- **Fil de conversation** avec Home Assistant : chaque satellite garde son propre historique, remis à zéro à chaque nouvelle conversation et conservé pour les questions de suite.
- Quand la réponse se termine par une question, le satellite réécoute sans mot d'activation.
- Langue de la demande transmise à l'IA ; option **Toutes les langues** pour utiliser le service dans un pipeline vocal d'une autre langue.
- Option **Consigne vocale** : réponses courtes adaptées à la voix par défaut, modifiable.
- Nécessite une version à jour d'AI Assistant pour le fil de conversation et la consigne vocale.
- **Découverte automatique par Home Assistant** : le service s'annonce sur le réseau local et apparaît dans les appareils découverts (option activée par défaut ; relancer les dépendances).

## 2026-11-10

- Heartbeat du core : un démon actif est désormais vu comme communiquant (sinon, avec un heartbeat réglé, le core l'aurait déclaré muet).
- Commandes de suivi : dernière phrase, dernière réponse, traitée ou non, durée, dernier satellite (mises à jour après l'envoi de la réponse).
- Documentation et changelog, publiés sur le site de documentation.
- Paquet `wyoming` 1.11.0.

## 2026-11-09

- Page du plugin au format Jeedom : Configuration, Tester, Santé, équipement et commandes.
- Fenêtre **Tester** : envoi d'une phrase par le protocole Wyoming, comme un satellite.
- Fenêtre **Santé** : état général, versions installées et dernières versions disponibles.
- Valeurs par défaut affichées dans la configuration (port, bind, délai, message d'échec).
- Limite de débit et historique propres à chaque satellite.
- Version du service annoncée d'après le plugin.
- Délai transmis à AI Assistant : plus aucune action lancée après une réponse « non traité ».
- Message d'échec dit au satellite au lieu d'un silence ; langue annoncée = langue de Jeedom.
- Sécurité : liste IPs autorisées obligatoire hors `127.0.0.1`, jeton hors de la ligne de commande, lectures réseau bornées, arrêt du démon limité à ses propres processus.
- Démarrage automatique du démon confié au core ; librairies d'installation embarquées.

## 2026-10-13

- `handle.php` répond 401 quand le plugin est inactif.

## 2026-09-20

- Jeton chiffré en base, licence AGPL-3.0.

## 2026-09-08

- Première version.
