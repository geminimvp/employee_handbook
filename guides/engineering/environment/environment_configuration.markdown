
This document will cover environment configuration for getting Engine Storefront up and running. This was tested and written on Fedora 26 (ymmv).

1. [Dependencies](#Dependencies)
2. [rbenv](#rbenv)
3. [Ruby](#ruby)
4. [Bundler](#bundler)
5. [Postgres](#bundler)
6. [GitHub Access](#github-access)
7. [Setup](#setup)

## Dependencies

On **Fedora**, I had to install some dependencies before hand. Here is what I started with:

    $ sudo yum install git-core zlib zlib-devel gcc-c++ patch readline readline-devel libyaml-devel libffi-devel openssl-devel make bzip2 autoconf automake libtool bison curl sqlite-devel
On **Mac OS**, you will need to install the XCode tools:

    $ xcode-select --install
Also, install Homebrew:

    $ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

## rbenv
rbenv is a environment manager for Ruby. There are other options, [rbenv is the best](https://github.com/rbenv/rbenv/wiki/Why-rbenv?).
Installing rbenv is simple in theory, but can be tricky.

**On Fedora:**

    cd
    git clone git://github.com/sstephenson/rbenv.git .rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
    exec $SHELL
    
    git clone git://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
    exec $SHELL
    
    source ~/.bash_profile
    source ~/.bashrc

**On Mac:**

    $ brew install rbenv

Run ``rbenv init`` and follow the instructions.

Run this to check your installation:

    $ curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
If you have problems refer to the [readme](https://github.com/rbenv/rbenv/blob/master/README.md).

## Ruby
Once you've got rbenv working, you should be able to run:

    rbenv install 2.3.6
To install Ruby 2.3.6, which is currently being used by Engine Storefront. You can always check which Ruby version is being used by a project by looking for a `.ruby-version` file in the repo.

**WARNING** I had some serious problems with this on Fedora. 
I was able to get pre-2.4.x Ruby versions install using [this hack](https://github.com/rbenv/ruby-build/issues/1115).

## Bundler
Bundler is a gem manager. It works with Gemfiles, it's really great.

    $ gem install bundler

## Postgres
**On Fedora:** 
Grab the repos with dnf:

    $ sudo dnf install postgresql-server postgresql-contrib

Enable Postgres (you should only have to do this once):

    $ sudo systemctl enable postgresql
Run the setup:

    $ sudo postgresql-setup --initdb --unit postgresql
[Allow connections to Postgres.](https://stackoverflow.com/questions/3278379/how-to-configure-postgresql-to-accept-all-incoming-connections) *This is not secure, but it works for development.*

If you have issues, refer to the [Fedora docs](https://fedoraproject.org/wiki/PostgreSQL#Installation).

**On Mac:**
Install Postgres with brew:

    $ brew install postgresql
Start the postgres service:

    $ brew services start postgresql

## GitHub Access
Ask someone :)

## Setup
Clone the Engine Storefront repo. 

Set your local Ruby version by using `rbenv local 2.3.6`. Install the gems by running `bundle install`.

When I set my local database up, I ran:
    
    $ bundle install && yarn install
    
    $ bundle exec rails db:create:all
    $ bundle exec rails db:migrate
    $ bundle exec rails db:seed
    $ bundle exec spree_sample:load

There are some warning in the repo about running multiple rake tasks at once, so don't.

    $ bundle exec rails s
Go to localhost:3000/admin and you should be able to log in with the credentials you used during the setup.
