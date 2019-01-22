# Mac OS X Dev Setup

Forked from https://github.com/nicolashery/mac-dev-setup

This document describes how I set up my developer environment on a new MacBook or iMac. We will set up popular programming languages (for example [Node](http://nodejs.org/) (JavaScript), [Python](http://www.python.org/), and [Ruby](http://www.ruby-lang.org/)). You may not need all of them for your projects, although I recommend having these three set up as they always come in handy.

The document assumes you are new to Mac, but can also be useful if you are reinstalling a system and need some reminder. The steps below were tested on **OS X El Capitan** (10.11).

- [System update](#system-update)
- [System preferences](#system-preferences)
- [Security](#security)
- [Projects folder](#projects-folder)
- [Google Chrome](#google-chrome)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [Consolas](#consolas)
- [Beautiful terminal](#beautiful-terminal)
- [Git](#git)
- [Atom Text Editor](#atom-text-editor)
- [EditorConfig](#editorconfig)
- [Python](#python)
- [Node.js](#nodejs)
- [Ruby](#ruby)
- [OpenSSL](#openssl)
- [MySQL](#mysql)
- [Github Desktop](#github-desktop)
- [Sourcetree](#sourcetree)
- [Google Cloud](#google-cloud)
- [PostgreSQL](#postgresql)
- [Other apps](#other-apps)

## System update

First thing you need to do, on any OS actually, is update the system! For that: **Apple Icon > About This Mac** then **Software Update...**.

## System preferences

If this is a new computer, there are a couple tweaks I like to make to the System Preferences. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Preferences**:

- Trackpad > Tap to click
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)

## Security

I recommend checking that basic security settings are enabled. You will be happy to have done so if ever your Mac is lost or stolen.

In **Apple Icon > System Preferences**:

- Users & Groups: If you haven't already set a password for your user during the initial set up, you should do so now
- Security & Privacy > General: Require password immediately after sleep or screen saver begins (you can keep a grace period of a couple minutes if you prefer, but I like to know that my computer locks as soon as I close it)

## Projects folder

This really depends on how you want to organize your files, but I like to put all my version-controlled projects in `~/Projects`. Other documents I may have, or things not yet under version control, I like to put in `~/Documents`. Sometimes, I get mixed up and try to access my projects by going to `~/Documents/Projects`, so it may be worth it to create a symlink between the two.

## Google Chrome

Install your favorite browser, mine happens to be Chrome.

Download from [www.google.com/chrome](https://www.google.com/chrome/). Open the **.dmg** file once it's done downloading (this will mount the disk image), and drag and drop the **Google Chrome** app into the Applications folder (on the Mac, most applications are installed this way). When done, you can unmount the disk in Finder (the small "eject" icon next to the disk under **Devices**).

## iTerm2

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one. Download and install [iTerm2](http://www.iterm2.com/).

In **Finder**, drag and drop the **iTerm** Application file into the **Applications** folder.

You can now launch iTerm, through the **Launchpad** for instance.

Let's just quickly change some preferences. In **iTerm2 > Preferences...**, under the tab **General**, uncheck **Confirm closing multiple sessions** and **Confirm "Quit iTerm2 (Cmd+Q)" command** under the section **Closing**.

In the tab **Profiles**, create a new one with the "+" icon, and rename it to your first name for example. Then, select **Other Actions... > Set as Default**. Under the section **General** set **Working Directory** to be **Reuse previous session's directory**. Finally, under the section **Window**, change the size to something better, like **Columns: 125** and **Rows: 35**.

When done, hit the red "X" in the upper left (saving is automatic in OS X preference panes). Close the window and open a new one to see the size change.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://brew.sh/).

### Install

An important dependency before Homebrew can work is the **Command Line Developer Tools** for **Xcode**. These include compilers that will allow you to build things from source. You can install them directly from the terminal with:

```
xcode-select --install
```

Once that is done, we can install Hombrew by copy-pasting the installation command from the [Homebrew homepage](http://brew.sh/) inside the terminal.

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Follow the steps on the screen. You will be prompted for your user password so Homebrew can set appropriate permissions.

Once installation is complete, you can run the following command to make sure everything works:

```
brew doctor
```

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

```
brew install <formula>
```

To update Homebrew's directory of formulae, run:

```
brew update
```

**Note**: I've seen that command fail sometimes because of a bug. If that ever happens, run the following (when you have Git installed):

```
cd /usr/local
git fetch origin
git reset --hard origin/master
```

To see if any of your packages need to be updated:

```
brew outdated
```

To update a package:

```
brew upgrade <formula>
```

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

```
brew cleanup
```

To see what you have installed (with their version numbers):

```
brew list --versions
```

### Homebrew Services

A nice extension to Homebrew is [Homebrew Services](https://github.com/Homebrew/homebrew-services). It will automatically launch things like databases when your computer starts, so you don't have to do it manually every time.

To install it, simply run:

```
brew tap homebrew/services
```

After installing a service (for example a database), it should automatically add itself to Hombrew Services. If not, you can add it manually with:

```
brew services <formula>
```

Start a service with:

```
brew services start <formula>
```

At anytime you can view which services are running with:

```
brew services list
```

## Consolas

I really like the Consolas font for coding. Being a Microsoft (!) font, it is not installed by default. Since we're going to be looking at a lot of terminal output and code, let's install it now.

There are two ways we can install it. If you bought **Microsoft Office for Mac**, install that and Consolas will be installed as well.

If you don't have Office, follow these steps:

```
brew install cabextract
cd ~/Downloads
mkdir consolas
cd consolas
curl -LO https://sourceforge.net/projects/mscorefonts2/files/cabs/PowerPointViewer.exe
cabextract PowerPointViewer.exe
cabextract ppviewer.cab
open CONSOLA*.TTF
```

And click **Install Font**. Thanks to Alexander Zhuravlev for his [post](http://blog.ikato.com/post/15675823000/how-to-install-consolas-font-on-mac-os-x).

## Beautiful terminal

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place. What follows might seem like a lot of work, but trust me, it'll make the development experience so much better.

Let's go ahead and start by changing the font. In **iTerm > Preferences...**, under the tab **Profiles**, section **Text**, change both fonts to **Consolas 13pt**.

Now let's add some color. I'm a big fan of the [Solarized](http://ethanschoonover.com/solarized) color scheme. It is supposed to be scientifically optimal for the eyes. I just find it pretty.

Scroll down the page and download the latest version. Unzip the archive. In it you will find the `iterm2-colors-solarized` folder with a `README.md` file, but I will just walk you through it here:

- In **iTerm2 Preferences**, under **Profiles** and **Colors**, go to **Load Presets... > Import...**, find and open the two **.itermcolors** files we downloaded.
- Go back to **Load Presets...** and select **Solarized Dark** or **Solarized Light** to activate it. Voila!

**Note**: You don't have to do this, but there is one color in the **Solarized Dark** preset I don't agree with, which is *Bright Black*. You'll notice it's too close to *Black*. So I change it to be the same as *Bright Yellow*, i.e. **R 83 G 104 B 112**.

Not a lot of colors yet. We need to tweak a little bit our Unix user's profile for that. This is done (on OS X and Linux), in the `~/.bash_profile` text file (`~` stands for the user's home directory).

We'll come back to the details of that later, but for now, just download the files [.bash_profile](https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.bash_profile), [.bash_prompt](https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.bash_prompt), [.aliases](https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.aliases) attached to this document into your home directory (`.bash_profile` is the one that gets loaded, I've set it up to call the others):

```
cd ~
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.bash_profile
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.bash_prompt
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.aliases
```

With that, open a new terminal tab (Cmd+T) and see the change! Try the list commands: `ls`, `ls -lh` (aliased to `ll`), `ls -lha` (aliased to `la`).

At this point you can also change your computer's name, which shows up in this terminal prompt. If you want to do so, go to **System Preferences** > **Sharing**. For example, I changed mine from "Nicolas's MacBook Air" to just "MacBook Air", so it shows up as `MacBook-Air` in the terminal.

Now we have a terminal we can work with!

(Thanks to Mathias Bynens for his awesome [dotfiles](https://github.com/mathiasbynens/dotfiles).)

## Git

Mac OS X comes with a pre-installed version of [Git](http://git-scm.com/), but we'll install our own through Homebrew to allow easy upgrades and not interfere with the system version. To do so, simply run:

```
brew update
brew install git
```

When done, to test that it installed fine you can run:

```
which git
```

The output should be `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.gitconfig) file to your home directory:

```
cd ~
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.gitconfig
```

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/)):

```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, we're going to use the recommended HTTPS method (versus SSH). So you don't have to type your username and password every time, let's enable Git password caching as described [here](https://help.github.com/articles/set-up-git):

```
git config --global credential.helper osxkeychain
```

On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your project `.gitignore` files. You can take a look at this repository's [.gitignore](https://github.com/nicolashery/mac-dev-setup/blob/master/.gitignore) file for inspiration. You can also set it up in a global `.gitignore` file by cloning it into your home directory (but you'll want to make sure any collaborators also do it):

```
cd ~
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.gitignore
```

## Atom Text Editor

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but if you're just getting started and looking for something simple that works, I recommend [Atom](https://atom.io/). Some people prefer [Sublime Text](http://www.sublimetext.com/), so if you would rather use that, look up the instructions in the [original setup instructions](https://github.com/nicolashery/mac-dev-setup#sublime-text).

Go ahead and [download](https://atom.io/) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

**Note**: At this point I'm going to create a shortcut on the OS X Dock for both for Atom and iTerm. To do so, right-click on the running application and select **Options > Keep in Dock**.

You are welcome to use some of my Atom editor settings. Go to **Atom > Preferences > Editor** and change the following:

- **Font Family:** Monaco, Consolas, 'Courier New', Courier
- **Font Size:** 15
- **Preferred Line Length:** 120
- Check **Scroll Past End**
- Check **Soft Tabs**
- **Tab Length:** 4
- **Tab Type:** auto

## EditorConfig

To make sure that all our code uses the same indentation and spacing type, we're going to use the awesome [EditorConfig](https://editorconfig.org/) plugin for our text editor. To install the plugin for Atom, go to **Atom > Preferences > Install**, search for "editorconfig", and install the plugin. Now download the EditorConfig file from this repo and place it at the project root directory.

```
cd path/to/my/project
curl -O https://raw.githubusercontent.com/rgutierrez-cotech/mac-dev-setup/master/.editorconfig
```

For every new project you start, you'll need to copy this file into the project root directory.

## Python

OS X, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version using [pyenv](https://github.com/yyuu/pyenv). This will also allow us to manage multiple versions of Python (ex: 2.7 and 3) should we need to.

Install `pyenv` via Homebrew by running:

```
brew install pyenv
```

When finished, you should see instructions to add something to your profile. Open your `.bash_profile` in the home directory and add the following line:

```bash
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
```

Save the file and reload it with:

```
source ~/.bash_profile
```

We can now list all available Python versions by running:

```
pyenv install --list
```

Look for the latest 2.7.x version (or 3.x), and install it:

```
pyenv install 2.7.11 # the latest version might be different
```

List the Python versions you have locally with:

```
pyenv versions
```

The start (`*`) should indicate we are still using the `system` version. We should set the one we installed to be the default:

```
pyenv global 2.7.11
```

You should now see that version when running:

```
python --version
```

For more information, see the [pyenv commands](https://github.com/yyuu/pyenv/blob/master/COMMANDS.md) documentation.

### pip

[pip](https://pip.pypa.io) was also installed by `pyenv`. It is the package manager for Python.

Here are a couple Pip commands to get you started. To install a Python package:

```
pip install <package>
```

To upgrade a package:

```
pip install --upgrade <package>
```

To see what's installed:

```
pip freeze
```

To uninstall a package:

```
pip uninstall <package>
```

### virtualenv

[virtualenv](https://virtualenv.pypa.io) is a tool that creates an isolated Python environment for each of your projects. For a particular project, instead of installing required packages globally, it is best to install them in an isolated folder, that will be managed by `virtualenv`.

The advantage is that different projects might require different versions of packages, and it would be hard to manage that if you install packages globally. It also allows you to keep your global `site-packages` folder clean, containing only big packages that you always need (for example Numpy and Scipy).

Instead of installing and using `virtualenv` directly, we'll use the dedicated `pyenv` plugin [pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv) which will make things a bit easier for us. Install it via Homebrew:

```
brew install pyenv-virtualenv
```

After installation, add the following line to your `.bash_profile`;

```bash
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

And reload it with:

```
source ~/.bash_profile
```

Now, let's say you have a project in a directory called `myproject`. You can set up virtualenv for that project and the Python version it uses (for example 2.7.11):

```
cd ~/Projects/myproject
pyenv virtualenv 2.7.11 myproject
```

To use your project's virtualenv, you need to **activate** it first. Instead of running `activate`, make sure you are in your project directory and do the following:

```
pyenv local myproject
```

You should see a `(myproject)` appear at the beginning of your terminal prompt indicating that you are working inside the virtualenv. Now when you install something:

```
pip install <package>
```

It will get installed in that virtualenv's folder, and not conflict with other projects.

### IPython

[IPython](http://ipython.org/) is an awesome project which provides a much better Python shell than the one you get from running `python` in the command-line. It has many cool functions (running Unix commands from the Python shell, easy copy & paste, creating Matplotlib charts in-line, etc.) and I'll let you refer to the [documentation](http://ipython.org/ipython-doc/stable/index.html) to discover them.

Before we install IPython, we'll need to get some dependencies. Run the following:

```
brew update # Always good to do
brew install zeromq # Necessary for pyzmq
brew install pyqt # Necessary for the qtconsole
```

It may take a few minutes to build these.

Once it's done, we can install IPython with all the available options:

```
pip install ipython[zmq,qtconsole,notebook,test]
```

You can launch IPython from the command line with `ipython`, but what's more interesting is to use its [QT Console](http://ipython.org/ipython-doc/stable/interactive/qtconsole.html). Launch the QT Console by running:

```
ipython qtconsole
```

You can also customize the font it uses:

```
ipython qtconsole --ConsoleWidget.font_family="Consolas" --ConsoleWidget.font_size=13
```

And since I'm lazy and I don't want to type or copy & paste that all the time, I'm going to create an alias for it. Create a `.extra` text file in your home directory with `atom ~/.extra` (I've set up `.bash_profile` to load `.extra`), and add the following line:

```bash
alias ipy='ipython qtconsole --ConsoleWidget.font_family="Consolas" --ConsoleWidget.font_size=13'
```

Open a fresh terminal. Now when you run `ipy`, it will launch the QT Console with your configured options.

To use the in-line Matplotlib functionality (nice for scientific computing), run `ipy --pylab=inline`.

## Node.js

The recommended way to install [Node.js](http://nodejs.org/) is to use [nvm](https://github.com/creationix/nvm) (Node Version Manager) which allows you to manage multiple versions of Node.js on the same machine.

Install nvm by copy-pasting the [install script command](https://github.com/creationix/nvm#install-script) into your terminal.

Once that is done, open a new terminal and verify that it was installed correctly by running:

```
command -v nvm
```

Install the latest stable version of Node.js with:

```
nvm install node
```

It will also set this version as your default version. You can install another specific version, for example Node 4, with:

```
nvm install 4
```

And switch between versions by using:

```
nvm use 4
nvm use default
```

Installing Node also installs the [npm](https://npmjs.org/) package manager.

### Npm usage

To install a package:

```
npm install <package> # Install locally
npm install -g <package> # Install globally
```

To install a package and save it in your project's `package.json` file:

```
npm install <package> --save
```

To see what's installed:

```
npm list # Local
npm list -g # Global
```

To find outdated packages (locally or globally):

```
npm outdated [-g]
```

To upgrade all or a particular package:

```
npm update [<package>]
```

To uninstall a package:

```
npm uninstall <package>
```

## Ruby

Like Python, [Ruby](http://www.ruby-lang.org/) is already installed on Unix systems. But we don't want to mess around with that installation. More importantly, we want to be able to use the latest version of Ruby.

### Install

We're going to use [rbenv](https://github.com/rbenv/rbenv), which allows you to manage multiple versions of Ruby on the same machine. Installing rbenv, as well as the latest version of Ruby, is very easy. Just run:

```
brew install rbenv
```

When it is done, initialize it:

```
rbenv init
```

It should give you a message about starting `rbenv` automatically. Let's do that by adding something to our `.bash_profile` file:

```
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

Reload `.bash_profile` by running `source ~/.bash_profile` or by closing and reopening your terminal window.

### Usage

The following command will show you which versions of Ruby you have installed:

```
rbenv versions
```

Now we can install a version of Ruby and use that instead of the system Ruby:

```
rbenv install 2.5.0 # change this to your preferred version
rbenv global 2.5.0
```

Run `rbenv versions` again to make sure the version you want is being used.

To update rbenv itself, use:

```
brew upgrade rbenv ruby-build
```

[RubyGems](http://rubygems.org/), the Ruby package manager, was also installed:

```
which gem
```

Update to its latest version with:

```
gem update --system
```

To install a "gem" (Ruby package), run:

```
gem install <gemname>
```

To install without generating the documentation for each gem (faster):

```
gem install <gemname> --no-document
```

To see what gems you have installed:

```
gem list
```

To check if any installed gems are outdated:

```
gem outdated
```

To update all gems or a particular gem:

```
gem update [<gemname>]
```

RubyGems keeps old versions of gems, so feel free to do come cleaning after updating:

```
gem cleanup
```

## OpenSSL

[OpenSSL](https://www.openssl.org/) is an SSL toolkit and cryptography library which is included with macOS. The version included with macOS is often very old and can cause issues when used in our projects. We can fix this by installing a newer version with Homebrew and, if needed, creating a symlink to force the use of the Homebrew version over the system version.

```
brew update
brew install openssl
brew link --force openssl
```

Check the version number to make sure we aren't using an outdated or vulnerable version:

```
openssl version -a
```

If it's outdated, check to see whether the system openssl is still being used by typing

```
which openssl
```

If you see the system openssl, create a symlink that will for the system to use the Homebrew version:

```
ln -s /usr/local/Cellar/openssl/<version>/bin/openssl /usr/local/bin/openssl
```

Where `<version>` is the version number of the Homebrew openssl.

## MySQL

### Install

We will install [MySQL](http://www.mysql.com/) using Homebrew, which will also install some header files needed for MySQL bindings in different programming languages (MySQL-Python for one).

To install, run:

```
brew update # Always good to do
brew install mysql
```

### Usage

To start the MySQL server, we can use our nifty Homebrew Services:

```
brew services start mysql
```

To connect with the command-line client, run:

```
mysql -uroot
```

(Use `exit` to quit the MySQL shell.)

**Note**: By default, the MySQL user `root` has no password. It doesn't really matter for a local development database. If you wish to change it though, you can use `mysqladmin -u root password 'new-password'`.

### Database Management

To manage your MySQL databases, I recommend using [Sequel Pro](http://www.sequelpro.com), a MySQL management tool designed for macOS.

## GitHub Desktop

[GitHub Desktop](https://desktop.github.com/) is a GUI for git and allows you to work with git repositories on GitHub. Download it, unzip the archive, and copy GitHub Desktop into your Applications folder. Open it and login with your Github account.

When you clone a repository, make sure you are cloning it to the "Projects folder" you set up earlier.

## Sourcetree

[Sourcetree](https://www.sourcetreeapp.com) is another popular GUI for git that is used for repositories stored in Bitbucket. If your team is using Bitbucket, you may want to download this. To begin using, make sure you have set up an Atlassian account first.

When you clone a repository, make sure you are cloning it to the "Projects folder" you set up earlier. To set this up in Sourcetree, go to **Sourcetree > Preferences**. Under "Miscellaneous", set the project folder. It should now be the default.

## Google Cloud

Cloud development solutions such as [Amazon Web Services](https://aws.amazon.com/) or [Google Cloud](https://cloud.google.com/) make life a lot easier for a developer. I tend to use Google Cloud, so I'll provide instructions for what you'll need on your machine to get up and running with Google Cloud.

### Google Cloud SDK

[Google Cloud SDK](https://cloud.google.com/sdk/docs/) is what we'll be using to perform actions both locally and remotely on our Google Cloud projects. It includes command-line tools for interacting with Google Cloud projects, under the `gcloud` moniker, as well as a command-line tool for interacting with Google Cloud storage, under the `gsutil` moniker.

#### Install

Follow the instructions on the [Google Cloud SDK](https://cloud.google.com/sdk/docs/) docs page.

Because we'll be using Python mainly, let's install the [App Engine Python](https://cloud.google.com/appengine/docs/standard/python/download) component.

```
gcloud components install app-engine-python
```

We may end up needing the extras component as well, so let's install that too.

```
gcloud components install app-engine-python-extras
```

We can see which components we have installed by typing

```
gcloud components list
```

Now that we've installed our components, let's configure the SDK. First, authenticate your Google account for the SDK:

```
gcloud auth application-default login
```

Second, set some global SDK configuration options. Use the email address you just authenticated with below.

```
gcloud config set compute/region us-central1
gcloud config set core/account your_google_account@gmail.com
```

### Cloud SQL Proxy

If your project is using Cloud SQL, using [Cloud SQL Proxy](https://cloud.google.com/sql/docs/mysql/sql-proxy) makes it very easy to connect to your Cloud SQL database for viewing and management.

#### Install

Download the proxy and make it executable:

```
curl -o cloud_sql_proxy https://dl.google.com/cloudsql/cloud_sql_proxy.darwin.amd64
chmod +x cloud_sql_proxy
```

Copy it into a folder on your `$PATH` so that we can call it by using `cloud_sql_proxy` instead of `path/to/cloud_sql_proxy`:

```
cp cloud_sql_proxy /usr/local/bin
```

To use, open your Google Cloud project in the cloud console, navigate to **SQL**, select the instance you want to use, and copy the instance name. Substitute it into the following command:

```
cloud_sql_proxy -instances='<instance_name>=tcp:9906' # where <instance_name> is the instance name
```

Your Cloud SQL instance should now be available at localhost (127.0.0.1) at the TCP port 9906.

To open a connection in Sequel Pro, under "Quick Connect", you can type the following:

- **Host:** 127.0.0.1 or localhost
- **Username:** a valid MySQL user name for your instance
- **Password:** the password for the user
- **Port:** 9906

Click "Connect" to connect.

## PostgreSQL

[PostgreSQL](https://www.postgresql.org/) is a popular relational database.

Install PostgreSQL using Homebrew:

```
brew update # Always good to do
brew install postgresql
```

It will automatically add itself to Homebrew Services. Start it with:

```
brew services start postgresql
```

If you reboot your machine, PostgreSQL will be restarted at login.

## Other apps

Here is a quick list of some apps I use, which you might find useful as well:

- [1Password](https://agilebits.com/onepassword) or [Lastpass Password Manager](https://www.lastpass.com): Allows you to securely store your login and passwords. Even if you only use a few different passwords (they say you shouldn't!), this is really handy to keep track of all the accounts you sign up for! Also, these services have a mobile app so you always have all your passwords with you. Having access through multiple sources (web+mobile) will usually cost money, but there are free alternatives.
