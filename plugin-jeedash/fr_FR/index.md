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
 - **8 thèmes** (JeeDash, Delprat, Ghome, Minimalist, Mushroom, Nord, Solarized, Aurora), chacun en version claire et sombre, et une page **Réglages** (taille, police, cartes compactes, horloge, plein écran…) ;
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
| Thème par défaut | JeeDash | Thème des comptes qui n'en ont pas choisi dans Réglages > Thèmes |
| Port socket | `55009` | Communication interne entre Jeedom et le démon |
| Port SSE | `55010` | Flux temps réel vers le navigateur |
| Temps réel | Auto | **Auto** : flux du démon (SSE), long-polling s'il ne passe pas. **Long-polling seul** : valeurs en direct par le serveur web de Jeedom, sans le démon ni son port |

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
 - **Réglages** : page des réglages (voir ci-dessous) ;
 - **Export** / **Import** : sauvegarde et restauration du tableau de bord (fichier JSON) ;
 - **Jeedom** : retour à l'interface Jeedom.

Sur grand écran, la barre latérale se replie en une colonne d'icônes.

##### Alertes, Analyse, Diagnostic
Les boutons **Alertes** et **Analyse** de la barre latérale ouvrent des pages qui remplacent la grille (adresses `#alerts` et `#analysis`) ; un clic sur un onglet y ramène.
 - **Alertes** : appareils en défaut (bouton **Voir** pour aller à leur tuile), messages Jeedom regroupés par texte semblable. Un administrateur peut les supprimer ; il voit aussi les erreurs des logs des dernières 24 h (bouton **Voir le log**) et les mises à jour en attente. Le compteur du bouton additionne appareils en défaut et messages à traiter ou en erreur.
 - **Analyse** : niveau des piles de tous les équipements (seuils de Jeedom). Un administrateur voit aussi la santé de Jeedom et les démons des plugins, qu'il peut démarrer ou relancer.
 - **Diagnostic** (Réglages > Diagnostic) : mesures de l'appareil, **Mesurer maintenant** et **Copier le rapport** (sans nom d'équipement).

##### Chat, Agendas, thèmes perso
 - **Chat** (bouton présent dès qu'un assistant est partagé) : écrire à un assistant du plugin ai_assistant et lire sa réponse ; dictée vocale si le navigateur la propose. Un administrateur choisit les assistants dans **Réglages > Chat IA** ; leur réponse arrive en direct.
 - **Agendas** (à afficher dans **Réglages > Général**) : agendas du plugin Agenda, plage en cours, prochaines plages. Un administrateur règle début, fin, jours et jours fériés d'une plage répétée par jours, ou active / désactive un agenda. Enregistrer une plage qui couvre l'heure actuelle lance ses actions.
 - **Thèmes perso** (**Réglages > Thèmes**) : **Créer un thème** part du thème affiché ; chaque thème a une version claire et une sombre (10 couleurs) et un arrondi, avec aperçu en direct et alerte de contraste. **Exporter** / **Importer un fichier** (JSON). Un administrateur peut **Partager** un thème à tous les comptes.

##### Tuile scénario
Le type **Scénario** (éditeur de widget) lance ou arrête un scénario Jeedom choisi dans la liste. La tuile affiche son état (en cours, arrêté, désactivé) et son dernier lancement, relus toutes les 10 secondes. Un utilisateur limité n'agit que sur les scénarios autorisés par ses droits Jeedom.

##### Menu ⋮ des tuiles
Chaque tuile a un bouton **⋮** en haut à droite (hors mode édition). Son menu indique le plugin de l'équipement et propose :
 - **Détails** : fiche de l'équipement (aussi par un appui long sur la tuile) ;
 - **Caméra** (tuile caméra) : l'image ou le flux en grand ;
 - **Masquer** : retire la tuile pour votre compte, sur tous vos appareils (réaffichage dans **Réglages > Éléments masqués**) ;
 - pour un administrateur : **Modifier** (ouvre l'éditeur en mode édition), **Dupliquer**, **Déplacer vers** un autre onglet, **Supprimer** ; ces trois derniers sont enregistrés tout de suite.

Un simple clic sur une tuile allume ou éteint l'équipement quand c'est possible, sinon ne fait rien.

##### Réglages
Le bouton **Réglages** ouvre une page (adresse `/plugins/jeedash/app/#settings`) qui remplace la grille ; un clic sur un onglet y ramène.
 - **Thèmes** : mode **Auto** (suit le réglage clair ou sombre du téléphone), **Clair** ou **Sombre**, propre à l'appareil ; **thème** au choix, enregistré pour votre compte (tous vos appareils). Chaque thème a une version claire et une version sombre : le mode choisit laquelle s'affiche.
 - **Cartes** (cet appareil) : disposition **Complètes**, **Compactes** ou **Mini** (tuiles moins hautes), et arrondi des cartes.
 - **Écran** (cet appareil) : taille de l'interface, police, taille de l'horloge (ou masquée), plein écran.
 - **Général** (votre compte) : **onglet d'accueil**, affiché à l'ouverture sur tous vos appareils. Une adresse qui désigne un onglet (`#tab=…`) reste prioritaire.
 - **Connexion** : box et compte connectés, ouverture de Jeedom, déconnexion. La version de JeeDash est indiquée en bas de page.

##### Voyant de connexion
 - **Couleur du thème** : valeurs en direct.
 - **Orange clignotant** : reconnexion en cours (automatique).
 - **Orange fixe** : temps réel interrompu (Jeedom injoignable), nouvel essai automatique.
 - **Rouge** : hors ligne.

##### Mode édition (administrateurs)
 - **Ajouter** : choisir le type de widget, l'équipement puis les commandes. Un aperçu s'affiche en direct.
 - Cliquer un widget pour le sélectionner : barre d'outils **Configurer**, **Copier**, **L+ / L-**, **H+ / H-** (taille), **→ Tab** (déplacer vers un autre onglet), **Suppr.**
 - Glisser-déposer pour réorganiser, poignée en bas à droite pour redimensionner.
 - Options d'un widget : **badges** (petites infos ou actions sur le widget), **condition d'affichage** (le widget n'apparaît que si une commande info vérifie une condition), **actions au toucher** (simple appui, double appui, appui long : action par défaut, panneau d'informations, bascule ou rien).
 - Onglets : jusqu'à 20, avec un nom et une icône (emoji).
 - **Onglet depuis une pièce** : au bouton **+** des onglets, choisir une pièce Jeedom dans « Remplir depuis la pièce ». Un widget est créé par équipement visible de la pièce, son type choisi d'après les types génériques des commandes (lumière, interrupteur, volet, thermostat, capteur…) ; un équipement sans type générique reconnu devient une tuile **Équipement**. Les widgets restent à ajuster, puis **Sauver**.
 - **Sauver** enregistre ; **Annuler** abandonne les modifications.
 - Des modèles prêts à l'emploi (tableau de bord complet ou groupe de widgets) peuvent être appliqués.

##### Fiche d'un équipement
Le panneau d'informations (action au toucher **Panneau d'informations**, réglable par widget) affiche :
 - l'éventuel **défaut** de l'équipement (pile, injoignable, alerte) ;
 - la **valeur principale** et sa **courbe** sur **24 h** ou **7 jours** (min, max ; un toucher sur une autre info numérique ou binaire de la liste la trace à son tour) ;
 - toutes les **infos visibles** de l'équipement, en direct ;
 - ses **actions** selon leur type : bouton, curseur, liste de choix, couleur, message (titre et texte).

Le bouton retour du téléphone ferme la fiche. Les protections Jeedom (droits, confirmation, code) s'appliquent comme sur les widgets.

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
 - Seules les commandes **visibles dans Jeedom** des équipements présents dans le tableau de bord peuvent être lues ou actionnées (widgets et fiche d'équipement), et le nombre d'actions par seconde est limité.

#### 6 - Accès à distance
 - **Par le DNS Jeedom (`*.jeedom.link`)** : tout fonctionne. Le relais ne laisse pas passer le flux du démon : après 10 secondes, l'application passe d'elle-même en long-polling (valeurs en direct, à une seconde près). Pour éviter ces 10 secondes, choisir **Long-polling seul** dans la configuration du plugin.
 - **Par un VPN** (Tailscale, WireGuard…) ou un accès direct à votre réseau : tout fonctionne, temps réel compris.

#### 7 - Dépannage
##### Le voyant reste orange ou rouge
 - Vérifier que le démon est démarré (page du plugin).
 - En accès par le DNS Jeedom : voyant orange fixe normal, voir la section 6.
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
