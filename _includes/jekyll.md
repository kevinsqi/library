## Initialize project folder with a new gemset

```bash
PROJECT='new_project_name'
RUBY_VERSION='2.0.0-p247'

mkdir $PROJECT
cd $PROJECT
rvm --ruby-version --create $RUBY_VERSION@$PROJECT
git add .
git commit -m "Add ruby-version"
```

## Create gemfile

```bash
cat > Gemfile <<DELIM
source 'https://rubygems.org'
gem 'jekyll'
gem 'jekyll-compass'

gem 'sass', '3.2'
gem 'bourbon', '~> 3.2'
gem 'bitters'
gem 'neat', '1.5'

gem 'capistrano', '~> 2.15.4'
DELIM
bundle install
git add Gemfile*
git commit -m "Add gem dependencies"
```

## Create new jekyll project

```bash
cd ..
rvm use $RUBY_VERSION@$PROJECT
jekyll new --force $PROJECT
cd $PROJECT
git add .
git commit -m "Create new jekyll project"
```

```bash
cat >> .gitignore <<DELIM
.sass-cache
*.swp
DELIM
```

## jekyll-compass

Add to `_config.yml`:

```yaml
gems:
- jekyll-compass
```

Install files:

```bash
compass create -r jekyll-compass --app=jekyll <project_path>
```

## Install bourbon, bitters, and neat

```bash
cd _sass
bourbon install
neat install
bitters install
```

## Deploying with Capistrano

```bash
capify .
```

Customize config/deploy.rb

```bash
wget https://raw.githubusercontent.com/iqnivek/library/master/config/deploy.rb
# customize it and replace config/deploy.rb
git add .
git commit -m "Capify and customize deploy.rb"
```

Setup and deploy

```bash
cap deploy:setup
cap deploy
```

Add site to nginx.conf

```nginx
server {
  listen 80;
  server_name library.kevinqi.com;
  root /home/iqnivek/devel/library/current;
  index index.html;
}
```

Reload nginx config

```bash
sudo /etc/init.d/nginx reload
```
