---
layout: default
title: Changelog JeeMarket
lang: fr_FR
pluginId: jeemarket
---

# Changelog plugin JeeMarket

>**IMPORTANT**
>
>Pour rappel s'il n'y a pas d'information sur la mise à jour, c'est que celle-ci concerne uniquement de la mise à jour de documentation, de traduction ou de texte.

# 12/11/2026

- Branches GitHub **non renseignées** dans la fiche Market : elles ne sont plus remplacées par `beta` / `stable` par défaut. Les modales **Market** et **GitHub** les affichent « non renseignée », le bouton **Forcer MAJ** de la branche est grisé et la promotion vers une branche non renseignée est refusée avec un message clair (cas d'un plugin sans dépôt ni branche, ou bêta seule)

# 06/11/2026

- Modale **Technique** renommée **Market** : champ **Recherche** (nom, identifiant ou id Market), dates **Bêta / Stable** affichées en entier avec l'heure (dernière version connue du Market, ex. `2026-11-02 01:29:12`), boutons **Stable** en vert
- Modale **Market** : bouton **Rafraîchir** (à côté de **Tout tester**) qui relit la liste enregistrée sans interroger le Market et efface les résultats de test
- Modale **Market** : sablier Jeedom affiché pendant les tests GitHub (**Tout tester** et test d'un plugin)

# 26/10/2026

- Modale **Technique** : le test GitHub utilise le **jeton GitHub de la configuration du plugin** (si les fonctions GitHub sont activées) pour tous les plugins, au lieu du jeton de chaque fiche Market (souvent expiré : « Jeton GitHub refusé »). Boutons **Stable** en bleu, et grisés quand aucune mise à jour n'est nécessaire

# 25/10/2026

- **Fonctions GitHub (option, désactivées par défaut)** : dans la configuration du plugin, jeton GitHub personnel (chiffré, bouton **Tester**) et nom de la branche de développement (`alpha` par défaut). Nouvelle modale **GitHub** (bouton affiché seulement si l'option est active) : état des branches développement → bêta → stable sur GitHub (une seule requête GraphQL par lot de 10 plugins : quelques secondes pour tout le compte), commits à promouvoir, état du Market, bouton **Actualiser depuis le Market**, et **promotion de branche à branche** (boutons **→ bêta** / **→ stable**) : aperçu des commits et alertes, puis fusion par un commit de merge sur GitHub ; le passage en stable demande de taper l'id du plugin. Les branches bêta et stable sont celles déclarées sur la fiche Market de chaque plugin. Organisation des branches conseillée dans la documentation
- Modale **Technique** : aide intégrée (**À quoi sert cette fenêtre ?**), colonne **Bêta avant Stable**, nouvelle icône (outils) ; elle ne fait aucune action sur GitHub
- Modale **Technique** : les boutons **Forcer MAJ** envoyaient une branche vide (refusée) : corrigé
- **Compte Market dans la configuration du plugin** : l'email et le mot de passe ne sont plus portés par l'équipement ; bouton **Valider le compte** (ou simple enregistrement de la configuration)
- L'équipement n'est créé **que si l'identification Market réussit et que le compte est un compte développeur** (un simple utilisateur est refusé, avec message) ; **un seul équipement** : le bouton **Ajouter** et **Dupliquer** disparaissent et un second équipement est refusé
- À la mise à jour, les identifiants des équipements existants sont repris automatiquement dans la configuration du plugin (chiffrés par le core) puis retirés de l'équipement

# 24/10/2026 (7)

- Nouvelle modale **Technique** (bouton de la page du plugin et de la barre de l'équipement) : plugin privé ou public, branches stable/bêta et écart, **test GitHub** (jeton, dépôt, branches, commit GitHub comparé à celui du Market ; le jeton n'est ni affiché, ni stocké, ni loggé) et **Forcer MAJ** (déplacé depuis « Gains et plugins »)
- La modale **Gains et plugins** ne garde que les gains et les statistiques des plugins

# 24/10/2026 (6)

- Nouveau logo du plugin (309x309 px, fond transparent)

# 24/10/2026 (5)

- Nouveau : **Forcer MAJ** (modale « Technique ») : relecture GitHub côté Market d'un plugin et d'une branche (stable ou bêta) à la fois, sans attendre le scan quotidien du Market ; **usage exceptionnel** (urgence) : le Market propose alors la mise à jour aux utilisateurs, sans changelog ; confirmation obligatoire, action tracée en `warning` dans le log ; les détails des plugins sont relus ensuite

# 24/10/2026 (4)

- Nouvelle **annonce quotidienne** (équipement) : à l'heure choisie, le plugin relit le Market puis envoie sur une commande message (par exemple « Speak » d'Alexa) les gains du jour et le solde ; texte personnalisable par balises, option « titre » de la commande (ex. volume d'Alexa), bouton « Tester l'annonce »
- L'onglet « Gains et plugins » devient la modale **Gains et plugins** (bouton de la page du plugin et de la barre de l'équipement, tableau triable par colonne avec recherche, colonne « Écart (j) » triable : jours de retard de la stable sur la bêta, en orange) : la page du plugin n'effectue plus aucune lecture de données
- Nouvelle modale **Mes boxs** : boxs enregistrées sur le compte Market (URL, IP, version, dernière synchronisation), service pack et état des services (accès à distance, assistants vocaux, monitoring, sauvegardes cloud, SMS), et état public des services Jeedom (status.jeedom.com)
- Nouvelle option **Plugins exclus des gains** (équipement) : les ventes de plugins repris d'un autre développeur sont ignorées dans les gains par période et par plugin

# 24/10/2026 (3)

- Rafraîchissement automatique **toutes les heures** (au lieu de toutes les 5 minutes) : moins de charge sur le Market
- Sécurité : les fichiers PHP de `plugin_info/`, `core/class/` et `core/php/` ne sont plus accessibles en direct
- Installation du widget iso renforcée : archive vérifiée, seuls les fichiers de rendu sont copiés, nettoyage systématique
- Un équipement en erreur n'empêche plus le rafraîchissement des autres ; nettoyage des caches à la suppression d'un équipement

# 24/10/2026 (2)

- Commandes : widgets **iso** par défaut et bouton « Défaut config »
- Tableau des commandes compact avec mode édition (comme f2ban)
- Nouvelles modales **Santé** et **Liens**, boutons Assistance et Informations brutes
- Email et mot de passe Market stockés **chiffrés** (reprise automatique des anciens), affichage du mot de passe et bouton « Tester la connexion »

# 24/10/2026

- Renommage du plugin « Gain Market » en **JeeMarket** (reprise automatique des équipements et de la configuration)
- Nouvelle lecture des soldes, compatible avec la page actuelle du Market
- Gains du jour, des 7 derniers jours, du mois et de l'année, au total et par plugin
- Liste des plugins avec dates des versions, installations, achats, téléchargements et note (nouvel onglet)
- Erreurs explicites dans le log (connexion refusée, page du Market modifiée)

# 15/02/2021

- Création du plugin
