## Prerequisites

Install rvm from https://rvm.io, then:

```bash
rvm install 2.0.0
```

Install postgres:

```bash
sudo apt-get install postgresql libpq-dev
```

Rails 4 requires a javascript runtime:

```bash
sudo apt-get install nodejs
```

## Initialize project

```bash
git clone git@github.com:iqnivek/REPOSITORY_NAME.git
cd REPOSITORY_NAME
```

## Create gemset with rvm

```bash
rvm --ruby-version --create use 2.0.0-p<patchlevel>@REPOSITORY_NAME
git add .
git commit -am "Add ruby-version and gemset"
gem install bundler
gem install rails  # if you want a new version of rails
```

Create rails project:

```bash
cd ..
rvm use 2.0.0-p<patchlevel>@REPOSITORY_NAME
rails new REPOSITORY_NAME -d postgresql
cd REPOSITORY_NAME

git add .
git commit -m "Initialize rails project"
```

Ignore vim swp files:

```bash
echo "*.swp" >> .gitignore
git commit -am "Ignore vim .swp files"
```

Configure database:

```bash
echo "config/database.yml" >> .gitignore
git mv config/database.yml config/database.yml.template
cp config/database.yml.template config/database.yml
git commit -am "Move database.yml to database.yml.template and add database.yml to .gitignore" # enter correct username/password in database.yml
```

## Postgres configuration

Create new user with password prompt, allow creating databases, no superuser, no role creation:

```bash
sudo -u postgres createuser -P -d -R -S <username>
```

[Setup pg_hba.conf as described here.](http://stackoverflow.com/questions/5817301/rake-dbcreate-fails-authentication-problem-with-postgresql-8-4)

Create database:

```bash
rake db:create
rake db:migrate
git add db/schema.rb
git commit -am "Add schema.rb"
```

Test that you can boot rails server:

```bash
rails s
```

## TODO formatting

```
# add some useful gems (some are optional)
# [DEPRECATED] patch -p0 < gemfile-additions.diff  # TODO fix path for real script
#
# ADD LINES TO GEMFILE
# read http://foundation.zurb.com/docs/rails.html
gem 'thin'
gem 'compass'
gem 'compass-rails'
gem 'zurb-foundation'

bundle install
git commit -am "Add a few useful gems"

# configure foundation
rails g foundation:install
# merge changes to layout/application

# generate landing page
# http://stackoverflow.com/a/11003820/341512

# initial controller
rails generate controller Pages index
git add *
git commit -am "Add scaffolded PagesController"

# homepage
git rm public/index.html
# TODO add root :to => 'pages#index' to config/routes.rb
git commit -am "Make pages#index homepage"

# capistrano deployment
# [OLD] capistrano 2: http://guides.beanstalkapp.com/deployments/deploy-with-capistrano.html
# capistrano 3:
# - http://www.capistranorb.com/documentation/getting-started/preparing-your-application/
# - http://www.capistranorb.com/documentation/frameworks/rbenv-rvm-chruby/ 
1. uncomment capistrano from Gemfile
  - also add (???)
    capistrano-rvm
    capistrano-bundler
    capistrano-rails
2. cap install
3. modify cap config
  - Capfile
  - config/deploy.rb
  - config/deploy/production.rb
4. on REMOTE machine
  - install correct ruby
  - prepare repo
    - clone git repo
    - setup database.yml
      - http://stackoverflow.com/a/9686757/341512
      - cap3 use linked_files? https://semaphoreapp.com/blog/2013/11/26/capistrano-3-upgrade-guide.html
      - execute cap production deploy to create directories
      - on LOCAL
        - scp config/database.yml user@my_server.com:path_to_rails_app/shared/config/database.yml
          - can use ssh aliases (linode:path_to_rails_app/...)
        - scp config/database.yml user@my_server.com:path_to_rails_app/config/database.yml
          - (used later for rake db:create:all)
    - bundle install
  - setup db
    - create postgres user with password (see previous createuser command)
    - create database
      - rake db:create:all
  - update nginx config
    - sudo /etc/init.d/nginx reload

5. cap production deploy
```
