---
title: Workflow, outils et ressources de développement
category: Commercial
order: 1
---

# [OSP][Decidim] Workflow, outils et ressources de développement

## Onboarding dev
- Team OSP sur Github
- Adresse mail OSP
- Dossier drive "Technique"
- Rocket Chat
- Matomo
- Serveur dédié ?

## Outils dev
* [Github d'OSP](https://github.com/OpenSourcePolitics)
    * `/decidim` : repo de référence, où l'on intègre les dev spécifiques à OSP et les màj de `decidim/decidim`. Il faut **au maximum réaliser les développements sur ce repo.**
    * `/decidim-[feature-name]` : engine (eg. `/decidim-polis`) qui peut être activé à travers le Gemfile de l'app
    * `/osp-app` : 
        * `/osp-app#master` : application qui compile `/decidim`
        * `/osp-app#alt/[client-name]` : application custo pour un client (custo graphique, trad spécifiques)
* [Waffle.io](https://waffle.io/OpenSourcePolitics/decidim/join
) qui centralise toutes les issues (Voir [Mode d'emploi du WaffleBot](
https://help.waffle.io/wafflebot-basics/getting-started-with-the-wafflebot/how-to-use-wafflebot))

## Rédaction et gestion des issues GH

* **IN ENGLISH** as much as we can !
* **Une issue = une modification**
Une bonne issue correspond à une Story de type  "As an [user, admin, MO] I can [update a proposition, chose among more stages, ...]"
* [Pro tips for mastering issues on GH](https://guides.github.com/features/issues/) is worth the read ;)

### Attribuer une Milestone à une issue
Dans le cadre d'OSP, un milestone correspond à une phase d'un projet sur le modèle `[phase]-[client-name]`, e.g. : `2nd_retour-Angers`
Ajouter une due date au milestone permet ensuite de les trier par ordre de priorité, ce qui permet d'améliorer le dispatch des issues entre les devs.


### Attribuer des Tags à une issue (soit par OSP soit par les développeurs)
1. Projet, e.g. `nouvelle-aquitaine`
1. Prioriser (OSP) :
    - `urgent` : signale que l'issue doit être résolue en priorité
2. Type (OSP) :
    - `bug` : signalement d'un bug (côté dev ou client) à priori urgent
    - `question`: question à priori non-urgente
    - `enhancement` : proposition d'amélioration à priori non-urgente
    - `feature` : proposition d'une nouvelle feature à priori non-urgente
    - `idea` : proposition autre à priori non-urgente
3. Élément d'architecture affecté (OSP) :
    - general, e.g. `core`,`admin`
    - feature, e.g.`proposals`,`comments`
    - spaces, e.g. `assemblies`, `processes`, `initiatives`
4. Status (Devs) :
    - `blocked` : impossible d'avancer sur cette issue (problème technique/demande est impossible)
    - `design-required` : besoin d'écran
    - `needs-definition` : l'issue n'est pas assez détaillée/précise/claire
    - `needs-feedback` : besoin de réponse aux commentaires
    - `planned` : fait partie de la roadmap d'OSP
    - `ready`

## POUR LES DEVELOPPEURS : Processus à suivre quand une tâche vous est assignée
A réception du CdC client, OSP réalise des issues, idéalement 

OSP fournit au(x) dev(s): 
- une issue détaillée décrivant ce qu'il y a à faire
- des informations concernant l'endroit où ça doit être codé

Côté dev : 
- si l'issue n'est pas claire, poser des questions (sur quel repo faut-il créer la branche ? Faut-il créer un nouveau repo dans le cas d'une nouvelle feature ? etc.) directement sur Github, jusqu'à ce que la spec soit claire
- si les développeurs acceptent la / les tâches, ils font un devis (nombre de jours passé sur les /l'issue) incluant :
    - le développement; 
    - la recette : le déploiement sur un environnement de test (heroku = OK );
    - la résolution des beugs + corrections / ajustement si cahier des charges (l'issue) n'est pas respecté;

Côté OSP : 
- Julie (ou autre membre de l'équipe OSP) teste le déploiement et fait des retours sur l'issue
- Une fois terminé, faire une PR, idéalement un autre dev fait la code review et merge
- OSP la déploiera sur ses environnement de prod, des erreurs peuvent survenir à cette étape. C'est donc seulement lorsque plus aucune anomalie n'est détectée sur les serveurs de production que la tâche est finie.


### Bonnes pratiques de communication / information 
- Faire des messages clairs et fournis dans les commits et les PR. Prendre le temps de rappeler les issues concernées.
- Utiliser [GH's closing keywords](https://help.github.com/articles/closing-issues-using-keywords/), _e.g. fixes #29 (keywords marchent aussi dans les commits)_
- Coder sur une nouvelle branche nommée d'après l'issue à résoudre, _e.g. pour "Issue #29 Bug on comment form", la branche s'appelle `29-bug-comment-form`._
- Prendre le temps de bien rédiger/màj le [README.md](https://doc.meta.osp.cat/IYdgzAnAHAjAZnAtFApgEwCyI8ATANmQxV0QGMAjGKNOMKfOABhSA===?both#)
    - Quelles migrations à installer ?
    - Préciser les configurations s'il y en a des spécifiques
- Lors de la livraison du code, être clair sur la prochaine étape, e.g. "J'attends le code de Michel pour clore mon issue"
- Documenter dans le [Doc d’usage & résolution de bugs](https://doc.meta.osp.cat/GwUwhgrADAZmCcBaCAOAJjRAWK8BMi8IA7AIzYDGWEIedwEFEQA=#) les résolutions de bugs/commandes utilisées si ça peut être utile aux développeurs qui suivront, _e.g. les importations de migration_

## Resources
* Repo [Github](github.com/decidim/decidim) officiel de Decidim

### Documentation 
* **[Doc d'usage OSP & résolution de bugs](https://doc.meta.osp.cat/GwUwhgrADAZmCcBaCAOAJjRAWK8BMi8IA7AIzYDGWEIedwEFEQA=?view)**
*   Le [dossier de doc](https://github.com/decidim/decidim/blob/master/docs/)
    *   [How to create a plugin](https://github.com/decidim/decidim/blob/master/docs/how_to_create_a_plugin.md)
    *   [Features and components](https://github.com/decidim/decidim/blob/master/docs/features_and_components.md)
*   Documentation [administrateur](https://docs.google.com/document/d/10OVuLnFm6yY-VgoA31D2eYMkXzOo3K7HQbzFpXrPbmw/edit#heading=h.wr4tepcqk6x0) pour mieux comprendre le périmètre fonctionnel de l'application
*   [Style guide](https://decidim-design.herokuapp.com/style-manual)

### Vidéos
* [Vidéo de présentation par le lead dev Josep Jaume](https://youtu.be/SKWaiNRYgNA?t=21m5s)
* [Vidéo de présentation par le product manager Xabier Barandarian](https://youtu.be/SKWaiNRYgNA?t=13s)
* * [Video call avec Josep Jaume](https://www.youtube.com/watch?v=EqSpQ4t12L4), lead developper de Codegram  où il nous explique l'architecture de Decidim (qualité pourrie)

### Liens vers des instances Decidim
* Instance de [démo Decidim OSP](https://demo.decidim.opensourcepolitics.eu/) pour comprendre le fonctionnement de l'application en tant qu'administrateur avec accès Mako
* Instance de [staging Decidim Codegram (Barcelone)](http://staging.decidim.codegram.com/) pour comprendre le fonctionnement de l'application originale en tant qu'administrateur avec accès admin decidim
* [Suivi technique des instances](https://docs.google.com/spreadsheets/u/1/d/1No60bAjYvJ459jx3DVaykH4M1_LM1m3eIvkwyLFA00w/edit#gid=0)
