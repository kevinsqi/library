PROJECT='new_project_name'
RUBY_VERSION='2.0.0-p247'

mkdir $PROJECT
cd $PROJECT
rvm --ruby-version --create $RUBY_VERSION@$PROJECT
git add .
git commit -m "Add ruby-version"

cat > Gemfile <<DELIM
source 'https://rubygems.org'
gem 'github-pages'
gem 'foundation'
DELIM
bundle install
git add Gemfile*
git commit -m "Add foundation and jekyll gems"

cd ..
rvm use $RUBY_VERSION@$PROJECT
jekyll new --force $PROJECT
cd $PROJECT
git add .
git commit -m "Create new jekyll project"

cd ..
rvm use $RUBY_VERSION@$PROJECT
foundation new $PROJECT
