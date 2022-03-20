<img src="https://raw.githubusercontent.com/abdelino17/mac-dev-playbook/master/files/Mac-Dev-Playbook-Logo.png" width="250" height="156" alt="Mac Dev Playbook Logo" />

# Mac Development Ansible Playbook

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have a few manual installation steps, but at least it's all documented here.

## Installation

1. Ensure Apple's command line tools are installed (`xcode-select --install` to launch the installer).
2. [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html):

   1. Run the following command to add Python 3 to your $PATH: `export PATH="$HOME/Library/Python/3.8/bin:/opt/homebrew/bin:$PATH"`
   2. Upgrade Pip: `sudo pip3 install --upgrade pip`
   3. Install Ansible: `pip3 install ansible`

3. Clone or download this repository to your local drive.
4. Run `ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
5. Run `ansible-playbook main.yml --ask-become-pass` inside this directory. Enter your macOS account password when prompted for the 'BECOME' password.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

```yaml
homebrew_installed_packages:
  - cowsay
  - git
  - go

mas_installed_apps:
  - { id: 973130201, name: "Be Focused" }
  - { id: 1091189122, name: "Bear" }
  - { id: 585829637, name: "Todoist" }
  - { id: 1278508951, name: "Trello" }

npm_packages:
  - name: webpack

pip_packages:
  - name: mkdocs

configure_dock: true
dockitems_remove:
  - Launchpad
  - TV
  - Messages

dockitems_persist:
  - name: "Sublime Text"
    path: "/Applications/Sublime Text.app/"
    pos: 5
```

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

- [ChromeDriver](https://sites.google.com/chromium.org/driver/)
- [Docker](https://www.docker.com/)
- [Firefox](https://www.mozilla.org/en-US/firefox/new/)
- [Google Chrome](https://www.google.com/chrome/)
- [Homebrew](http://brew.sh/)
- [MacVim](http://macvim-dev.github.io/macvim/)
- [Slack](https://slack.com/)
- [Sublime Text](https://www.sublimetext.com/)
- [Vagrant](https://www.vagrantup.com/)
- [Iterm2](https://iterm2.com/)
- [Insomnia REST Client](https://insomnia.rest/)
- [Discord](https://discord.com/)
- [Jetbrains Toolbox](https://www.jetbrains.com/toolbox-app/)
- [Loom](https://www.loom.com/)
- [Microsoft Edge](https://www.microsoft.com/edge)
- [Microsoft Teams](https://teams.microsoft.com/downloads)
- [Microsoft Remote Desktop](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac)
- [Miro](https://miro.com/)
- [MongoDB Compass](https://www.mongodb.com/products/compass)
- [Notion](https://www.notion.so/)
- [pgAdmin4](https://www.pgadmin.org/)
- [Oracle JDK](https://www.oracle.com/java/technologies/downloads/)
- [Postman](https://www.postman.com/)
- [Redis Insight](https://www.redislabs.com/redisinsight/)
- [Spotify](https://www.spotify.com/)
- [Sqlite Studio](https://sqlitestudio.pl/)
- [Sublime Text](https://www.sublimetext.com/)
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant Manager](https://www.vagrantmanager.com/)
- [Visual Studio Code](https://code.visualstudio.com/)
- [VLC](https://www.videolan.org/vlc/)
- [Zoom](https://www.zoom.us/)
- [Zscaler](https://www.zscaler.fr/)

Packages (installed with Homebrew):

- autoconf
- gettext
- git
- go
- rust
- python
- wget
- htop
- rustup-init
- tfenv
- ...

My [dotfiles](https://github.com/geerlingguy/dotfiles) are also installed into the current user's home directory, including the `.osx` dotfile for configuring many aspects of macOS for better performance and ease of use. You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Ansible for DevOps

Check out [Ansible for DevOps](https://www.ansiblefordevops.com/), which teaches you how to automate almost anything with Ansible.

## Author

This project is my version of [Jeff Geerling](https://www.jeffgeerling.com/) [mac setup](https://github.com/geerlingguy/mac-dev-playbook).
