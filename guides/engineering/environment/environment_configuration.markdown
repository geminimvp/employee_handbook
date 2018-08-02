# Mac OS

This guide was written on `MacOS High Sierra 10.13.6`. It favors Fish syntax.

1. Install [Homebrew](https://brew.sh/)

Homebrew is a package mananger for MacOS. It's like `dnf` for Fedora or `apt-get` for Ubuntu.

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. Install [iTerm2](https://www.iterm2.com/) (Optional)

iTerm2 is a terminal emulator of MacOS. It replaces the standard terminal.

```
$ brew cask install iterm2
```

3. Install [Fish](https://fishshell.com/)

Fish is a alternate shell. It comes with a lot of really cool features like syntax highlighting and tab completion.

Obviously, you can use another shell if you have a favorite. 

```
$ brew install fish

$ echo "/usr/local/bin/fish" | sudo tee -a /etc/shells

$ chsh -s $(which zsh)
```

  a. Install [Bass](https://github.com/edc/bass)

  Bass makes it easier to use Bash utilities in Fish.

  ```
  https://github.com/edc/bass
  ```

4. Install [asdf](https://github.com/asdf-vm/asdf)

asdf is a version manager that works for Ruby, Node, and [about 30 other languages](https://github.com/asdf-vm/asdf-plugins).

```
$ brew install coreutils automake autoconf openssl libyaml readline libxslt libtool unixodbc

$ git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.5.1

$ echo 'source ~/.asdf/asdf.fish' >> ~/.config/fish/config.fish

$ mkdir -p ~/.config/fish/completions; and cp ~/.asdf/completions/asdf.fish ~/.config/fish/completions
```

5. Install preferred text editor

Purely your preference. If you don't have a favorite, here are a few great options:

  - VS Code
  - Atom
  - Sublime Text
  - SpaceMacs
  
6. Generate an SSH key and add it to GitHub

Generate your new SSH key.

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@email.com"

$ eval (ssh-agent -c)
```

Edit your `~/.ssh/config` file:

```
$ vim ~/.ssh/config
```

Add these lines:

```
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```

```
ssh-add -K ~/.ssh/id_rsa
```

Copy your SSH key and then [add it to your GitHub account](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account/).
 
```
$ pbcopy < ~/.ssh/id_rsa.pub
```

7. Install Ruby versions
  
You'll need to install the necessary Ruby versions to work on each Repository. I recommend starting with `2.5.1`.

```
$ asdf plugin-add ruby https://github.com/asdf-vm/asdf-ruby.git

$ asdf install ruby <VERSION>
```

8. Install [bundler](https://bundler.io/)

Bundler is a package manager for Ruby. 

```
$ gem install bundler
```
  
9. Install Node versions

Same as with Ruby. I recommend starting with `8.9.1`.

```
$ brew install gpg

$ asdf plugin-add nodejs https://github.com/asdf-vm/asdf-nodejs.git

$ bash ~/.asdf/plugins/nodejs/bin/import-release-team-keyring

$ asdf install nodejs <VERSION>
```

10. Install [yarn](https://yarnpkg.com/lang/en/)

Yarn is a package manager for Node.

```
$ brew install yarn --without-node
```
  
11. Install [PostgreSQL](https://www.postgresql.org/)

PostgreSQL is relational database.

We can easily install it with Homebrew and set it to run every time we start the machine.

```
$ brew install postgresql

$ pg_ctl -D /usr/local/var/postgres start; brew services start postgresql
```

12. Install [ImageMagick](https://www.imagemagick.org/script/index.php)

ImageMagick is a dependency for pretty much everything.

```
$ brew install imagemagick
```
  
# Engine Storefront

1. Make a new directory `~/src`

```
$ mkdir ~/src
```

2. Make an Engine directory

```
$ mkdir ~/src/engine
```

3. Clone engine_storefront 

```
$ git clone git@github.com:geminimvp/engine_storefront.git
```

4. Bundle

```
$ bundle install
```

5. Yarn install

```
$ yarn install
```

6. Create a `postgres` PostgreSQL role

```
$ psql postgres

postgres=# create role postgres with createdb login;
```

7. Create and seed your databases

```
$ bundle exec rake db:create:all

$ bundle exec rake db:migrate

$ bundle exec rake db:seed

$ bundle exec rake spree_sample:load

```

# Fedora

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
