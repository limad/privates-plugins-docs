---
layout: default
title: Kiosk Browser — Changelog
lang: fr_FR
pluginId: kiosk_browser
---

# Changelog Kiosk Browser

## Version en cours de développement

### 09/11/2026
- **Nouveautés et corrections** : plusieurs écrans ne se disputent plus les mêmes ports (un conflit empêchait d'enregistrer un écran, voire de mettre à jour le plugin) ; installation des dépendances plus fiable (un échec de l'installation de Chromium est désormais signalé, la page des dépendances ne reste plus bloquée « en cours ») ; le niveau de log choisi est respecté ; les noms des commandes peuvent être traduits ; pack de widgets installé une seule fois et vérifié avant installation.
- **Sécurité et fiabilité** : seul Jeedom peut piloter les écrans (une page web affichée sur la box ne le peut plus) ; navigateur isolé du système quand c'est possible ; la clé API n'apparaît plus dans les journaux du serveur web ; une connexion ouverte par un autre plugin via le relais n'est plus coupée par un relais lancé à la main.

### 29/10/2026
- **Nouveautés** : tuiles « Documentation » et « Community » sur la page du plugin.

### 28/10/2026
- **Nouveautés et corrections** : Chromium redémarre tout seul après un plantage et revient sur la dernière page (3 fois au plus en 10 minutes) ; un écran désactivé arrête son navigateur ; le nom et l'ordre des commandes sont conservés lors des mises à jour ; clics et défilement plus fiables dans la fenêtre d'écran en direct.
- **Sécurité et fiabilité** : plus de double démarrage d'un même écran ; les sessions de connexion du relais (cookies) sont effacées à l'arrêt ; fermer l'écran en direct libère aussitôt les ressources de la box.

### 23/10/2026
- **Nouveautés** : interface traduite dans les autres langues de Jeedom.

### 22/10/2026
- **Corrections** : le champ « Texte à saisir » ne déclenche plus le gestionnaire de mots de passe du navigateur.

### 16/10/2026
- **Corrections** : journaux du plugin au format Jeedom.

### 15/10/2026
- **Nouveautés** : affichage des commandes avec les widgets iso.

### 14/10/2026
- **Corrections** : arrêter un écran ne relance plus les autres.
- **Sécurité et fiabilité** : navigation limitée aux adresses web (http/https) ; les captures d'écran et la clé API n'apparaissent plus dans les journaux.

### 03/10/2026
- Première version : pilotage d'écrans Chromium en plein écran et relais OAuth.
