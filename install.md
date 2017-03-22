Install the tools we need to build Jila
=====

Documentation for getting the tools to install Jila on OSX and mobile. This is using the forked version which has been updated for latest software versions.

Homebrew, Ruby, NPM, Git
-----

##### Homebrew

Get [Homebrew](http://brew.sh/) set up.

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


##### Ruby

Jila requires Ruby On Rails. Check if you have the system version of ruby.

    ruby -v

If this returns a value containing 'universal' then let's install a Ruby version manager so we leave the system version alone. Why? [See this article about system-ruby](https://robots.thoughtbot.com/psa-do-not-use-system-ruby)

Get [rbenv](https://github.com/sstephenson/rbenv) using Homebrew.

    brew install rbenv

With rbenv installed, get Ruby and Rails. [More info here.](https://gorails.com/setup/osx/10.10-yosemite)

The backend uses 2.1.6, the mobile part requires 2.2.2. Install them both.

    rbenv install 2.1.6
    rbenv install 2.2.2

Install the 'bundle' gem.

    gem install bundle 


##### NPM

Requires npm (javascript code library manager). This guide is way more complex that installing via brew, but should avoid issues of future updates breaking..
- [npm without sudo](http://www.johnpapa.net/how-to-use-npm-global-without-sudo-on-osx/)
- [or here's another method](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md)

##### Git

Gotta have Git. Follow the steps at the links below.

[Install Git and create a GitHub account.](http://guides.beanstalkapp.com/version-control/git-on-mac.html)

[Add an SSH key to Git account.](http://burnedpixel.com/blog/setting-up-git-and-github-on-your-mac/)

[Or see the configuring git docs.](https://gorails.com/setup/osx/10.10-yosemite)

With that done, test your ssh connection to GitHub.

    ssh -T git@github.com

Set up a global gitignore. Add .DS-Store to this - look for relevant gists.

    git config --global core.excludesfile '~/.gitignore_global'


Next
=====

See the [backend](get_backend.md) and [mobile](get_mobile.md) docs for steps on getting and customising the code.

See the [deploy doc](deploy.md) for info about publishing the app and backend.
