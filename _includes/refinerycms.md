# On local and remote
sudo apt-get install imagemagick

# Setup .ruby-version and .ruby-gemset
# (see previous)

gem install refinerycms
gem install execjs [???]

# Go to parent folder and make sure using the right gemset
cd ..
rvm use RUBY_VERSION@GEMSET

# Add postgres user (on local and remote)
# (see previous, createuser)

# RefineryCMS generator NOT working:
# refinerycms FOLDERNAME -d postgresql -u USERNAME -p PASSWORD

# Add to existing app
# http://refinerycms.com/guides/with-an-existing-rails-app
rails new FOLDERNAME -d postgresql

# Add refinerycms to gemfile and bundle install (bundle update if needed)

# Initialize refinery
rails generate refinery:cms --fresh-installation

#
# Customizing
#

  # Modifying an existing engine
  # http://stackoverflow.com/questions/7139343/refinerycms-adding-an-image-field-to-the-blog-engine

  # Adding page parts
  # http://refinerycms.com/guides/changing-page-parts
  
  # Overridable views
  # https://gist.github.com/ryandeussing/2502881
