# Mac OS X Dev Setup

This document describes how I set up my developer environment on a new MacBook or iMac. We will set up [Python](http://www.python.org/), [Node](http://nodejs.org/) (JavaScript), and [Ruby](http://www.ruby-lang.org/) environments, mainly for JavaScript development. Even if you don't program in all three, it is good to have them as many command-line tools use one of them. As you read  and follow these steps, feel free to send me any feedback or comments you may have.

The document assumes you are new to Mac. The steps below were tested on **OS X Mountain Lion**.

- [System update](#system-update)
- [System preferences](#system-preferences)
- [Google Chrome](#google-chrome)
- [iTerm2](#iterm2)
- [Homebrew](#homebrew)
- [Beautiful terminal](#beautiful-terminal)
- [Dotfile source control](#dotfile-source-control)
- [Git](#git)
- [Sublime Text](#sublime-text)
- [Customize Sublime Text Packages](#customize-sublime-text-packages)
- [Python](#python)
- [Node.js](#nodejs)
- [Ruby and rbenv](#ruby-and-rbenv)
- [Heroku](#heroku)
- [Projects folder](#projects-folder)
- [Vagrant](#vagrant)
- [Virtual Box](#virtual-box)
- [Adobe Apps](#adobe-apps)
- [Apps](#apps)
- [Style guides for additional reading](#style-guides)

## System update

First thing you need to do, on any OS acutally, is update the system! For that: **Apple Icon > Software Update...**

## System preferences

If this is a new computer, there are a couple tweaks I like to make to the System Preferences. Feel free to follow these, or to ignore them, depending on your personal preferences.

In **Apple Icon > System Preferences**:

- Trackpad > Tap to click
- Keyboard > Key Repeat > Fast (all the way to the right)
- Keyboard > Delay Until Repeat > Short (all the way to the right)
- Dock > Automatically hide and show the Dock

## Google Chrome

Install your favorite browser, mine happens to be Chrome.

Download from [www.google.com/chrome](https://www.google.com/intl/en/chrome/browser/). Open the **.dmg** file once it's done downloading (this will mount the disk image), and drag and drop the **Google Chrome** app into the Applications folder (on the Mac, most applications are installed this way). When done, you can unmount the disk in Finder (the small "eject" icon next to the disk under **Devices**).

## iTerm2

Since we're going to be spending a lot of time in the command-line, let's install a better terminal than the default one. Download and install [iTerm2](http://www.iterm2.com/) (the newest version, even if it says "beta release").

In **Finder**, drag and drop the **iTerm** Application file into the **Applications** folder.

You can now launch iTerm, through the **Launchpad** for instance.

Let's just quickly change some preferences. In **iTerm > Preferences...**, under the tab **General**, uncheck **Confirm closing multiple sessions** and **Confirm "Quit iTerm2 (Cmd+Q)" command** under the section **Closing**.

In the tab **Profiles**, create a new one with the "+" icon, and rename it to your first name for example. Then, select **Other Actions... > Set as Default**. Finally, under the section **Window**, change the size to something better, like **Columns: 125** and **Rows: 35**.

When done, hit the red "X" in the upper left (saving is automatic in OS X preference panes). Close the window and open a new one to see the size change.

## Homebrew

Package managers make it so much easier to install and update applications (for Operating Systems) or libraries (for programming languages). The most popular one for OS X is [Homebrew](http://mxcl.github.com/homebrew/).

### Install

In the terminal paste the following line
(without the `$`), hit **Enter**, and follow the steps on the screen:

    $ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

You'll be prompted to install **Command Line Tools** for **Xcode**. When the installation of Command Line Tools is complete, Homebrew will continue installing.

One thing we need to do is tell the system to use programs installed by Homebrew (in `/usr/local/bin`) rather than the OS default if it exists. We do this by adding `/usr/local/bin` to your `$PATH` environment variable:

    $ echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.bash_profile

Open an new terminal tab with **Cmd+T** (you should also close the old one), then run the following command to make sure everything works:

    $ brew doctor

### Usage

To install a package (or **Formula** in Homebrew vocabulary) simply type:

    $ brew install <formula>

To update Homebrew's directory of formulae, run:

    $ brew update

**Note**: I've seen that command fail sometimes because of a bug. If that ever happens, run the following (when you have Git installed):

    $ cd /usr/local
    $ git fetch origin
    $ git reset --hard origin/master

To see if any of your packages need to be updated:

    $ brew outdated

To update a package:

    $ brew upgrade <formula>

Homebrew keeps older versions of packages installed, in case you want to roll back. That rarely is necessary, so you can do some cleanup to get rid of those old versions:

    $ brew cleanup

To see what you have installed (with their version numbers):

    $ brew list --versions

## Bash Completion

    $ brew install bash-completion

## Use OSX Finder Quicklook to preview all plain text files

- [OSX Finder Quicklook](https://coderwall.com/p/dlithw)


## Beautiful terminal

Since we spend so much time in the terminal, we should try to make it a more pleasant and colorful place. What follows might seem like a lot of work, but trust me, it'll make the development experience so much better.

Let's go ahead and start by changing the font. In **iTerm > Preferences...**, under the tab **Profiles**, section **Text**, change both fonts to **Consolas 13pt**.

(Note: you may have to install Consolas, instructions for doing so can be found [here](http://www.zjhzxhz.com/2014/01/install-microsofts-consolas-font-on-mac-os-x/))

Now let's add some color. I'm a big fan of the [Solarized](http://ethanschoonover.com/solarized) color scheme. It is supposed to be scientifically optimal for the eyes. I just find it pretty.

Scroll down the page and download the latest version. Unzip the archive. In it you will find the `iterm2-colors-solarized` folder with a `README.md` file, but I will just walk you through it here:

- In **iTerm2 Preferences**, under **Profiles** and **Colors**, go to **Load Presets... > Import...**, find and open the two **.itermcolors** files we downloaded.
- Go back to **Load Presets...** and select **Solarized Dark** to activate it. Voila!

**Note**: You don't have to do this, but there is one color in the **Solarized Dark** preset I don't agree with, which is *Bright Black*. You'll notice it's too close to *Black*. So I change it to be the same as *Bright Yellow*, i.e. **R 83 G 104 B 112**. Skip this step if installing [Solarized](http://ethanschoonover.com/solarized/vim-colors-solarized) for **Vim** as it will make the background too bright for the text.

Not a lot of colors yet. We need to tweak a little bit our Unix user's profile for that. This is done (on OS X and Linux), in the `~/.bash_profile` text file (`~` stands for the user's home directory).

We'll come back to the details of that later, but for now, just download the files [.bash_profile](https://github.com/nicolahery/mac-dev-setup/blob/master/.bash_profile), [.bash_prompt](https://github.com/nicolahery/mac-dev-setup/blob/master/.bash_prompt), [.aliases](https://github.com/nicolahery/mac-dev-setup/blob/master/.aliases) attached to this document into your home directory (`.bash_profile` is the one that gets loaded, I've set it up to call the others):

    $ cd ~
    $ curl -O https://raw.github.com/jkatsnelson/mac-dev-setup-js/master/.bash_profile
    $ curl -O https://raw.github.com/jkatsnelson/mac-dev-setup-js/master/.bash_prompt
    $ curl -O https://raw.github.com/jkatsnelson/mac-dev-setup-js/master/.aliases

With that, open a new terminal tab (Cmd+T) and see the change! Try the list commands: `ls`, `ls -lh` (aliased to `ll`), `ls -lha` (aliased to `la`).

At this point you can also change your computer's name, which shows up in this terminal prompt. If you want to do so, go to **System Preferences** > **Sharing**. For example, I changed mine from "Nicolas's MacBook Air" to just "MacBook Air", so it shows up as `MacBook-Air` in the terminal.

Now we have a terminal we can work with!

### Terminal word navigation sanity
To enable whole-word skipping while holding down alt plus the left or right arrow keys,
Go to iTerm preferences > Profiles and choose your user profile.
Then select the 'Keys' tab.

Add the following two shortcuts:

```
Keyboard Shortcut: alt + left arrow
Action: send escape sequence
Enter the letter 'b' in the text field next to 'Esc +'
```

```
Keyboard Shortcut: alt + right arrow
Action: send escape sequence
Enter 'f' in the text field next to 'Esc +'
```

(Thanks to Mathias Bynens for his awesome [dotfiles](https://github.com/mathiasbynens/dotfiles).)

## Dotfile source control

It is recommended that you place your workstation configuration dotfiles under source control, as explained [here](https://github.com/palimpsests/dotfiles) (ignore the 'Setting up a new Mac section').

## Git

What's a developer without [Git](http://git-scm.com/)? To install, simply run:

    $ brew install git

When done, to test that it installed fine you can run:

    $ git --version

And `$ which git` should output `/usr/local/bin/git`.

Let's set up some basic configuration. Download the [.gitconfig](/nicolahery/mac-dev-setup/blob/master/.gitconfig) file to your home directory:

    $ cd ~
    $ curl -O https://raw.github.com/nicolahery/mac-dev-setup/master/.gitconfig

It will add some color to the `status`, `branch`, and `diff` Git commands, as well as a couple aliases. Feel free to take a look at the contents of the file, and add to it to your liking.

Next, we'll define your Git user (should be the same name and email you use for [GitHub](https://github.com/) and [Heroku](http://www.heroku.com/)):

    $ git config --global user.name "Your Name Here"
    $ git config --global user.email "your_email@youremail.com"

They will get added to your `.gitconfig` file.

To push code to your GitHub repositories, we're going to use the recommended HTTPS method (versus SSH). So you don't have to type your username and password everytime, let's enable Git password caching as described [here](https://help.github.com/articles/set-up-git):

    $ git config --global credential.helper osxkeychain

**Note**: On a Mac, it is important to remember to add `.DS_Store` (a hidden OS X system file that's put in folders) to your `.gitignore` files. You can take a look at this repository's [.gitignore](/nicolahery/mac-dev-setup/blob/master/.gitignore) file for inspiration.

## Sublime Text

With the terminal, the text editor is a developer's most important tool. Everyone has their preferences, but unless you're a hardcore [Vim](http://en.wikipedia.org/wiki/Vim_(text_editor)) user, a lot of people are going to tell you that [Sublime Text](http://www.sublimetext.com/) is currently the best one out there.

Go ahead and [download](http://www.sublimetext.com/) it. Open the **.dmg** file, drag-and-drop in the **Applications** folder, you know the drill now. Launch the application.

**Note**: At this point I'm going to create a shorcut on the OS X Dock for both for Sublime Text and iTerm. To do so, right-click on the running application and select **Options > Keep in Dock**.

Sublime Text is not free, but I think it has an unlimited "evaluation period". Anyhow, we're going to be using it so much that even the seemingly expensive $60 price tag is worth every penny. If you can afford it, I suggest you [support](http://www.sublimetext.com/buy) this awesome tool. :)

Just like the terminal, let's configure our editor a little. Go to **Sublime Text 2 > Preferences > Settings - User** and paste the following in the file that just opened:

```json
{
    "font_face": "Consolas",
    "font_size": 13,
    "rulers":
    [
        79
    ],
    "highlight_line": true,
    "bold_folder_labels": true,
    "highlight_modified_tabs": true,
    "tab_size": 2,
    "translate_tabs_to_spaces": true,
    "word_wrap": false,
    "indent_to_bracket": true
}
```

Feel free to tweak these to your preference. When done, save the file and close it.

I use tab size 2 for everything except Python and Markdown files, where I use tab size 4. If you have a Python and Markdown file handy (or create dummy ones with `$ touch dummy.py`), for each one, open it and go to **Sublime Text 2 > Preferences > Settings - More > Syntax Specific - User** to paste in:

```json
{
    "tab_size": 4
}
```

Now for the color. I'm going to change two things: the **Theme** (which is how the tabs, the file explorer on the left, etc. look) and the **Color Scheme** (the colors of the code). Again, feel free to pick different ones, or stick with the default.

A popular Theme is the [Soda Theme](https://github.com/buymeasoda/soda-theme). To install it, run:

    $ cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/
    $ git clone https://github.com/buymeasoda/soda-theme/ "Theme - Soda"

Then go to **Sublime Text 2 > Preferences > Settings - User** and add the following two lines:

    "theme": "Soda Dark.sublime-theme",
    "soda_classic_tabs": true

Restart Sublime Text for all changes to take affect (Note: on the Mac, closing all windows doesn't close the application, you need to hit **Cmd+Q**).

The Soda Theme page also offers some [extra color schemes](https://github.com/buymeasoda/soda-theme#syntax-highlighting-colour-schemes) you can download and try. But to be consistent with my terminal, I like to use the **Solarized** Color Scheme, which already ships with Sublime Text. To use it, just go to **Sublime Text 2 > Preferences > Color Scheme > Solarized (Dark)**. Again, this is really according to personal flavors, so pick what you want.

Sublime Text 2 already supports syntax highlighting for a lot of languages. I'm going to install a couple that are missing:

    $ cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/
    $ git clone https://github.com/jashkenas/coffee-script-tmbundle CoffeeScript
    $ git clone https://github.com/miksago/jade-tmbundle Jade
    $ git clone https://github.com/danro/LESS-sublime.git LESS
    $ git clone -b SublimeText2 https://github.com/kuroir/SCSS.tmbundle.git SCSS
    $ git clone https://github.com/nrw/sublime-text-handlebars Handlebars

Let's create a shortcut so we can launch Sublime Text from the command-line:

    $ cd ~
    $ mkdir bin
    $ ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" ~/bin/subl

Now I can open a file with `$ subl myfile.py` or start a new project in the current directory with `$ subl .`. Pretty cool. (Note: the path to the apps shared support folder will change depending on the version of sublime.)

## Sublime Text Packages to install:
Install the [Sublime Package Control](http://wbond.net/sublime_packages/package_control/installation). 

To install individual packages, bring up the Command Palette with Cmd+Shift+P and type "install."  Select "Package Control: Install Package." Once the repositories load search for and install the following packages:

- Dust
- Sass
- Chai completions
- Chef
- CoffeeLinter
- Backbone
- Backbone Marionette
- Color Highlighter
- Emmet
- Sublime Linter
- Plain Tasks

## Customize Sublime Text Packages

It's recommended to style PlainTasks to something more palatable than the default yellow sticky color. 

Download the following Plain Tasks theme [here](https://github.com/monsoonco/mac-dev-setup-js/blob/master/DarkNotes.tmTheme) and add it to the folder ~/Library/Application Support/Sublime Text 2/Packages/PlainTasks

Navigate to Preferences -> Package Settings -> Plain Tasks -> Settings - User and update the config file to contain:

    {
    ...
        "color_scheme": "Packages/PlainTasks/DarkNotes.tmTheme"
    ...
    }

## Python

OS X, like Linux, ships with [Python](http://python.org/) already installed. But you don't want to mess with the system Python (some system tools rely on it, etc.), so we'll install our own version with Homebrew. It will also allow us to get the very latest version of Python 2.7.

The following command will install Python 2.7 and any dependencies required (it can take a few minutes to build everything):

    $ brew install python

When finished, you should get a summary in the terminal. Running `$ which python` should output `/usr/local/bin/python`.

It also installed [Pip]() (and its dependency [Distribute]()), which is the package manager for Python. Let's upgrade them both:

    $ pip install --upgrade setuptools
    $ pip install --upgrade distribute
    $ pip install --upgrade pip

Executable scripts from Python packages you install will be put in `/usr/local/share/python`, so let's add it to the `$PATH`. To do so, we'll create a `.path` text file in the home directory (I've already set up `.bash_profile` to call this file):

    $ cd ~
    $ subl .path

And add these lines to `.path`:

```bash
PATH=/usr/local/share/python:$PATH
export PATH
```

Save the file and open a new terminal to take the new `$PATH` into account (everytime you open a terminal, `.bash_profile` gets loaded).

### Pip Usage

Here are a couple Pip commands to get you started. To install a Python package:

    $ pip install <package>

To upgrade a package:

    $ pip install --upgrade <package>

To see what's installed:

    $ pip freeze

To uninstall a package:

    $ pip uninstall <package>


## Node.js

### Node Version Manager ([NVM](https://github.com/creationix/nvm))

Simple bash script to manage multiple active node.js versions

### Install

If you have git installed, then just clone the repo (More info here: [https://github.com/creationix/nvm#manual-install](https://github.com/creationix/nvm#manual-install):

    git clone https://github.com/creationix/nvm.git ~/.nvm

To activate nvm, you need to source it from your bash shell

    source ~/.nvm/nvm.sh

I always add this line to my ~/.bashrc or ~/.profile file (you may need to add this line to ~/.bash_profile file instead) to have it automatically sourced upon login. Often I also put in a line to use a specific version of node.

Use nvm to install the latest version of node

    nvm install 0.10

Node modules are installed locally in the `node_modules` folder of each project by default, but there are at least a few that are worth installing globally. Those are [CoffeeScript](http://coffeescript.org/), [Grunt](http://gruntjs.com/), [Bower](http://bower.io/), and [Yeoman](http://yeoman.io/):

    $ npm install -g coffee-script
    $ npm install -g grunt-cli
    $ npm install -g bower
    $ npm install -g yo

### NPM usage

To install a package:

    $ npm install <package> # Install locally
    $ npm install -g <package> # Install globally

To install a package and save it in your project's `package.json` file:

    $ npm install <package> --save

To see what's installed:

    $ npm list # Local
    $ npm list -g # Global

To find outdated packages (locally or globally):

    $ npm outdated [-g]

To upgrade all or a particular package:

    $ npm update [<package>]

To uninstall a package:

    $ npm uninstall <package>

## Ruby and rbenv
Like Python, [Ruby](http://www.ruby-lang.org/) is already installed on Unix systems. But we don't want to mess around with that installation. More importantly, we want to be able to use the latest version of Ruby.

### Install

When installing Ruby, best practice is to use [rbenv](https://github.com/sstephenson/rbenv) (Ruby Version Manager) which allows you to manage multiple versions of Ruby on the same machine. Follow the steps [here](https://github.com/sstephenson/rbenv#homebrew-on-mac-os-x) to checkout and install rbenv. Also, install the [rbenv-gem-rehash](https://github.com/sstephenson/rbenv-gem-rehash) plugin.

Compatibility note: rbenv is incompatible with RVM. Please make sure to fully uninstall RVM and remove any references to it from your shell initialization files before installing rbenv.

You are encouraged to install `ruby-build` using `brew install ruby-build` for managing the installation of new versions of ruby.  Using `ruby-build` and `bash-completion`, installing a version of Ruby is as easy as typing `rbenv install`, then pressing tab twice to get a list of all available versions.

For example, install the latest version of 2.0.X:

    $ rbenv install 2.0.X-pXXX
    
To use this version globally now run..

    $ rbenv global 2.0.X-pXXX


[RubyGems](http://rubygems.org/), the Ruby package manager, was also installed:

    $ which gem

Update to its latest version with:

    $ gem update --system

To install a "gem" (Ruby package), run:

    $ gem install <gemname>

To install without generating the documentation for each gem (faster):

    $ gem install <gemname> --no-rdoc --no-ri

To see what gems you have installed:

    $ gem list

To check if any installed gems are outdated:

    $ gem outdated

To update all gems or a particular gem:

    $ gem update [<gemname>]

RubyGems keeps old versions of gems, so feel free to do come cleaning after updating:

    $ gem cleanup

I mainly use Ruby for the CSS pre-processor [Compass](http://compass-style.org/), which is built on top of [Sass](http://sass-lang.com/):

    $ gem install compass --no-rdoc --no-ri

### Gems to install:

    $ gem install compass --no-rdoc --no-ri
    $ gem install berkshelf --no-rdoc --no-ri
    $ gem install chef --no-rdoc --no-ri

### Install rbenv-gem-rehash

Never run rbenv rehash again. This rbenv plugin automatically runs rbenv rehash every time you install or uninstall a gem. Make sure you have rbenv 0.4.0 or later, then run:

    $ git clone https://github.com/sstephenson/rbenv-gem-rehash.git ~/.rbenv/plugins/rbenv-gem-rehash

More details [here](https://github.com/sstephenson/rbenv-gem-rehash).

## Heroku

[Heroku](http://www.heroku.com/), if you're not already familiar with it, is a [Platform-as-a-Service](http://en.wikipedia.org/wiki/Platform_as_a_service) (PaaS) that makes it really easy to deploy your apps online. There are other similar solutions out there, but Heroku was among the first and is currently the most popular. Not only does it make a developer's life easier, but I find that having Heroku deployment in mind when building an app forces you to follow modern app development [best practices](http://www.12factor.net/).

### Install

Assuming that you have an account (sign up if you don't), let's install the [Heroku Client](https://devcenter.heroku.com/articles/using-the-cli) for the command-line. Heroku offers a Mac OS X installer, the [Heroku Toolbelt](https://toolbelt.heroku.com/), that includes the client. But for these kind of tools, I prefer using Homebrew. It allows us to keep better track of what we have installed. Luckily for us, Homebrew includes a `heroku-toolbelt` formula:

    $ brew install heroku-toolbelt

The formula might not have the latest version of the Heroku Client, which is updated pretty often. Let's update it now:

    $ heroku update

Don't be afraid to run `heroku update` every now and then to always have the most recent version.

### Usage

Login to your Heroku account using your email and password:

    $ heroku login

If this is a new account, and since you don't already have a public **SSH key** in your `~/.ssh` directory, it will offer to create one for you. Say yes! It will also upload the key to your Heroku account, which will allow you to deploy apps from this computer.

If it didn't offer create the SSH key for you (i.e. your Heroku account already has SSH keys associated with it), you can do so manually by running:

     $ mkdir ~/.ssh
     $ ssh-keygen -t rsa

Keep the default file name and skip the passphrase by just hitting Enter both times. Then, add the key to your Heroku account:

    $ heroku keys:add

Once the key business is done, you're ready to deploy apps! Heroku has a great [Getting Started](https://devcenter.heroku.com/articles/python) guide, so I'll let you refer to that (the one linked here is for Python, but there is one for every popular language). Heroku uses Git to push code for deployment, so make sure your app is under Git version control. A quick cheat sheet (if you've used Heroku before):

    $ cd myapp/
    $ heroku create myapp
    $ git push heroku master
    $ heroku ps
    $ heroku logs -t

The [Heroku Dev Center](https://devcenter.heroku.com/) is full of great resources, so be sure to check it out!

## Projects folder

This really depends on how you want to organize your files, but I like to put all my version-controlled projects in `~/Projects`. Other documents I may have, or things not yet under version control, I like to put in `~/Code`.


## Vagrant

[Vagrant](http://www.vagrantup.com/) is open-source software for creating and configuring virtual development environments. It can be considered a wrapper around VirtualBox and configuration management software such as Chef, Salt and Puppet.

[Download](http://www.vagrantup.com/downloads.html) the latest stable version (v1.3.5), open the installer and follow the instructions.


## Virtual Box

Oracle VM [Virtual Box](https://www.virtualbox.org/wiki/Downloads) is a virtualization tool that allows you to create virtual machines on Mac OS X, Linux, or Windows.

[Download](https://www.virtualbox.org/wiki/Downloads) and start the installer and follow the instructions.


## Adobe Apps

Go to the [Adobe Creative Cloud](https://creative.adobe.com/join/starter) site and set up an account. You'll download the creative cloud app which will allow you to download 30 day trial versions of any application you may need. Monsoon can provide licenses as needed when the trial version of the application has expired.

## Apps

Other apps to download:

- [HipChat](https://www.hipchat.com/downloads#mac) Group chat and IM for teams.
- [Yeoman](http://yeoman.io/): Follow the download instructions. You should already have grunt, but you want yeoman and bower too.
- [Dropbox](https://www.dropbox.com/): Design shares assets and wireframes and comps with us through Dropbox. You need a dropbox account with your work email.
- [Google Drive](https://drive.google.com/): File syncing to the cloud too! I use Google Docs a lot to collaborate with others (edit a document with multiple people in real-time!), and sometimes upload other non-Google documents (pictures, etc.), so the app comes in handy for that. **(Free for 5GB)**
- [Dillinger](http://dillinger.io/): Dillinger is a free, open source markdown editor in the browser. Use the hosted app or download it yourself.
- Spaces: Multiple desktop feature.  Support page for OS X 10.6 [here](http://support.apple.com/kb/ht1624)
- [Alfred](http://www.alfredapp.com/): Application hot-launching. Open with Option + Space and start typing what you're looking for. Does lots of other cool stuff.
- [SizeUp](http://www.irradiatedsoftware.com/sizeup/): A window management tool.  Maps shortcuts that arrange your windows into neat little halves/quadrants/etc.  It's a paid app with an unlimited trial period, the caveat being that it will bug you with a "Buy a license prompt" every so often.


## Style Guides

 - [Ruby style guide](https://github.com/bbatsov/ruby-style-guide) Best practices for developing in Ruby
 - [Rails style guide](https://github.com/bbatsov/rails-style-guide) Complimentary guide on Rails styling (many parts are specific to Rails 4.0+)

