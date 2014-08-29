# Ultimate Dev Machine Setup
Mac OS X 10.9

## System Preferences
( > System Preferences)

### Dock
- Size: around the 40% mark
- Magnification: off
- Automatically hide and show the Dock: on

### Mission Control
- Select Hot Corners
- Set the bottom left hot corner to "Start Screen Saver"
- Set the top left hot corner to "Mission Control" 

(note - I like to use the left side of my screen for hot corners as I tend to use an external display with my laptop on the right. You may want to mirror this depending on your configuration)

### Security and Privacy

#### General
- I like to set my password to be required 5 seconds after sleep or screen saver to give me a bit of buffer

#### Firevault
- Turn Filevault on
- I don't share my recovery key with Apple

#### Firewall
- I tend to turn this on, especially since GSA's network doesn't seem to do any sort of protection for devices behind the firewall

### Display
- If you have a retina display, change the resolution to "Scaled" and select the option between "Best (Retina)" and "More Space".

### Trackpad
- Ensure "Tap to click" is selected
- Change "Secondary click" to "Click or tap with two fingers"

### Printers & Scanners
- Add the 18F Printer
- "+ Sign"
- IP
- Address: [ the printer IP address ]

### Sharing
- Change your computer name to something more personal. I use names of Space Shuttles for my personal computers.
- Ensure all sharing options are disabled

## Finder Configuration

- Unhide the library folder:
With the Finder as the foremost application, press shift-command-H and then command-J, which will bring up a window that configures Finder view options. Check the “Show Library Folder” and close the window.

- Create a "Projects" folder at ~/Projects
- Go to Finder -> Preferences
- New Finder Windows show: username (eg. seanherron)
- In the tabs pane, uncheck all tags under "Show these tags in the sidebar"
- In the sidebar pane, check/uncheck the following:

```
Favorites
☐ All My Files
☑ AirDrop
☑ Applications
☑ Desktop
☑ Documents
☑ Downloads
☑ username (eg. "seanherron")

Shared
☐ Back to My Mac
☑ Connected Servers
☐ Bonjour Computers

Devices
☐ Computer Name
☑ Hard Disks
☑ External Disks

Tags
☐ Recent Tags
```

- In the "Advanced" pane, check "Empty Trash Securely"

## Basic Bash Setup

- Open your ~/.bash_profile file (I use `vim ~/.bash_profile`)

and add:

	# Set architecture flags
	export ARCHFLAGS="-arch x86_64"
	# Ensure user-installed binaries take precedence
	export PATH=/usr/local/bin:$PATH
	# Load .bashrc if it exists
	test -f ~/.bashrc && source ~/.bashrc

## Configure Chrome
Most of your work will probably be done in the web browser, so configuring Chrome is super important.

### Multiple users
I find it useful to set up Chrome's users options with an "18F" profile and a "Personal" profile. You can even do differnt profiles for each project you work on.

- Go to Chrome Preferences (Chrome -> Preferences)
- Under users, click "Add New User"
- You can add as many users as you want, and click the icon in the top right of the browser to switch between profiles

## Developer Setup
The follow section covers basic developer setup:

### Xcode
I like to install the full version of Xcode. You can do this by installing the "Xcode" app from the Mac App Store. After it has installed, **you must open the app and accept the terms and conditions before doing anything else**.

### Better Terminal
I like to use [iTerm 2](http://www.iterm2.com/#/section/home) rather than the default bundled Terminal app.

### Homebrew
Everything possible should be installed via Homebrew. Do this by running the following:

    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"

After you install homebrew, run the following:

    $ brew doctor && brew update

Install your first packages, some simple ones I enjoy:

	$ brew install bash-completion ssh-copy-id wget

**NOTE**: If you get an error thing, make sure you open Xcode and accept the terms and conditions, then try again!

Enable bash completion in your ~/.bash_profile:

	# Bash compeltion
	if [ -f $(brew --prefix)/etc/bash_completion ]; then
	    . $(brew --prefix)/etc/bash_completion
	fi

### Git
Install git:

    $ brew install git

Config git:
   
    $ git config --global user.name "Sean Herron"
	$ git config --global user.email sean.herron@gsa.gov

**Note**: If you need to change the email address you are committing from on a per-project basis, you can simply run

    $ git config user.email sean@herron.io

from within the project in question. This will not change things globally, just within the folder of that repository.

### Python
I've been a Python 2 guy for a long time, but with my new role at 18F I'm starting to try running Python 3 for everything. For the ultimate setup, I'm going to run both python 2 and python 3 side by side. This tutorial is very much thanks to [Hacker Codex](http://hackercodex.com/guide/python-development-environment-on-mac-osx/)

First we'll install Python 2 via Homebrew:

	$ brew install python --with-brewed-openssl

Installing via Homebrew rather than using the baked in Python that comes with Mavericks offers several advantages, including:

- When using the bundled Python upgrading OS X can nuke your Python packages, forcing you to reinstall them.
- As new versions of Python are released, the Python bundled with OS X will become out-of-date. Homebrew always has the most recent version.
- Apple has made significant changes to its bundled Python, potentially resulting in hidden bugs.
- Homebrew’s Python includes the latest Python package management tools: pip and Setuptools

We'll also install Python 3:

	$ brew install python3 --with-brewed-openssl


#### Virtualenv

Next we'll install virtualenv:

    $ pip install virtualenv

And we will modify our `~/.bashrc` file with some preferences:

	# pip should only run if there is a virtualenv currently activated
	export PIP_REQUIRE_VIRTUALENV=true
	syspip(){
	   PIP_REQUIRE_VIRTUALENV="" pip "$@"
	}
	# cache pip-installed packages to avoid re-downloading
	export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache

This does two great things. PIP_REQUIRE_VIRTUALENV means that we can't accidently install packages when we are not in a virtual environment. If you need to install global packages, you can instead use

    $ syspip install packagename

PIP_DOWNLOAD_CACHE allows us to cache packages we download to avoid having to download common packages over. And over. And over.

##### Virtualenvwrapper

I like to use [virtualenvwrapper](http://virtualenvwrapper.readthedocs.org/en/latest/) to manage my Python virtual environments. Let's get that set up.

    $ syspip install virtualenvwrapper
	$ mkdir -p ~/.virtualenvs

Next we'll edit our `~/.bashrc` file by adding the following:

	# Virtualenvwrapper time!
	export WORKON_HOME=~/.virtualenvs
	source /usr/local/bin/virtualenvwrapper.sh

Quickly reload your shell:

    $ . ~/.bash_profile

And try making a quick virtualenv:

    $ mkvirtualenv test-env

Voila!

##### Switching between Python 2 and Python 3

In our `test-env` virtual environment, quickly test your Python version:

    $ python -V

You should see that you are running Python 2.7.7, which we installed with homebrew earlier. Exit your virtualenv and we'll create one using Python 3:

    $ deactivate

Now, we'll set up a quick little alias to let us make a virtual environment using Python 3. Edit your `~/.bash_profile` and add:

	# Set up an alias to launch a Python 3 Virtual Environment
	alias mkvirtualenv3="mkvirtualenv --python=/usr/local/bin/python3"

Now reload your shell:

    $ . ~/.bash_profile

and try making a new Python 3 virtual environment using our new alias:

    $ mkvirtualenv3 test-env-python-3

Which should net you:

	Running virtualenv with interpreter /usr/local/bin/python3
	Using base prefix '/usr/local/Cellar/python3/3.4.1/Frameworks/Python.framework/Versions/3.4'
	New python executable in test-env-python-3/bin/python3.4
	Also creating executable in test-env-python-3/bin/python
	Installing setuptools, pip...done.

ta-da!

### Bash Prompt
Edit your `~/.bash_profile` and add the following for a better bash prompt:

    # Set up bash prompt
    export PS1="\W \[$txtcyn\]\$git_branch\[$txtred\]\$git_dirty\[$txtrst\]\$ "
    export SUDO_PS1="\[$bakred\]\[$txtrst\] \w\$ "

## Applications
These are applications I typically install:
- Google Chrome
- Password Manager
