# macOS Dev Setup

This document describes how I set up my developer environment on a new MacBook or iMac. We will set up popular programming languages (for example [Node](http://nodejs.org/) (JavaScript), [Python](http://www.python.org/), and [Ruby](http://www.ruby-lang.org/)). You may not need all of them for your projects, although I recommend having them set up as they always come in handy.

The document assumes you are new to Mac, but can also be useful if you are reinstalling a system and need some reminder. The steps below were tested on **macOS High Sierra** (10.13), but should work for more recent versions as well.

**Contributing**: If you find any mistakes in the steps described below, or if any of the commands are outdated, do let me know! For any other suggestions, please understand that this guide was originally written for some friends and as a personal reference for myself, and I'm trying to keep it simple. There are many valid ways to set up a developer environment, and this is just one of them.

- [System update](#system-update)
- [System preferences](#system-preferences)
- [Security](#security)
- [Google Chrome](#google-chrome)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [Git](#git)
- [Visual Studio Code](#visual-studio-code)
- [Vim](#vim)
- [Python](#python)
- [Node.js](#nodejs)
- [Ruby](#ruby)
- [Heroku](#heroku)
- [PostgreSQL](#postgresql)
- [Redis](#redis)
- [MongoDB](#mongodb)
- [Elasticsearch](#elasticsearch)
- [Projects folder](#projects-folder)
- [Apps](#apps)

## System update

First thing you need to do, on any OS actually, is update the system! For that: **Apple Icon > About This Mac** then **Software Update...**.

## System preferences

If this is a new computer, there are a couple tweaks I like to make to the System Preferences. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Preferences**:

- Trackpad > Tap to click
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)
- Dock > Automatically hide and show the Dock

## Security

I recommend checking that basic security settings are enabled. You will be happy to have done so if ever your Mac is lost or stolen.

In **Apple Icon > System Preferences**:

- Users & Groups: If you haven't already set a password for your user during the initial set up, you should do so now
- Security & Privacy > General: Require password immediately after sleep or screen saver begins (you can keep a grace period of a couple minutes if you prefer, but I like to know that my computer locks as soon as I close it)
- Security & Privacy > FileVault: Make sure FileVault disk encryption is enabled
- iCloud: If you haven't already done so during set up, enable Find My Mac

## Google Chrome

Install your favorite browser, mine happens to be Chrome.

Download from [www.google.com/chrome](https://www.google.com/chrome/). Open the **.dmg** file once it's done downloading (this will mount the disk image), and drag and drop the **Google Chrome** app into the Applications folder (on the Mac, most applications are installed this way). When done, you can unmount the disk in Finder (the small "eject" icon next to the disk under **Devices**).

## iTerm2

### Install

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one. Download and install [iTerm2](http://www.iterm2.com/).

In **Finder**, drag and drop the **iTerm** Application file into the **Applications** folder.

You can now launch iTerm, through the **Launchpad** for instance.

Let's just quickly change some preferences. In **iTerm2 > Preferences...**, under the tab **General**, uncheck **Confirm closing multiple sessions** and **Confirm "Quit iTerm2 (Cmd+Q)" command** under the section **Closing**.

In the tab **Profiles**, create a new one with the "+" icon, and rename it to your first name for example. Then, select **Other Actions... > Set as Default**. Under the section **General** set **Working Directory** to be **Reuse previous session's directory**. Finally, under the section **Window**, change the size to something better, like **Columns: 125** and **Rows: 35**.

When done, hit the red "X" in the upper left (saving is automatic in macOS preference panes). Close the window and open a new one to see the size change.

### Beautiful terminal

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place. What follows might seem like a lot of work, but trust me, it'll make the development experience so much better.

First let's add some color. There are many great color schemes out there, but if you don't know where to start you can try [Atom One Dark](https://github.com/nathanbuchar/atom-one-dark-terminal). Download the iTerm presets for the theme by running:

```
cd ~/Downloads
curl -o "Atom One Dark.itermcolors" https://raw.githubusercontent.com/nathanbuchar/atom-one-dark-terminal/master/scheme/iterm/One%20Dark.itermcolors
curl -o "Atom One Light.itermcolors" https://raw.githubusercontent.com/nathanbuchar/atom-one-dark-terminal/master/scheme/iterm/One%20Light.itermcolors
```

Then, in **iTerm2 Preferences**, under **Profiles** and **Colors**, go to **Color Presets... > Import...**, find and open the **Atom One Dark.itermcolors** file we just downloaded. Repeat these steps for **Atom One Light.itermcolors**.  Now open **Color Presets...** again and select **Atom One Dark** to activate the dark theme (or choose the light them if that's your preference).

Not a lot of colors yet. We need to tweak a little bit our Unix user's profile for that. This is done (on macOS and Linux), in the `~/.bash_profile` text file (`~` stands for the user's home directory).

We'll come back to the details of that later, but for now, just download the files [.bash_profile](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_profile), [.bash_prompt](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_prompt), [.aliases](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.aliases) attached to this document into your home directory (`.bash_profile` is the one that gets loaded, I've set it up to call the others):

```
cd ~
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_profile
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.bash_prompt
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.aliases
```

With that, open a new terminal tab (Cmd+T) and see the change! Try the list commands: `ls`, `ls -lh` (aliased to `ll`), `ls -lha` (aliased to `la`).

Now we have a terminal we can work with!

(Thanks to Mathias Bynens for his awesome [dotfiles](https://github.com/mathiasbynens/dotfiles).)

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for macOS is [Homebrew](http://brew.sh/).

### Install

An important dependency before Homebrew can work is the **Command Line Developer Tools** for **Xcode**. These include compilers that will allow you to build things from source. You can install them directly from the terminal with:

```
xcode-select --install
```

Once that is done, we can install Homebrew by copy-pasting the installation command from the [Homebrew homepage]([Homebrew](http://brew.sh/) inside the terminal:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
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

After installing a service (for example a database), it should automatically add itself to Homebrew Services. If not, you can add it manually with:

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

## Git

macOS comes with a pre-installed version of [Git](http://git-scm.com/), but we'll install our own through Homebrew to allow easy upgrades and not interfere with the system version. To do so, simply run:

```
brew update
brew install git
```

When done, to test that it installed fine you can run:

```
which git
```

The output should be `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig) file to your home directory:

```
cd ~
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitconfig
```

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

```
git config --global user.name "Your Name Here"
git config --global user.email "your_email@youremail.com"
```

They will get added to your `.gitconfig` file.

On a Mac, it is important to remember to add `.DS_Store` (a hidden macOS system file that's put in folders) to your project `.gitignore` files. You also set up a global `.gitignore` file, located for instance in your home directory (but you'll want to make sure any collaborators also do it):

```
cd ~
curl -O https://raw.githubusercontent.com/nicolashery/mac-dev-setup/master/.gitignore
git config --global core.excludesfile ~/.gitignore
```

## Visual Studio Code

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but if you're just getting started and looking for something simple that works, [Visual Studio Code](https://code.visualstudio.com/) is a pretty good option.

Go ahead and [download](https://code.visualstudio.com/Download) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

**Note**: At this point I'm going to create a shortcut on the macOS Dock for both for Visual Studio Code and iTerm. To do so, right-click on the running application and select **Options > Keep in Dock**.

Just like the terminal, let's configure our editor a little. Go to **Code > Preferences > Settings**. In the very top-right of the interface you should see an icon with brackets that appeared **{ }** (on hover, it should say "Open Settings (JSON)"). Click on it, and paste the following:

```json
{
  "editor.tabSize": 2,
  "editor.rulers": [80],
  "files.insertFinalNewline": true,
  "files.trimTrailingWhitespace": true,
  "workbench.editor.enablePreview": false
}
```

Feel free to tweak these to your preference. When done, save the file and close it.

Pasting the above JSON snippet was handy to quickly customize things, but for further setting changes feel free to search in the "Settings" panel that opened first (shortcut **Cmd+,**). When your happy with your setup, you can save the JSON to quickly restore it on a new machine.

If you remember only one keyboard shortcut in VS Code, it should be **Cmd+Shift+P**. This opens the **Command Palette**, from which you can run pretty much anything.

Let's open the command palette now, and search for `Shell Command: Install 'code' command in PATH`. Hit enter when it shows up. This will install the command-line tool `code` to quickly open VS Code from the terminal. When in a projects directory, you'll be able to run:

```
cd myproject/
code .
```

VS Code is very extensible. To customize it further, open the **Extensions** tab on the left.

Let's do that now to customize the color of our editor. Search for the [Atom One Dark Theme](https://marketplace.visualstudio.com/items?itemName=akamud.vscode-theme-onedark) extension, select it and click **Install**. Repeat this for the [Atom One Light Theme](https://marketplace.visualstudio.com/items?itemName=akamud.vscode-theme-onelight).

Finally, activate the theme by going to **Code > Preferences > Color Theme** and selecting **Atom One Dark** (or **Atom One Light** if that is your preference).

## Vim

Although Sublime Text will be our main editor, it is a good idea to learn some very basic usage of [Vim](http://www.vim.org/). It is a very popular text editor inside the terminal, and is usually pre-installed on any Unix system.

For example, when you run a Git commit, it will open Vim to allow you to type the commit message.

I suggest you read a tutorial on Vim. Grasping the concept of the two "modes" of the editor, **Insert** (by pressing `i`) and **Normal** (by pressing `Esc` to exit Insert mode), will be the part that feels most unnatural. Also, it is good to know that typing `:x` when in Normal mode will save and exit. After that, it's just remembering a few important keys.

Vim's default settings aren't great, and you could spend a lot of time tweaking your configuration (the `.vimrc` file). But if you just use Vim occasionally, you'll be happy to know that [Tim Pope](https://github.com/tpope) has put together some sensible defaults to quickly get started.

Using Vim's built-in package support, install these settings by running:

```
mkdir -p ~/.vim/pack/tpope/start
cd ~/.vim/pack/tpope/start
git clone https://tpope.io/vim/sensible.git
```

With that, Vim will look a lot better next time you open it!

## Python

macOS, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version using [pyenv](https://github.com/yyuu/pyenv). This will also allow us to manage multiple versions of Python (ex: 2.7 and 3) should we need to.

Install `pyenv` via Homebrew by running:

```
brew install pyenv
```

When finished, you should see instructions to add something to your profile. Open your `.bash_profile` in the home directory (you can use `$ code ~/.bash_profile`), and add the following line:

```bash
if command -v pyenv 1>/dev/null 2>&1; then eval "$(pyenv init -)"; fi
```

Save the file and reload it with:

```
source ~/.bash_profile
```

Before installing a new Python version, the [pyenv wiki](https://github.com/pyenv/pyenv/wiki) recommends having a few dependencies available:

```
brew install openssl readline sqlite3 xz zlib
```

We can now list all available Python versions by running:

```
pyenv install --list
```

Look for the latest 3.x version (or 2.7.x), and install it (replace the `.x.x` with actual numbers):

```
pyenv install 3.x.x
```

List the Python versions you have locally with:

```
pyenv versions
```

The star (`*`) should indicate we are still using the `system` version, which is the default. We recommend leaving it as the default as some [Node.js](https://nodejs.org/en/) packages will use it in their installation process.

You can switch your current terminal to another Python version with:

```
pyenv global 3.x.x
```

You should now see that version when running:

```
python --version
```

If in a project directory, you can use:

```
pyenv local 3.x.x
```

This will save that project's Python version to a `.python-version` file. Next time you enter the project's directory from a terminal, `pyenv` will automatically load that version for you.

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

Now, let's say you have a project called `myproject`. You can set up virtualenv for that project and the Python version it uses (replace `3.x.x` with the version you want):

```
pyenv virtualenv 3.x.x myproject
```

See the list of virtualenvs you created with:

```
pyenv virtualenvs
```

To use your project's virtualenv, you need to **activate** it first (in every terminal where you are working on your project):

```
pyenv activate myproject
```

If you run `pyenv virtualenvs` again, you should see a star (`*`) next to the active virtualenv.

Now when you install something:

```
pip install <package>
```

It will get installed in that virtualenv's folder, and not conflict with other projects.

You can also set your project's `.python-version` to point to a virtualenv your created:

```
pyenv local myproject
```

Next time you enter that project's directory, `pyenv` will automatically load the virtualenv for you.

### Anaconda and Miniconda

The Anaconda/Miniconda distributions of Python come with many useful tools for scientific computing.

You can install them using `pyenv`, for example (replace `x.x.x` with an actual version number):

```
pyenv install miniconda3-x.x.x
```

You can create [conda](https://docs.conda.io/) environments after loading the Python distribution into your shell:

```
pyenv shell miniconda3-x.x.x
conda create --name  mycondaproject
conda activate mycondaproject
```

Install packages, for example the [Jupyter Notebook](https://jupyter.org/), using:

```
conda install jupyter
```

You should now be able to run the notebook:

```
jupyter notebook
```

Deactivate the environment, and return to the default Python version with:

```
conda deactivate
pyenv shell --unset
```

## Node.js

The recommended way to install [Node.js](http://nodejs.org/) is to use [nvm](https://github.com/creationix/nvm) (Node Version Manager) which allows you to manage multiple versions of Node.js on the same machine.

Install nvm by copy-pasting the [install script command](https://github.com/creationix/nvm#install--update-script) into your terminal.

Once that is done, open a new terminal and verify that it was installed correctly by running:

```
command -v nvm
```

View the all available stable versions of Node with:

```
nvm ls-remote --lts
```

Install the latest stable version with:

```
nvm install node
```

It will also set this version as your default version. You can install another specific version, for example Node 10, with:

```
nvm install 10
```

And switch between versions by using:

```
nvm use 10
nvm use default
```

See which versions you have install with:

```
nvm ls
```

In a project's directory you can create a `.nvmrc` file containing the Node.js version the project uses, for example:

```
echo "10" > .nvmrc
```

Next time you enter the project's directory from a terminal, you can load the correct version of Node.js by running:

```
nvm use
```

### npm

Installing Node also installs the [npm](https://npmjs.org/) package manager.

To install a package:

```
npm install <package> # Install locally
npm install -g <package> # Install globally
```

To install a package and save it in your project's `package.json` file:

```
npm install --save <package>
```

To see what's installed:

```
npm list --depth 1 # local
npm list -g --depth 1 # global
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
npm uninstall --save <package>
```

## Ruby

Like Python, [Ruby](http://www.ruby-lang.org/) is already installed on Unix systems. But we don't want to mess around with that installation. More importantly, we want to be able to use the latest version of Ruby.

### Install

When installing Ruby, best practice is to use [RVM](https://rvm.io/) (Ruby Version Manager) which allows you to manage multiple versions of Ruby on the same machine. Installing RVM, as well as the latest version of Ruby, is very easy. Just run:

```
curl -sSL https://get.rvm.io | bash -s stable --ruby
```

When it is done, both RVM and a fresh version of Ruby are installed.

Open a new terminal and run:

```
type rvm | head -1
```

You should get the output `rvm is a function`.

### Usage

The following command will show you which versions of Ruby you have installed:

```
rvm list
```

The one that was just installed, Ruby 2, should be set as default. When managing multiple versions, you switch between them with:

```
rvm use system # Switch back to system install (ex: 2.0)
rvm use 2.3 --default # Switch to 2.3 and sets it as default
```

Run the following to make sure the version you want is being used:

```
which ruby
ruby --version
```

You can install another version with:

```
rvm install 2.2
```

To update RVM itself, use:

```
rvm get stable
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

## Heroku

[Heroku](http://www.heroku.com/), if you're not already familiar with it, is a [Platform-as-a-Service](http://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) that makes it really easy to deploy your apps online. There are other similar solutions out there, but Heroku was among the first and is currently the most popular. Not only does it make a developer's life easier, but I find that having Heroku deployment in mind when building an app forces you to follow modern app development [best practices](http://www.12factor.net/).

Assuming that you have an account (sign up if you don't), let's install the [Heroku CLI](https://devcenter.heroku.com/categories/command-line). Heroku offers a macOS installer, the [Heroku Toolbelt](https://toolbelt.heroku.com/), that includes the client. But for these kind of tools, I prefer using Homebrew. It allows us to keep better track of what we have installed. Luckily for us, Homebrew includes a `heroku-toolbelt` formula:

```
brew install heroku-toolbelt
```

Login to your Heroku account using your email and password:

```
heroku login
```

Once logged-in, you're ready to deploy apps! Heroku has a great [Getting Started](https://devcenter.heroku.com/start) guides for different languages, so I'll let you refer to that. Heroku uses Git to push code for deployment, so make sure your app is under Git version control. A quick cheat sheet (if you've used Heroku before):

```
cd myapp/
heroku create myapp
git push heroku master
heroku ps
heroku logs -t
```

The [Heroku Dev Center](https://devcenter.heroku.com/) is full of great resources, so be sure to check it out!

## PostgreSQL

[PostgreSQL](https://www.postgresql.org/) is a popular relational database, and Heroku has first-class support for it.

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

## Redis

[Redis](http://redis.io/) is a blazing fast, in-memory, key-value store, that uses the disk for persistence. It's kind of like a NoSQL database, but there are a lot of [cool things](http://oldblog.antirez.com/post/take-advantage-of-redis-adding-it-to-your-stack.html) that you can do with it that would be hard or inefficient with other database solutions. For example, it's often used as session management or caching by web apps, but it has many other uses.

To install Redis, use Homebrew:

```
brew update
brew install redis
```

Start it through Homebrew Services with:

```
brew services start redis
```

I'll let you refer to Redis' [documentation](http://redis.io/documentation) or other tutorials for more information.

## MongoDB

[MongoDB](http://www.mongodb.org/) is a popular [NoSQL](http://en.wikipedia.org/wiki/NoSQL) database.

Installing it is very easy through Homebrew:

```
brew update
brew install mongo
```

Start it through Homebrew Services with:

```
brew services start mongo
```

I'll let you refer to MongoDB's [Getting Started](http://docs.mongodb.org/manual/tutorial/getting-started/) guide for more!

## Elasticsearch

As it says on the box, [Elasticsearch](http://www.elasticsearch.org/) is a "powerful open source, distributed real-time search and analytics engine". It uses an HTTP REST API, making it really easy to work with from any programming language.

You can use elasticsearch for such cool things as real-time search results, autocomplete, recommendations, machine learning, and more.

### Install

Elasticsearch runs on Java, so check if you have it installed by running:

```bash
java -version
```

If Java isn't installed yet, a window will appear prompting you to install it. Go ahead and click "Install".

Next, install elasticsearch with:

```bash
$ brew install elasticsearch
```

**Note**: Elasticsearch also has a `plugin` program that gets moved to your `PATH`. I find that too generic of a name, so I rename it to `elasticsearch-plugin` by running (will need to do that again if you update elasticsearch):

```bash
$ mv /usr/local/bin/plugin /usr/local/bin/elasticsearch-plugin
```

Below I will use `elasticsearch-plugin`, just replace it with `plugin` if you haven't followed this step.

As you guessed, you can add plugins to elasticsearch. A popular one is [elasticsearch-head](http://mobz.github.io/elasticsearch-head/), which gives you a web interface to the REST API. Install it with:

```bash
$ elasticsearch-plugin --install mobz/elasticsearch-head
```

### Usage

Start a local elasticsearch server with:

```bash
$ elasticsearch -f
```

(The `-f` option tells it to run in the foreground, so you can stop it with `Ctrl+C`.)

Test that the server is working correctly by running:

```bash
$ curl -XGET 'http://localhost:9200/'
```

If you installed the elasticsearch-head plugin, you can visit its interface at `http://localhost:9200/_plugin/head/`.

Elasticsearch's [documentation](http://www.elasticsearch.org/guide/) is more of a reference. To get started, I suggest reading some of the blog posts linked on this [StackOverflow answer](http://stackoverflow.com/questions/11593035/beginners-guide-to-elasticsearch/11767610#11767610).

## Projects folder

This really depends on how you want to organize your files, but I like to put all my version-controlled projects in `~/Projects`. Other documents I may have, or things not yet under version control, I like to put in `~/Dropbox` (if you have Dropbox installed), or `~/Documents`.

## Apps

Here is a quick list of some apps I use, and that you might find useful as well:

- [Dropbox](https://www.dropbox.com/): File syncing to the cloud. I put all my documents in Dropbox. It syncs them to all my devices (laptop, mobile, tablet), and serves as a backup as well! **(Free for 2GB)**
- [Google Drive](https://drive.google.com/): File syncing to the cloud too! I use Google Docs a lot to collaborate with others (edit a document with multiple people in real-time!), and sometimes upload other non-Google documents (pictures, etc.), so the app comes in handy for that. **(Free for 5GB)**
- [1Password](https://agilebits.com/onepassword): Allows you to securely store your login and passwords. Even if you only use a few different passwords (they say you shouldn't!), this is really handy to keep track of all the accounts you sign up for! Also, they have a mobile app so you always have all your passwords with you (syncs with Dropbox). A little pricey though. There are free alternatives. **($50 for Mac app, $18 for iOS app)**
- [Marked](http://markedapp.com/): As a developer, most of the stuff you write ends up being in [Markdown](http://daringfireball.net/projects/markdown/). In fact, this `README.md` file (possibly the most important file of a GitHub repo) is indeed in Markdown, written in Sublime Text, and I use Marked to preview the results everytime I save. **($4)**
- [Path Finder](http://cocoatech.com/pathfinder/): I love OSX, it's Unix so great for developers, and all of it just works and looks pretty! Only thing I "miss" from Windows (OMG what did he say?), is a decent file explorer. I think Finder is a pain to use. So I gladly paid for this alternative, but I understand others might find it expensive just to not have to use Finder. **($40)**
- [Evernote](https://evernote.com/): If I don't write something down, I'll forget it. As a developer, you learn so many new things every day, and technology keeps changing, it would be insane to want to keep it all in your head. So take notes, sync them to the cloud, and have them on all your devices. To be honest, I switched to [Simplenote](http://simplenote.com/) because I only take text notes, and I got tired of Evernote putting extra spaces between paragraphs when I copy & pasted into other applications. Simplenote is so much better for text notes (and it supports Markdown!). **(Both are free)**
- [Moom](http://manytricks.com/moom/): Don't waste time resizing and moving your windows. Moom makes this very easy. **($10)**




