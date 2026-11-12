---
layout: default
title: Documentation JeeDash
lang: fr_FR
pluginId: jeedash
---

# JeeDash

#### 1 - Description
JeeDash est un **tableau de bord temps réel** pour Jeedom, servi comme une **application web autonome** à l'adresse `/plugins/jeedash/app/` de votre Jeedom :
 - une grille de **widgets** (31 types : lumières, volets, thermostats, capteurs, caméras, médias, énergie, météo, alarmes…) organisée en **onglets** ;
 - des valeurs mises à jour **en direct**, sans recharger la page ;
 - **7 thèmes** (Dark, Light, Minimalist, Mushroom, Nord, Solarized, Aurora), un mode compact et un mode plein écran ;
 - une application **installable** sur l'écran d'accueil d'un téléphone ou d'une tablette ;
 - l'accès se fait avec **votre compte Jeedom** et respecte ses droits.

L'application s'ouvre en dehors de l'interface Jeedom : elle est plus légère et plus rapide qu'une page du core. Le bouton **Jeedom** de la barre latérale ramène à Jeedom.

#### 2 - Prérequis
 - Jeedom 4.5 ou supérieur, Debian 12 ou supérieur.
 - Serveur web **Apache** (installation Jeedom standard).
 - Un navigateur récent (Chrome, Edge, Firefox, Safari).

#### 3 - Installation et configuration
1. Installer et activer le plugin.
2. Le **démon** démarre automatiquement (son état est visible sur la page du plugin).
3. Ouvrir le tableau de bord : menu **Accueil > JeeDash** (si l'affichage du panel est activé sur la page du plugin), ou directement `http://<votre-jeedom>/plugins/jeedash/app/` (la barre `/` finale est nécessaire).

Configuration du plugin :

| Paramètre | Défaut | Rôle |
|---|---|---|
| Thème par défaut | Sombre | Thème au premier affichage (ensuite, chaque tableau de bord garde le sien) |
| Port socket | `55009` | Communication interne entre Jeedom et le démon |
| Port SSE | `55010` | Flux temps réel vers le navigateur |

Après un changement de port, le démon redémarre de lui-même et le serveur web est réaligné : rien d'autre à faire. Ne changer ces ports qu'en cas de conflit avec un autre service.

##### Ce que le plugin modifie sur le système
Pour que le flux temps réel passe par le port web habituel de Jeedom (80/443) au lieu d'ouvrir un port supplémentaire, le plugin ajoute **un relais Apache** :
 - fichier `/etc/apache2/conf-available/jeedash.conf` (une règle `ProxyPass` vers `127.0.0.1:<port SSE>`), activé avec `a2enconf` ;
 - modules Apache `proxy` et `proxy_http` activés ;
 - Apache est rechargé seulement si la configuration change, et après un test de la configuration (en cas d'échec, l'ancienne configuration est remise).

Ce relais est écrit à l'installation, à la mise à jour et au changement du port SSE. Il est **retiré à la désinstallation** du plugin. Le démon n'écoute que sur la machine elle-même (`127.0.0.1`).

#### 4 - Utilisation
##### Barre latérale
La barre latérale (à gauche sur grand écran, en haut sur téléphone) contient l'horloge, le voyant de connexion temps réel, la liste des **onglets** et les actions :
 - **Éditer** (ou touche `E`) : passe en mode édition ;
 - **Thème**, **Compact**, **Plein écran** ;
 - **Export** / **Import** : sauvegarde et restauration du tableau de bord (fichier JSON) ;
 - **Jeedom** : retour à l'interface Jeedom.

Sur grand écran, la barre latérale se replie en une colonne d'icônes.

##### Voyant de connexion
 - **Couleur du thème** : valeurs en direct.
 - **Orange clignotant** : reconnexion en cours (automatique).
 - **Rouge** : temps réel indisponible.

##### Mode édition (administrateurs)
 - **Ajouter** : choisir le type de widget, l'équipement puis les commandes. Un aperçu s'affiche en direct.
 - Cliquer un widget pour le sélectionner : barre d'outils **Configurer**, **Copier**, **L+ / L-**, **H+ / H-** (taille), **→ Tab** (déplacer vers un autre onglet), **Suppr.**
 - Glisser-déposer pour réorganiser, poignée en bas à droite pour redimensionner.
 - Options d'un widget : **badges** (petites infos ou actions sur le widget), **condition d'affichage** (le widget n'apparaît que si une commande info vérifie une condition), **actions au toucher** (simple appui, double appui, appui long : action par défaut, panneau d'informations, bascule ou rien).
 - Onglets : jusqu'à 20, avec un nom et une icône (emoji).
 - **Sauver** enregistre ; **Annuler** abandonne les modifications.
 - Des modèles prêts à l'emploi (tableau de bord complet ou groupe de widgets) peuvent être appliqués.

##### Tout allumer / Tout éteindre
Au-dessus de la grille, les boutons **Tout allumer** et **Tout éteindre** agissent sur les lumières et interrupteurs de **l'onglet affiché**. La liste des équipements concernés est affichée avant confirmation. Les équipements déjà dans l'état voulu ou indisponibles sont ignorés, et une commande protégée par un code ou une confirmation Jeedom est **sautée**, jamais forcée.

##### Équipements en défaut
Un widget dont l'équipement est en alerte dans Jeedom (pile faible, absence de communication, alerte danger ou warning) porte une **étiquette**. Le compteur de la barre latérale indique leur nombre ; un clic amène au prochain widget en défaut, même sur un autre onglet.

##### Widgets orphelins
Si une commande ou un équipement utilisé par un widget est **supprimé dans Jeedom**, le widget est signalé en mode édition, avec deux boutons : **Remplacer** (ouvre l'éditeur) et **Supprimer**. Le compteur de la barre latérale amène au prochain widget orphelin.

##### Caméras
L'image d'une caméra est récupérée par Jeedom puis transmise au navigateur : l'adresse de la caméra (et ses identifiants éventuels) n'est jamais envoyée au navigateur.

##### Installer l'application sur un téléphone
Ouvrir `/plugins/jeedash/app/` dans le navigateur du téléphone, puis **Ajouter à l'écran d'accueil** (Safari : menu Partager ; Chrome : menu ⋮). L'application s'ouvre ensuite en plein écran, comme une application native.

#### 5 - Accès et droits
 - L'application demande une **connexion Jeedom**. Sans session, vous êtes redirigé vers la page de connexion de Jeedom, puis ramené au tableau de bord.
 - **Administrateur** : lecture, actions et édition du tableau de bord.
 - **Utilisateur** : lecture et actions, sans édition.
 - **Utilisateur limité** : seuls les équipements autorisés en lecture dans son profil Jeedom affichent leurs valeurs ; les autres widgets indiquent « Accès non autorisé ». Les actions demandent le droit d'exécution sur l'équipement.
 - Les **protections des commandes Jeedom** sont respectées : une commande qui demande une **confirmation** ou un **code d'accès** les demande aussi dans JeeDash.
 - Seules les commandes présentes dans le tableau de bord peuvent être lues ou actionnées, et le nombre d'actions par seconde est limité.

#### 6 - Accès à distance
 - **Par le DNS Jeedom (`*.jeedom.link`)** : l'application, la connexion et les actions fonctionnent, mais le **temps réel ne passe pas** par ce relais (le voyant reste déconnecté) : recharger la page pour actualiser les valeurs.
 - **Par un VPN** (Tailscale, WireGuard…) ou un accès direct à votre réseau : tout fonctionne, temps réel compris.

#### 7 - Dépannage
##### Le voyant reste rouge
 - Vérifier que le démon est démarré (page du plugin).
 - En accès par le DNS Jeedom : comportement connu, voir la section 6.
 - Consulter les logs `jeedash` et `jeedash_daemon`.

##### « Forbidden » en ouvrant l'application
L'adresse doit se terminer par une barre : `/plugins/jeedash/app/` (sans la barre finale, Jeedom refuse l'accès).

##### Le démon ne démarre pas
 - Vérifier que les ports configurés ne sont pas utilisés par un autre service.
 - Consulter les logs `jeedash_daemon`.

##### Un widget affiche « Commande supprimée dans Jeedom »
La commande a été supprimée ou recréée (inclusion, mise à jour d'un plugin) : passer en mode édition et utiliser **Remplacer**.

#### 8 - Changelog
Voir le [changelog](changelog).
