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

gem 'bourbon'
gem 'bitters'
gem 'neat'
DELIM
bundle install
git add Gemfile*
git commit -m "Add jekyll gem and other dependencies"
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

## jekyll-compass

Add to `_config.yml`:

```yaml
gems:
- jekyll-compass
```

Install files:

```
compass create -r jekyll-compass --app=jekyll <project_path>
```

## Install bourbon, bitters, and neat

```
cd _sass
bourbon install
neat install
bitters install
```

## Deploying with Capistrano

http://wolfslittlestore.be/2013/10/rendering-markdown-in-jekyll/

Use older version of Capistrano in Gemfile:

```
gem 'capistrano', '~> 2.15.4'
```
