---
layout: default
title: Changelog JeeDash
lang: fr_FR
pluginId: jeedash
---

# Changelog plugin JeeDash

>**IMPORTANT**
>
>Pour rappel s'il n'y a pas d'information sur la mise à jour, c'est que celle-ci concerne uniquement de la mise à jour de documentation, de traduction ou de texte.

# 13/11/2026

- Téléphone : un widget plus large que l'écran ne déborde plus et n'écrase plus ses voisins
- Le nombre de colonnes suit la place réellement disponible (barre latérale déduite) : tablettes et petits écrans moins serrés
- Téléphone : en-tête plus compact (actions en icônes), widget **Équipement** sur une colonne
- Écrans tactiles : boutons, interrupteurs et curseurs plus grands
- Textes des widgets plus lisibles (taille minimale relevée)
- Chargement du tableau de bord un peu plus léger
- Textes de l'interface avec leurs accents
- Graphique : indique « Commande non historisée » quand la commande n'a pas d'historique dans Jeedom

# 12/11/2026

- **Application autonome** : le tableau de bord s'ouvre à l'adresse `/plugins/jeedash/app/`, hors de l'interface Jeedom (plus léger et plus rapide), avec une **barre latérale** (horloge, onglets avec icône, actions) repliable sur grand écran. Le menu Jeedom y redirige
- **Installable sur téléphone et tablette** (ajout à l'écran d'accueil, ouverture en plein écran)
- **Connexion par compte Jeedom** : sans session, retour automatique au tableau de bord après la connexion. Les utilisateurs non administrateurs peuvent consulter et piloter, sans éditer
- **Droits Jeedom respectés** : un utilisateur limité ne voit que les valeurs des équipements autorisés dans son profil ; les commandes qui demandent une **confirmation** ou un **code d'accès** les demandent aussi dans JeeDash
- **Tout allumer / Tout éteindre** sur l'onglet affiché, avec la liste des équipements concernés avant confirmation
- **Équipements en défaut** (pile faible, absence de communication, alerte) signalés sur les widgets, avec un compteur dans la barre latérale
- **Widgets orphelins** : un widget dont la commande ou l'équipement a été supprimé dans Jeedom est signalé en mode édition, avec les boutons **Remplacer** et **Supprimer**
- **Caméras** : l'image passe par Jeedom, l'adresse de la caméra n'est plus visible dans le navigateur
- Changement du port SSE ou du port socket : le démon redémarre et le serveur web est réaligné automatiquement
- Temps réel : seules les commandes affichées sont suivies (moins de charge)
- **Accès par le DNS Jeedom** : le temps réel n'y passe pas ; l'application relit alors les valeurs de l'onglet affiché toutes les 10 secondes, et revient au temps réel dès qu'il est disponible (voyant orange fixe en attendant)
- Utilisateur limité : les widgets des équipements non autorisés ne sont plus envoyés à son navigateur (ni nom ni commande)
- Sécurité : la clé API n'est plus acceptée dans l'adresse, seulement dans un en-tête
- Jeedom en HTTPS ou sur un autre port que 80 : le contrôle de connexion du temps réel et la page de santé utilisent l'accès interne configuré dans Jeedom

# 06/11/2026

- Les filtres de commandes par type générique de l'éditeur de widgets fonctionnent de nouveau

# 15/10/2026

- Serrure, garage et alarme : la confirmation d'action utilise la fenêtre de Jeedom

# 14/10/2026

- Temps réel plus fiable : le flux passe par un relais du serveur web au lieu d'une règle de réécriture
- Nouvelle icône du plugin

# 21/06/2026

- **Mode édition repensé** : panneau latéral (en bas sur téléphone), choix du type de widget par icônes, aperçu en direct, commandes optionnelles
- Widget **Équipement** : les paires d'actions On/Off sont regroupées en un seul interrupteur

# 07/06/2026

- Widget **Équipement** : affichage de toutes les commandes d'un équipement, nombre de colonnes réglable, curseurs et listes restylés
- Panneau d'informations détaillées plus utile et plus compact
- Fenêtres de saisie intégrées à la place des fenêtres du navigateur

# 03/06/2026

- **Édition** : modifications annulables, indicateur de modifications non enregistrées, barre d'outils flottante (configurer, copier, taille, déplacer, supprimer)
- Onglets synchronisés avec l'adresse de la page
- **Badges**, **mini-graphiques**, **condition d'affichage** et **actions au toucher** (simple, double, appui long) sur les widgets
- Type de widget suggéré selon la commande, widget **Séparateur**
- Thème **Aurora**, thermostat redessiné
- État optimiste des widgets après une action, voyant de connexion temps réel

# 02/06/2026

- **Modèles** de tableaux de bord et de groupes de widgets, **thèmes** (Dark, Light, Minimalist, Mushroom, Nord, Solarized)
- Modes **compact** et **plein écran**
- Commande indisponible signalée sur le widget, caméra désactivée signalée

# 01/06/2026

- Nouveaux widgets : lumière couleur et température de couleur, caméra, scène, sélecteur, média, électricité, météo, alarme, ventilateur, garage, message, dialogue, texte, minuteur, hygrostat, batterie, qualité d'air, présence, santé système

# 31/05/2026

- Première version : tableau de bord temps réel, éditeur par glisser-déposer et redimensionnement, export / import, interface français / anglais
