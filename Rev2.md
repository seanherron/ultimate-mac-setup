# Get Started
- Install XCode from the Mac App Store. After it has installed, **you must open the app and accept the terms and conditions before doing anything else**.
- Install Homebrew:
    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go/install)"
    $ brew doctor && brew update
- Install Git
    $ brew install git
- Config Git
    $ git config --global user.name "Sean Herron"
    $ git config --global user.email sean.herron@gsa.gov

**Note**: If you need to change the email address you are committing from on a per-project basis, you can simply run

    $ git config user.email sean@herron.io

from within the project in question. This will not change things globally, just within the folder of that repository.

# Dotfiles
Get your dotfiles set up first. I like to clone mine (obviously):
    $ git clone https://github.com/seanherron/dotfiles ~/Projects/dotfiles
