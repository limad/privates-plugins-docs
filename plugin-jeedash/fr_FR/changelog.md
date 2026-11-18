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

# 18/11/2026

- **Thèmes clairs et sombres** : 8 thèmes (JeeDash, Delprat, Ghome, Minimalist, Mushroom, Nord, Solarized, Aurora), chacun en version claire et sombre ; Aurora passe du violet au bleu cyan. Le mode Auto (suit le téléphone), Clair ou Sombre se règle par appareil ; le thème choisi suit votre compte sur tous vos appareils. Les anciens thèmes Dark et Light deviennent le thème JeeDash
- **Page Réglages** (bouton Réglages de la barre latérale) : thèmes, taille de l'interface, police, cartes Complètes / Compactes / Mini, arrondi des cartes, taille de l'horloge, plein écran
- **Onglet d'accueil** : chaque utilisateur choisit l'onglet affiché à l'ouverture, sur tous ses appareils
- Les boutons Thème, Compact et Plein écran de la barre latérale passent dans les Réglages
- **Sécurité** : les actions d'administration (logs, démons, messages, partages) sont réservées à un administrateur connecté ; le Chat n'ouvre que l'envoi et la réponse des assistants partagés, un compte à la fois par assistant (un autre compte est invité à patienter) ; un profil restreint ne voit plus les messages de Jeedom
- Réponses du serveur jusqu'à 6 fois plus légères, et une requête de moins à l'ouverture pour les tuiles équipement
- **Menu ⋮ sur chaque tuile** : Détails, Caméra en grand, Masquer (pour votre compte, réaffichage dans Réglages > Éléments masqués) et, pour un administrateur, Modifier, Dupliquer, Déplacer vers un onglet, Supprimer. L'en-tête rappelle le plugin de l'équipement
- Thermostat : chiffres du cadran en blocs, et la température mesurée n'est plus minuscule en cartes compactes
- **Chat IA** : écrire à un assistant du plugin ai_assistant et lire sa réponse, avec dictée vocale ; un administrateur choisit les assistants partagés (Réglages > Chat IA)
- **Agendas** : plages du plugin Agenda, en cours et prochaines ; un administrateur règle heures, jours et jours fériés, ou coupe un agenda (bouton à afficher dans Réglages > Général)
- **Éditeur de thèmes** : créer un thème (versions claire et sombre, arrondi), le modifier avec aperçu en direct et contrôle des contrastes, l'exporter ou l'importer ; un administrateur peut le partager à tous les comptes
- **Page Alertes** (barre latérale, avec un compteur) : appareils en défaut, messages Jeedom regroupés (à traiter, erreurs, informations), et pour un administrateur les erreurs des logs des dernières 24 h et les mises à jour en attente
- **Page Analyse** : piles de tous les équipements (seuils de Jeedom), et pour un administrateur la santé de Jeedom et les démons des plugins, avec Démarrer / Relancer
- **Diagnostic** (Réglages) : mesures de l'appareil (écran, vitesse, temps réel) et rapport à copier
- **Session expirée** : un panneau propose de se reconnecter au lieu d'erreurs en série
- **Info-bulles** sur les boutons, au survol de la souris ou au clavier
- **Icône du plugin** de chaque équipement (en-tête des tuiles équipement, menu ⋮, Alertes, Analyse)
- **Tuile scénario** : lance ou arrête un scénario Jeedom, affiche son état et son dernier lancement
- **Couleur des lumières** : le rond de couleur ouvre une liste de 8 couleurs, 3 blancs et « Autre couleur… »
- **Envoi des commandes** : un petit anneau tourne sur la tuile pendant l'envoi ; en cas d'échec, la tuile tremble, un message explique l'erreur et l'état précédent revient
- **Interrupteurs** (Réglages > Cartes > Marche / arrêt) : lumières en interrupteur à bascule à la place des boutons On / Off
- **Thermostat** : pastille « Chauffe » quand le chauffage est en marche (commande facultative)
- Un simple clic sur une tuile n'ouvre plus la fiche de l'équipement : elle s'ouvre par ⋮ > Détails ou par un appui long

# 16/11/2026

- Icône du plugin allégée, le menu Plugins de Jeedom s'affiche plus vite (rendu identique).

# 14/11/2026

- **Temps réel sans le démon** : nouveau réglage **Temps réel** (Auto / Long-polling seul). En long-polling, les valeurs arrivent en direct par le serveur web de Jeedom, à une seconde près
- **Accès par le DNS Jeedom** et démon arrêté : les valeurs restent en direct (long-polling) au lieu d'être relues toutes les 10 secondes
- **Onglet depuis une pièce** : à la création d'un onglet, il peut être rempli automatiquement avec les équipements d'une pièce Jeedom
- **Fiche d'équipement enrichie** : courbe sur 24 h ou 7 jours, infos en direct, actions utilisables selon leur type (curseur, liste, couleur, message), défaut de l'équipement ; le bouton retour du téléphone la ferme
- Correction : depuis la fiche d'un équipement, ses autres commandes étaient refusées
- **Ouverture plus rapide**, surtout à distance par le DNS Jeedom : le tableau de bord s'affiche en moins d'une seconde sur téléphone (jusqu'à 3 secondes auparavant)
- Accès par le DNS Jeedom : les valeurs passent en direct dès l'ouverture, au lieu d'attendre une dizaine de secondes

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
