---
layout: default
title: Wyoming Bridge — Commande vocale Jeedom (Wyoming)
lang: fr_FR
pluginId: wyoming_bridge
---

# Wyoming Bridge

Le plugin **Wyoming Bridge** permet de piloter Jeedom à la voix depuis un pipeline vocal [Wyoming](https://github.com/rhasspy/wyoming) (satellites, Whisper, Piper).

Il expose un service Wyoming de type **handle** (« gestion des intentions ») : le pipeline lui envoie le texte transcrit, le plugin le transmet au plugin **AI Assistant**, qui tente d'abord une interaction Jeedom (si l'option est activée, voir ci-dessous) puis, à défaut, son LLM avec les outils MCP de Jeedom. La réponse texte revient au pipeline, qui la fait dire par Piper.

Le plugin ne remplace rien dans le pipeline vocal existant : il s'y ajoute comme un service Wyoming de plus.

## Prérequis

- Jeedom 4.5 ou supérieur, Debian 11 ou supérieur.
- Le plugin **AI Assistant**, actif et à jour, avec un **client MCP par défaut** configuré.
- Un pipeline vocal Wyoming (par exemple Home Assistant Assist, ou un orchestrateur Wyoming) capable d'utiliser un service « handle ».

## Installation

1. Installer et activer le plugin.
2. Lancer l'installation des **dépendances** (environnement Python avec les paquets `wyoming` et `zeroconf`).
3. Renseigner la configuration (ci-dessous), en particulier **IPs autorisées**.
4. Le démon démarre automatiquement (gestion automatique du démon dans la page de configuration).

## Configuration du plugin

| Paramètre | Rôle | Par défaut |
|---|---|---|
| Port TCP | Port du service Wyoming | 10500 |
| Bind | Adresse d'écoute ; `0.0.0.0` = toutes les interfaces | 0.0.0.0 |
| Timeout (s) | Délai maximal laissé à AI Assistant pour répondre (5 à 120 s) | 20 |
| Message d'échec | Phrase dite par Piper quand aucune réponse n'a pu être produite | « Désolé, je n'ai pas pu répondre. » |
| Consigne vocale | Consigne donnée à l'IA pour chaque phrase (forme de la réponse orale) ; vide = aucune | réponse en une à trois phrases courtes, sans mise en forme |
| Découverte Home Assistant | Annonce le service sur le réseau local pour que Home Assistant le détecte tout seul | oui |
| Toutes les langues | Propose le service dans un pipeline vocal de n'importe quelle langue ; l'IA répond dans la langue de la demande | non |
| IPs autorisées | IPs ou réseaux (CIDR) autorisés à se connecter, séparés par des virgules | — |
| Token interne | Jeton qui protège l'échange entre le démon et Jeedom, masqué (l'œil l'affiche) ; le bouton rouge le régénère. Inutile de le copier | généré |

**IPs autorisées est obligatoire** dès que l'adresse d'écoute n'est pas `127.0.0.1` : le protocole Wyoming n'a pas d'authentification, cette liste est la seule protection. Sans elle, le démon refuse de démarrer.

Exemple : `192.168.1.0/24` pour tout le réseau local, ou `192.168.1.50, 192.168.1.51` pour deux appareils précis.

## Réponses rapides sans LLM

AI Assistant peut essayer les **interactions Jeedom** avant d'appeler le LLM : une phrase reconnue par une interaction reçoit sa réponse en quelques millisecondes, sans consommer de jetons. Cette option est **désactivée par défaut** : cochez **Essayer interactions Jeedom d'abord** sur l'équipement AI Assistant utilisé comme client MCP par défaut. La fenêtre **Santé** indique si elle est activée.

## Raccorder le pipeline vocal

**Home Assistant** détecte le service tout seul (option **Découverte Home Assistant**) : il apparaît dans **Paramètres > Appareils et services** comme appareil découvert, à valider. L'adresse de Home Assistant doit figurer dans **IPs autorisées**, et l'adresse d'écoute doit être `0.0.0.0` ou l'IP de la box. Une entrée Wyoming déjà ajoutée à la main est reconnue, pas dupliquée.

Sinon, ou avec un autre orchestrateur, déclarer un service Wyoming à l'adresse `IP_de_Jeedom:10500`, de type **handle** / gestion des intentions. Le service s'annonce sous le nom `jeedom`, avec la langue de Jeedom, ou pour toutes les langues si l'option **Toutes les langues** est cochée (redémarrer le démon après l'avoir changée).

## Page du plugin

- **Configuration** : ouvre la configuration du plugin.
- **Tester** : envoie une phrase au démon par le protocole Wyoming, exactement comme un satellite. La fenêtre affiche l'état du démon, la réponse, si elle a été traitée, la durée et le profil AI Assistant utilisé. Les actions demandées sont réellement exécutées et chaque test peut consommer un appel au LLM.
- **Santé** : dépendances, démon, adresse d'écoute, IPs autorisées, délai, versions (Python, paquet `wyoming` installé et dernière version disponible, librairies d'installation), découverte Home Assistant, AI Assistant, client MCP par défaut et option des interactions Jeedom.
- **Équipement Wyoming Bridge** : créé automatiquement, avec ces commandes info, utilisables dans les scénarios :

| Commande | Contenu | Historisée par défaut |
|---|---|---|
| État du démon | 1 = actif (vérifié toutes les 5 minutes) | non |
| Dernière phrase | Texte reçu du satellite | non |
| Dernière réponse | Texte renvoyé (vide si non traitée) | non |
| Dernière requête traitée | 1 = réponse obtenue, 0 = échec | oui |
| Durée de la dernière requête | En millisecondes, côté Jeedom | oui |
| Dernier satellite | Satellite Home Assistant (ex. `assist_satellite.salon`), sinon adresse IP, ou `local` | non |

Les commandes sont mises à jour après l'envoi de la réponse : elles n'ajoutent aucun délai. La dernière phrase et la dernière réponse sont visibles par les utilisateurs de Jeedom ; ne pas historiser ces deux commandes si l'on ne veut pas conserver les conversations.

## Fonctionnement

- Chaque satellite a sa propre limite de débit et son propre historique de conversation dans AI Assistant. Home Assistant indique quel satellite parle : chacun garde son propre historique même si Home Assistant les relaie tous. Sans cette information (autre orchestrateur), le satellite est identifié par son adresse IP.
- **Fil de conversation** : à chaque nouvelle conversation Home Assistant, l'historique du satellite repart de zéro, et l'IA ne mélange pas la demande avec les précédentes. Dans une même conversation (« Et dehors ? » juste après « Quelle température au salon ? »), le fil est gardé.
- Quand la réponse se termine par une question, le satellite réécoute aussitôt, sans mot d'activation.
- La langue de la demande est transmise : si elle diffère de celle de Jeedom, l'IA répond dans cette langue.
- AI Assistant accepte une requête au LLM toutes les 2 secondes par appelant : une phrase envoyée trop tôt reçoit « Un instant, réessayez dans quelques secondes. » Les phrases reconnues par une interaction Jeedom ne sont jamais limitées.
- La réponse est nettoyée avant d'être renvoyée : mise en forme (gras, titres, listes, tableaux) et emojis retirés, pour que Piper la lise naturellement.
- En cas de délai dépassé, d'erreur ou d'AI Assistant indisponible, le pipeline reçoit « non traité » avec le message d'échec.
- Les phrases et les réponses ne sont écrites dans les logs qu'en niveau Debug.
- Une connexion sans activité pendant 10 minutes est fermée, et une même adresse IP ne peut pas ouvrir plus de 8 connexions simultanées.

## Dépannage

| Symptôme | Vérification |
|---|---|
| Le démon ne démarre pas | Fenêtre Santé : IPs autorisées vide alors que Bind n'est pas `127.0.0.1`, ou dépendances à installer |
| Le satellite n'obtient jamais de réponse | IP du satellite (ou de l'orchestrateur) absente de IPs autorisées ; log `wyoming_bridge_daemon` en niveau Warning (un message par heure et par IP refusée) |
| Réponse « Désolé… » | Délai trop court pour le LLM, ou pas de client MCP par défaut dans AI Assistant |
| Réponse « Un instant, réessayez… » | Deux phrases à moins de 2 secondes d'intervalle pour le même satellite (ou le même orchestrateur) |
| Toutes les phrases passent par le LLM, même les plus simples | Option des interactions Jeedom désactivée dans AI Assistant (voir la fenêtre Santé) |
| Le bouton Tester indique « IP refusée » | Ajouter `127.0.0.1` ou l'IP de Jeedom dans IPs autorisées |

Logs : `wyoming_bridge` (partie PHP) et `wyoming_bridge_daemon` (démon Python).
