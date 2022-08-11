# new_mac_setup

Simple list of instructions to make setting up your computer as fast and efficient as possible. 

## Glossary
* [Brewfile](Brewfile) will contain the list of applications that will be installed on the machine
* [optional-setup](optional-setup) contains useful steps for setting up your git, maven, terminal, tomcat, vscode, and zshrc



## Instructions

Install [Homebrew](https://brew.sh/), a package manager for macOS

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Install Applications using Homebrew

Clone this repo and run the following command inside it. This will execute the steps declared in the [Brewfile](Brewfile), installing all the declared applications in it

```shell
brew bundle install
```
***Note:***

* Comment out the tools that are already installed to avoid installation failure

* Run the following two commands individually if installing tools in bundle fails because of grep/openssh installation error and remember to comment them out in the [Brewfile](Brewfile)

  ```bash
  brew install grep
  brew install openssh
  ```

### RVM and Ruby (Ruby Version Manager)

#### Download rvm

```shell
\curl -sSL https://get.rvm.io | bash -s stable
```

#### Install Ruby version

```shell
rvm install 2.4.0
```

```shell
rvm --default use 2.4.0
```

```shell
gem install bundler
```

### Java Environment

#### Install jenv

Install [jenv](https://github.com/gcuisinier/jenv) to switch between Java 6 and 8

```shell
git clone https://github.com/gcuisinier/jenv.git ~/.jenv
```

If zshell is used, run
```shell
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc
```
Or if bash is used, run
```shell
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(jenv init -)"' >> ~/.bashrc
```
```shell
# Restart shell
exec $SHELL -l

# Before running command make sure version is correct
jenv add /Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
jenv add /Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home

# Make Java 8 global
jenv global 1.8

# Enable maven support
jenv enable-plugin maven
# Enable plugin to properly set $JAVA_HOME needed by various tools
jenv enable-plugin export
```
Use `jenv doctor` first if encounter any problems. E.g.
```shell
$> jenv enable-plugin
jenv: no such command `enable-plugin`

# Help diagnose the problem and give suggested solutions
jenv doctor
```

### Additional JAVA

There is an existing Java Developer [Getting Started](https://wiki.cerner.com/pages/releaseview.action?spaceKey=pophealth&title=Java%20Developer%20-%20Getting%20Started) which might have information necessary for your setup.  If there are necessary, please update this wiki/brewfile with the missing steps.

### Jumpgate
The jumpgate allows you to access SphereStage(staging) and CernerSphere(prod) Web UIs from your Mac without having to go through the hassle of logging into [SphereStage](https://access.spherestage.com) or [CernerSphere](https://access.cernersphere.com) and then going to a browser instance

Steps to configure the jumpgate can be found [here](https://github.cerner.com/ETS/workstation-config/blob/master/README.md)

***Alternative***

This option assumes that you use lastpass to securely store your passwords and have lastpass-cli installed.

* Download zone0.sh file on local machine. On Terminal window navigate to file location (ZONE0_FILE_LOCATION) and run 'chmod +x zone0.sh'. Add the following to your ~/.bash_profile file.

```shell
alias lpp_prod="lpass show <YOUR_CERNSPHERE_PASSWORD_NAME> --password"

alias zone0="<ZONE0_FILE_LOCATION>/zone0.sh "$(lpp_prod)""
```

Now, you can connect to Zone 0 by simply typing this into Terminal and accepting Duo authentication.

```shell
zone0
```

You can create similar files (and corresponding aliases) for spherestage, uksphere and aws (zone 1 and 2).

## VPN Split Tunnel
The ETS VPN split tunnel scripts allow you to connect to multiple VPN's at once so that you can access other HealtheIntent environments from home such as AWS or UK.
[Split Tunnel](https://github.cerner.com/ETS/split_tunnel/)

## Optional

### Clipy
[Clipy](https://github.com/Clipy/Clipy) is a very useful application that stores your recent copy+paste history so you can quickly go back and forth between recently pasted items

Download link [here](https://github.com/Clipy/Clipy/releases), just grab the latest release.

### Alfred
[Alfred](https://www.alfredapp.com/) enhances a Mac's spotlight search by adding a lot more features such as searching within your clipboard history, and others

### oh-my-zsh
[Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh) is an open source, community-driven framework for managing your zsh configuration that easily allows you to install plugins and themes to uplift your zsh to allow you to be more productive

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

You can run the following command to source (i.e. reload) the shell with the new config from oh-my-zsh
```shell
source ~/.zshrc
```

### Find_user
[find_user](https://github.cerner.com/devecosystem-tools/shell_tools/tree/master/find_user) is a script that searches for users by their various names

### lastpass-cli
[lastpass-cli](https://github.com/lastpass/lastpass-cli) is a command line interface that will allow you to easily copy passwords to your clipboard from the command line.

Start by downloading LastPass and lastpass-cli.
```shell
brew cask install lastpass
brew install lastpass-cli
```

Next, add any passwords you want to access from the command line to LastPass. To quickly access them you can now run the following command.

```shell
lpass show -c <PASSWORD_NAME> --password
```

To simplify this process, we can add the following alias to our .bash_profile file.

```shell
alias lpp="lpass show -c <PASSWORD_NAME> --password"
```

You will then only need to run 'lpp' (or whatever you name the alias) from the command line. 

### General MacOS Preferences

#### Show Library folder

```shell
chflags nohidden ~/Library
```

#### Show hidden files

This can also be done by pressing `command` + `shift` + `.`.

```shell
defaults write com.apple.finder AppleShowAllFiles YES
```

#### Show path bar

```shell
defaults write com.apple.finder ShowPathbar -bool true
```

#### Show status bar

```shell
defaults write com.apple.finder ShowStatusBar -bool true
```
