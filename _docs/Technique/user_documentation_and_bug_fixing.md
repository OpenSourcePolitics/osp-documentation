---
title: Decidim : Doc d’usage & résolution de bugs
category: Technique
order: 1
---

# Decidim : Doc d’usage & résolution de bugs

# [OSP][Decidim] Doc d'usage & résolution de bugs

## Getting started on osp-app
In order to develop on decidim, you'll need:

* **Git** 2.15+
* **PostgreSQL** 9.4+
* **Ruby** 2.5.0
* **NodeJS** 9.x.x (with `yarn` as a package manager)
* **ImageMagick**

Further documentation on Decidim [can be found here.](http://www.rubydoc.info/github/decidim/decidim/master/frames)
### Importing the migrations
```
rails decidim:upgrade
```
### Setting up admin system access

1. Open a Rails console in the server: `rails console`
2. Create a System Admin user:
```ruby
email = "email"
password = "password"
user = Decidim::System::Admin.new(email: email, password: password, password_confirmation: password)
user.save!
```
Create an admin
```ruby
email = "email"
name = "name"
nickname = "nickname"
password = "password"
Decidim::User.find_or_initialize_by(email: email).update!(
  name: name,
  nickname: nickname,
  password: password,
  password_confirmation: password,
  organization: Decidim::Organization.last,
  confirmed_at: Time.current,
  locale: "fr",
  admin: true,
  tos_agreement: true
)
```
3. Visit `<your app url>/system` and login with your system admin credentials
4. Create a new organization. Check the locales you want to use for that organization, and select a default locale.
5. Set the correct default host for the organization, otherwise the app will not work properly. Note that you need to include any subdomain you might be using.
6. Fill the rest of the form and submit it.


## Identifiants seed
* User
    * user@example.org
    * decidim123456
* Admin
    * admin@example.org
    * decidim123456
* System
    * system@example.org
    * decidim123456
    
## Commandes utiles

### Importer les migrations

```
bundle update --conservative $gem-name
rails decidim:install:migrations
rails decidim_admin:install:migrations
rails decidim_system:install:migrations
rails decidim_accountability:install:migrations
rails decidim_participatory_processes:install:migrations
rails decidim_debates:install:migrations
rails decidim_assemblies:install:migrations
rails decidim_budgets:install:migrations
rails decidim_pages:install:migrations
rails decidim_meetings:install:migrations
rails decidim_comments:install:migrations
rails decidim_surveys:install:migrations
rails decidim_proposals:install:migrations
rake db:create db:migrate db:seed
```
ou
```
rails decidim:upgrade
```

### Migrer une install de Decidim entre 2 repo 

Migrer une install de Decidim entre 2 repo va surement créer des problèmes au niveau du `db:migrate` à cause des préfixes de dates sur les noms de fichiers ( dossier `db/migrate/` ).

Pas de panique ! Il faut enlever les préfixes sur chaque dossier et faire une comparaison entre les 2 dossiers `db/migrate/` (celui d'origine et le nouveau code) :

--> les fichiers en plus sont ceux qu'il faut garder.

Attention ⚠ la feinte c'est qu'il faut remettre seulement les fichiers concernés *avec* le préfixe numérique, sinon vous perdez l'ordre d''exécution des scripts.

Je n'ai pas encore de code / procédure auto pour ça, donc c'est à la main pour l'instant 😛 

*-->* Ne pas lancer d'appli Rails avec un compte `root`
ça peut marcher mais Internet tout entier nous racontent que c'est dangereux pour la sécurité.

⚠ de plus, ça ne marche pas avec le mode NGINX + Passenger (lancement de l'application en synchro avec le serveur)

### Liste de commande Heroku

:::warning
Documentation à compléter : en l'état il s'agit de commandes qui ont pu être utlisées dans le cadre d'un déploiement Heroku. Il est important de comprendre ce qu'elles font et de lire leur documentation :wink:  

https://devcenter.heroku.com/articles/getting-started-with-rails5

https://devcenter.heroku.com/articles/heroku-cli-commands

:::

### Deploying the app on Heroku

#### Without heroku decidim deploy installer'
For a new app :
```
heroku create $app-name --region eu -- org $org-name
heroku addons:create newrelic:wayne -a $app-name
heroku addons:create heroku-redis:hobby-dev -a $app-name
heroku addons:create memcachedcloud:30 -a $app-name
heroku addons:create sentry:f1 -a $app-name
heroku addons:create sendgrid:starter -a $app-name
heroku labs:enable runtime-dyno-metadata -a $app-name
heroku config:set SEED=true -a $app-name
rake secret
heroku config:set SECRET_KEY_BASE=<to_be_changed> -a $app-name
heroku config:set AWS_ACCESS_KEY, OMNIAUTH_FACEBOOK, etc if needed
git push $heroku-remote branch:master -a app-name
```
For an existing app: 
```
heroku git:remote -a $app-name
git push $heroku-remote $branch:master -a $app-name
```

Reseting db: 
```
heroku pg:reset DATABASE -a $app-name --confirm $app-name
heroku run rails db:migrate db:seed db:migrate -a heroku run rails db:migrate db:seed db:migrate -a osp-dcd-free
```
Renaming remote: 
`git remote rename heroku heroku-staging`

Renaming heroku app: 
`heroku apps:rename -a oldname newname`

Heroku logs:
`heroku logs --tail -a $app-name`

Heroku releases & rollback
`heroku releases -a $app-name`
`heroku rollback v# -a $app-name`

#### With heroku deployer
```
# add to gemfile
# gem "decidim-deploy-heroku", git: "https://github.com/codegram/decidim-deploy-heroku.git"
bundle install
rails g decidim:deploy:heroku_installer
```
Then : 
1. Use the "Deploy to Heroku" button
1. Choose a name for the app, and organization and a tier
1. Fill in the required env vars.
1. Create the app
1. Enable Review Apps for this app (you'll need to create a Pipeline)

#### Set up heroku DB

```
heroku run rails db:migrate -a $app-name
# heroku run rails db:seed -a $app-name
```
To set up admin system access as said above, you need to open heroku rails console with `heroku run rails c -a $app-name`


_**// ARCHIVE**_
```
heroku apps --team osp-dev
heroku git:remote -a osp-dcd-dev

git remote rename heroku osp-dcd-dev

heroku git:remote -a osp-dcd-free

SEED=true heroku run rails db:setup
heroku pg:reset DATABASE -a osp-dcd-free --confirm osp-dcd-free
heroku run rails db:migrate db:seed db:migrate -a osp-dcd-free

SECRET_KEY_BASE=# RAILS_ENV=production bin/rails assets:precompile

heroku run rails assets:precompile -a osp-dcd-free
heroku run rails webpacker:install -a osp-dcd-free

heroku labs:enable runtime-dyno-metadata
heroku run rails console
```

### Créer un admin dans la console
Admin System
```
Decidim::System::Admin.new(email: "mako@osp.cat", password: "xxxxxxxxx", password_confirmation: "xxxxxxxxx").save
```
Admin admin
```
email = "admin@example.org"
password = "decidim123456"
user = Decidim::User.find_or_initialize_by(email: email).update!(
  name: "admin",
  nickname: "admin",
  password: password,
  password_confirmation: password,
  organization: Decidim::Organization.last,
  confirmed_at: Time.current,
  locale: "en",
  admin: true,
  tos_agreement: true
)
user.save!
```

## FAQ

### Quelle version choisir ?
**Ludivine**
>Another question, I see that some bugs fixes from 0.8-stable are merged in master. I'm not sure to understand the worflow. You made changes and when those changes are consequent you made an relesase right ? So can I pull master in total security ? I need to have decidim-proposals up to date

**Marc Riera**
>we keep adding things to master and, at some point, we release a new version. If the version has serious bugs, those bugs get fixed on master and backported to the version (branch 0.8-stable) and released as a minor (0.8.x). Using master should be quite stable though



## Architecture logicielle

### Environnement système de l'app
Une fois déployée en production, mettre en place 3 orga sur 3 DNS :  
- Orga seedée (pratique pour les démos) (si possible avec que français comme langue activée)
- Orga de test / bac à sable (pratique pour vérifier qu'un beug est bien patché ou qu'une mise à jour est bien passée)
- Orga en production

### Serveurs existants
https://docs.google.com/spreadsheets/u/1/d/1No60bAjYvJ459jx3DVaykH4M1_LM1m3eIvkwyLFA00w/edit#gid=0


