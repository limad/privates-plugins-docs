---
layout: default
title: Documentation JeeMarket
lang: fr_FR
pluginId: jeemarket
---

# JeeMarket

#### 1 - Description
JeeMarket se connecte à votre compte **développeur** du Market Jeedom et remonte dans Jeedom :
 - le **solde** (versé / en cours) ;
 - vos **gains** (part développeur) du jour, des 7 derniers jours, du mois et de l'année, au total et **par plugin** ;
 - la liste de vos plugins avec les **dates des versions bêta et stable**, le nombre d'installations, d'achats et de téléchargements, et la note ;
 - des **alertes** lorsque le solde évolue ou lorsqu'un paiement Jeedom est détecté.

Le Market n'ayant pas d'API publique pour ces données, le plugin lit les pages du Market avec votre compte (comme votre navigateur). Si le Market modifie ses pages, le plugin signale l'erreur dans son log.

#### 2 - Configuration du plugin
Installer et activer le plugin. Le compte Market et, en option, GitHub se configurent ici ; les commandes de notification se choisissent sur l'équipement (sections *Alertes* et *Annonce quotidienne*).

#### 3 - Équipement
Dans la **configuration du plugin**, renseigner l'**email** et le **mot de passe** du Market puis cliquer sur **Tester le compte** (l'œil affiche le mot de passe). Les deux sont enregistrés **chiffrés**. Le plugin se connecte au Market et vérifie qu'il s'agit bien d'un **compte développeur** (un simple utilisateur est refusé) : seulement dans ce cas, l'équipement **JeeMarket** est créé. Il n'y a **qu'un seul équipement** (pas de bouton Ajouter ni Dupliquer) ; enregistrer la configuration valide aussi le compte si les identifiants ont changé. Les comptes avec **code à 2 facteurs** ne sont pas gérés. À la mise à jour, les identifiants des anciens équipements sont repris dans la configuration du plugin.

Cases d'alerte :
 - être alerté lorsque le solde non payé évolue ;
 - être alerté lorsque le paiement est détecté (le solde non payé repasse à 0).

Choisir la **commande** (type message) qui enverra ces alertes ; vide = aucune alerte envoyée.

#### 4 - Commandes
| Commande | Contenu |
|---|---|
| Solde total | Montant déjà versé |
| Solde non payé | Montant en cours (non encore reversé) |
| Gain du jour / 7 derniers jours / du mois / de l'année | Somme de la part développeur des ventes de la période |
| Gains cumulés | Somme de la part développeur de toutes les ventes (total gagné) |
| Gain mois - *plugin* / Gain année - *plugin* | Mêmes gains pour chaque plugin vendu dans l'année |
| Rafraichir | Lance une actualisation immédiate |

Les commandes utilisent les widgets **iso** (installés avec le plugin). Le bouton **Défaut config** de l'équipement recrée les commandes de base manquantes et réapplique ces widgets, sans écraser un autre widget choisi. Le **mode édition** (icône crayon) du tableau des commandes permet d'ajuster nom, unité, min/max, visibilité et historisation.

Les gains sont historisés. Le rafraîchissement automatique a lieu toutes les heures (ainsi que le détail des plugins) ; le bouton « Actualiser » force une mise à jour immédiate.

Le bouton **Historique** de la barre de l'équipement reconstitue l'**historique mensuel des gains** à partir de **toutes les ventes du Market** : un point par mois pour le gain du mois, le gain de l'année (remis à zéro chaque janvier), le cumul total (commande **Gains cumulés**) et, pour les plugins dont la commande par plugin existe déjà (vendus dans l'année), le gain du mois et de l'année. Les points déjà présents sur la période sont remplacés (ré-exécutable sans doublon). La purge d'historique est désactivée sur ces commandes pour que les points anciens ne soient pas effacés par le nettoyage de Jeedom.

#### 5 - Modale « Gains et plugins »
Bouton **Gains et plugins** de la page du plugin (ou icône € de la barre de l'équipement). Affiche les gains par période, puis un tableau de vos plugins (installations, achats, téléchargements, note, gains du mois, de l'année et total). Les données viennent du cache ; le bouton **Actualiser depuis le Market** force une mise à jour complète. L'option **Plugins exclus des gains** (page de l'équipement) écarte les ventes de plugins repris d'un autre développeur.

#### 5 bis - Modale « Market »
Bouton **Market** de la page du plugin (ou icône outils de la barre de l'équipement). Diagnostic côté Market, **sans aucune action sur GitHub**. Pour chaque plugin : badge **privé** (obtenu uniquement sur invitation/clé du développeur) ou **public**, date et commit des versions **bêta** et **stable** connus du Market, et **écart** en jours entre elles (en orange si une branche est en retard). Le lien **À quoi sert cette fenêtre ?** en haut de la modale rappelle le rôle de chaque colonne et de chaque bouton.
- **Test GitHub** (bouton fiole, ou **Tout tester**) : vérifie le jeton GitHub (le vôtre si les fonctions GitHub sont activées dans la configuration du plugin, sinon celui de la fiche Market), l'accès au dépôt et l'existence des branches, et compare le dernier commit GitHub à celui du Market. Permet d'écarter un jeton expiré, un dépôt inaccessible ou une branche inexistante. Une branche laissée vide sur la fiche Market est signalée **non renseignée** (le Market ne publie pas cette version). Aucun jeton n'est affiché, stocké ni loggé.
- **Forcer MAJ** (boutons Bêta / Stable, **usage exceptionnel**) : demande au Market de relire GitHub immédiatement, sans attendre son scan quotidien. Les utilisateurs du plugin voient alors la mise à jour tout de suite, sans changelog : à réserver aux urgences. Un plugin et une branche à la fois, confirmation obligatoire, action tracée en `warning` dans le log.

#### 5 bis (suite) - Modale « GitHub » : branches et promotions (option)
Fonction **désactivée par défaut**. Une fois activée, le bouton **GitHub** apparaît sur la page du plugin (et dans la barre de l'équipement). Sa modale permet de suivre et de promouvoir vos plugins **de branche à branche**, sans ouvrir GitHub ni un terminal.

**Organisation des branches conseillée** (une branche par niveau de maturité, chacune alimentant la suivante) :

| Branche | Rôle | Lue par le Market |
|---|---|---|
| `alpha` | Développement : chaque modification y est poussée et testée sur votre box | Non |
| `beta` | Version bêta, proposée aux utilisateurs bêta-testeurs | Oui (branche bêta de la fiche) |
| `stable` | Version stable, proposée à tous les utilisateurs | Oui (branche stable de la fiche) |

Le code ne circule que dans un sens, **alpha → bêta → stable**, une étape à la fois, par **commit de merge** (jamais de rebase ni de push direct sur bêta ou stable). Un correctif urgent fait directement sur bêta ou stable doit ensuite être reporté sur alpha, sinon l'aperçu le signale à la promotion suivante.

Vos branches peuvent porter **d'autres noms** (`dev`, `develop`, `master`, `main`…) :
- les branches **bêta et stable** sont celles déclarées sur la **fiche Market** de chaque plugin (onglet GitHub de la fiche) : le plugin les lit automatiquement, plugin par plugin ; une branche laissée vide sur la fiche apparaît **non renseignée** et aucune promotion n'est possible vers elle ;
- la branche de **développement**, que le Market ne connaît pas, se règle une fois pour toutes dans la configuration du plugin (`alpha` par défaut). Si elle n'existe pas dans un dépôt, seule la promotion bêta → stable est utilisable pour ce plugin.

**Mise en place** :
1. Sur GitHub : *Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token*. Choisir le propriétaire des dépôts, une date d'expiration, *Only select repositories* (vos dépôts de plugins) et la permission **Contents : Read and write** (Metadata en lecture est ajouté d'office). **Ne pas donner la permission Workflows** : GitHub refusera alors toute fusion qui modifierait `.github/workflows/`, ce qui protège vos branches bêta et stable des fichiers de CI.
2. Dans la configuration du plugin, section **GitHub** : cocher **Activer les fonctions GitHub**, coller le jeton, cliquer sur **Tester** puis enregistrer. Le jeton est stocké **chiffré** et n'apparaît jamais dans les logs. C'est **votre** jeton, distinct de celui de la fiche Market (qui ne sert au Market qu'à lire vos dépôts et n'est jamais réutilisé pour écrire).

**Utilisation** : à l'ouverture, la modale **GitHub** analyse chaque plugin avec votre jeton (bouton **Analyser** pour recommencer). Colonnes : branche de développement, **bêta** et **stable** (dernier commit sur GitHub), avec pour bêta et stable l'état du Market (**à jour**, ou commit **pas encore repris**), et pour chaque branche le nombre de **commits à promouvoir** vers la suivante. Le lien **À quoi sert cette fenêtre ?** rappelle le rôle de chaque élément.

Chaque plugin a deux boutons **→ bêta** et **→ stable** (au nom de vos branches), actifs seulement s'il y a des commits à promouvoir et un droit d'écriture. Un clic affiche d'abord un **aperçu** : nombre de commits et de fichiers, liste des commits, lien de comparaison GitHub et alertes (droit d'écriture manquant, branche protégée, commits présents sur la cible mais absents de la source, fichiers de workflow modifiés). Après confirmation, le plugin fusionne **exactement le commit affiché** dans la branche cible. Le passage en **stable** demande en plus de taper l'id du plugin.

En cas de **conflit**, rien n'est modifié : fusionnez en local (`git merge`), résolvez le conflit et poussez. Le Market reprend la nouvelle version à son scan quotidien ; le bouton **Forcer MAJ** reste réservé aux urgences. Chaque promotion est tracée en `warning` dans le log `jeemarket`.

#### 5 ter - Modale « Mes boxs »
Boxs enregistrées sur votre compte Market (URL, IP, version, dernière synchronisation), service pack et état des services, et état public des services Jeedom (status.jeedom.com).

#### 5 quater - Annonce quotidienne
Sur la page de l'équipement, section **Annonce quotidienne** : cochez *Activer*, choisissez l'**heure**, la **commande** message qui lira ou enverra l'annonce (par exemple la commande « Speak » d'une enceinte Alexa ; vide = aucune annonce envoyée) le champ **titre** (son nom est lu sur la commande : « Volume » pour « Faire parler Alexa » ; vide = rien n'est transmis avec une commande dédiée) et, si besoin, le **texte**. Balises : `#gain_jour#`, `#gain_7j#`, `#gain_mois#`, `#gain_annee#`, `#solde_np#`, `#solde_total#`, `#name#` (montants en euros, virgule décimale). Texte par défaut : « Gains du jour : #gain_jour# euros. Solde en cours : #solde_np# euros. »

Chaque jour, à l'heure choisie (tolérance de 30 minutes si Jeedom était occupé), le plugin interroge d'abord le Market pour avoir des chiffres à jour, puis envoie l'annonce une seule fois. Si l'interrogation échoue, l'annonce est annulée (voir le log `jeemarket`). Le bouton **Tester l'annonce** l'envoie immédiatement avec les valeurs déjà en mémoire, à partir de la configuration enregistrée.

#### 6 - Santé et liens
Sur la page du plugin : **Santé** affiche pour chaque équipement les contrôles (activation, identifiants, dernier refresh réussi ou en échec, fraîcheur des données, alertes et commande d'alerte), les soldes et gains en direct et les gains par plugin, avec un bouton d'actualisation. **Liens** regroupe l'espace développeur du Market, la liste de vos plugins, la documentation, le changelog et la Community. **Assistance** ouvre une demande d'aide pré-remplie sur le forum.

#### 7 - Migration depuis « Gain Market »
Ce plugin remplace l'ancien plugin *Gain Market*. À l'activation de JeeMarket, les équipements, les commandes (et leur historique) et la configuration de l'ancien plugin sont repris automatiquement. L'ancien plugin peut ensuite être supprimé.
