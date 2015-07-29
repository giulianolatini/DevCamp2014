# Script di presentazione

## Punti da sviluppare
Ora che iniziamo a parlare di queste cose vediamo il sito radice di [Puppet Labs][Puppet#01] che aiuta 



### Frammenti di codice e comandi

```bash
azure account download
```
La procedura migliore per attivare il collegamento con il proprio account Microsoft Azure è procedere come segue:

![azure account download][azure-cli#01]

* dal prompt digitare `azure account download` , verrà chiamato il browser standard di sistema che, risolta la fase di autenticazione tramite il servizio di accounting microsoft, provvederà a scaricare un file contenente le informazioni di connessione.

![azure account import][azure-cli#02]

* tramite il comando `azure account import` importiamo nel sistema le informazioni necessarie a costruire il canale autenticato tra cui transiteranno i comandi verso il nostro account Microsoft Azure.

* tramite il comando `azure vm image list` vediamo l'elenco delle immagini disponibili nello store per applicarlo al comando di deploy
* b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140618.1-en-us-30GB
```bash
openssl req -x509 -nodes -days 365 \
-newkey rsa:2048 \
-keyout test-pp02.key \
-out test-pp02.pem

mv test-pp02.key ~/.ssh
chmod a-rwx,u+rw ~/.ssh/test-pp02.key
ssh-add ~/.ssh/test-pp02.key

azure vm create test-pp02.cloudapp.net b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140618.1-en-us-30GB wolf --location "North Europe" -z small -e 22 -t test-pp02.pem -P

ssh -p 22 wolf@test-pp02.cloudapp.net -c blowfish -C -X #Connessione ed apertura shell come utente wolf

# Pre-stage installation (installazione prerequisiti)
wget https://apt.puppetlabs.com/puppetlabs-release-trusty.deb
sudo dpkg -i puppetlabs-release-trusty.deb
sudo apt-get update

# Install Puppet-Master
sudo apt-get -y install puppetmaster-passenger
sudo apt-get -y install puppetmaster

sudo puppet module install thomasvandoren-etckeeper
sudo puppet module install thomasvandoren-etckeeper
sudo puppet module install jpadams-puppet_vim_env
sudo puppet module install acme/ohmyzsh
sudo puppet module install stankevich-python
sudo puppet module install puppetlabs-apache
sudo puppet module install puppetlabs-mysql
sudo puppet module install example42-php

azure vm shutdown test-pp02 # spengo la vm test-pp02
```

![azure vm list][azure-cli#03]

![azure vm start][azure-cli#04]

### Uso dei Ports in FreeBSD (dalla 10.01 in poi)

#### Installazione Ports usando il tool nativo <b>portsnap<b>
##### Installazione
```bash
# Scarico l'archivio dei ports e ne verifico le signature
portsnap fetch
# scompatto in /usr/ports l'archivio scaricato
portsnap extract
```
##### Aggiornamento

```bash
# Aggiorno il ramo /usr/ports
portsnap fetch update
```

#### Installazione pacchetti di gestione Ports consigliati

```bash
# Aggiorno il ramo /usr/ports
cd /usr/ports/ports-mgmt/portmaster
make install clean
```
##### Al termine dell'installazione seguire la direttiva:
    Enable PKGNG as your package format:
```bash
        echo 'WITH_PKGNG=yes' >> /etc/make.conf
```
    Then convert your /var/db/pkg database to the new pkg format:
```bash
        pkg2ng
```
##### Per convertire l'archivio dei ports installati nel nuovo formato
#### Comandi Utili: 
```bash
        portmaster -L 		# Lista pacchetti da aggiornare
        portmaster -l 		# Lista pacchetti organizzati per macrocategorie
        portmaster -a		# Aggiornata tutti i pacchetti
        portmaster -af		# forza l'aggiornamento in caso di errori
        portmaster shells/bash  # installazione del paccheto bash nella classe shells
```
Usare Portmaster per l'installazione di pacchetti ha il vantaggio di eseguire automaticamente make clean e cancellare la temporary work dir per la parte di configurazione e costrzuione del pacchetto compilato a meno di non specificare l'ozione -K

Note:
By default, Portmaster will make a backup package before deleting the existing port. If the installation of the new version is successful, Portmaster will delete the backup. Using -b will instruct Portmaster not to automatically delete the backup. Adding -i will start Portmaster in interactive mode, prompting for confirmation before upgrading each port. Many other options are available. Read through the manual page for portmaster(8) for details regarding their usage.


### Script di installazione

Accesso al server **Per Ubuntu Server 15.04**
aggiungere in coda al file ```/etc/ssh/sshd_config``` le seguenti righe:
```bash
# enable all ciphers!
# obtained with ssh -Q cipher localhost | paste -d , -s
Ciphers blowfish-cbc,arcfour256,aes256-cbc,aes192-ctr,aes256-ctr,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com
```
per abilitare l'accesso via protocolli veloci e/o sicuri

```bash
sudo -i

/usr/bin/puppet-init/puppet-enterprise-3.2.2-ubuntu-12.04-amd64/puppet-enterprise-installer

apt-get install git etckeeper

cd /etc
git init
vi /etc/etckeeper/etckeeper.conf #Modifico l'assegnazione della variabile VCS in VCS="git"

git config --global user.name "Administrator"
git config --global user.email "root@localhost.localnets"
git config -l
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

mkdir /var/git
git init --bare /var/git/localconf_etc.git # I Repository bare per convenzione sono indicati .git
```

Con il comando, [ssh-keygen][git-scm.com#ssh-keygen] genero la coppia di chiavi rsa da utilizzare per le connessioni securizzate via ssh senza la necessità di digitare la password (nel caso in cui si voglia aggiungere automaticamente le chiavi nello store di security gestito dal `ssh-agent` lasciare vuota la richiesta della passphrase)

```txt
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
Enter passphrase (empty for no passphrase): 
Enter same passphrase again:
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
bf:d5:8b:8b:48:f5:53:44:00:43:4f:33:7e:26:db:21 root@puppet-test01
The key's randomart image is:
+--[ RSA 2048]----+
|         .+.=..  |
|           = +   |
|            E =  |
|             O . |
|        S . . o  |
|         o . o   |
|        . . + .  |
|       . . + o . |
|        . o o..  |
+-----------------+
```

da aggiungere al file `~/.bash_profile` così che la chiave appena generata al login venga aggiunta automaticamente nello store gestito da ssh-agent per risolvere le problematiche di connessione securizzata senza la necessità di digitare password 

```bash
if [ -z "$SSH_AUTH_SOCK" ] ; then
  eval `ssh-agent -s`
  ssh-add
fi
```

```bash
# add the ssh-agent plugin to `plugins` variables to load ssh-agent oh-my-zsh support
plugins=(ssh-agent ... )
#  enable and load ssh-agent identities
zstyle :omz:plugins:ssh-agent identities id_rsa latini.giuliano@gmail.com
# @end-of-script adding openssh-RSA keys
export SSH_KEYS_PATH="/Users/wolf/.ssh"
ssh-add $SSH_KEYS_PATH/test-pp02.key
ssh-add $SSH_KEYS_PATH/test-pp03.key
```


Aggiungo la parte pubblica della chiave appena generata (`id_rsa.pub`) al db che openssh-server utilizza per ricavare le parti pubbliche utilizzate per decodificare la richiesta di connessione. Se la parte pubblica della propria chiave è presente, la decodifica della richiesta di accesso (codificata con la propria parte privata) ha successo, l'accesso alla shell è consentito senza la richiesta di password, l'esempio classico d'uso di questa modalità è lo scripting di attività da un sistema ad un altro remoto.

```bash
cd ~/.ssh
cat id_rsa.pub >> authorized_keys

cd /etc
git remote add origin root@localhost:/var/git/localconf_etc.git
o
git remote add origin ssh://root@localhost:[ssh-port]/var/git/localconf_etc.git
git add *
etckeeper commit "First Commit"
git push -u origin master
```

```txt
Counting objects: 1658, done.
Compressing objects: 100% (1282/1282), done.
Writing objects: 100% (1658/1658), 902.11 KiB, done.
Total 1658 (delta 85), reused 0 (delta 0)
To root@localhost:/var/git/localconf_etc.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.
```

Configurazione Generale

```bash

# Comandi Brevi
sudo apt-get -y install vim-addon-manager vim-puppet vim-scripts zsh curl git-flow build-essential cmake python-dev python-pip exuberant-ctags byobu
sudo curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
sudo pip install git+git://github.com/Lokaltog/powerline

sudo chsh -s $(which zsh) 
# Comandi Singoli

sudo apt-get install vim-addon-manager vim-puppet vim-scripts
sudo apt-get install zsh
sudo chsh -s $(which zsh) # Modifica della shell chiamata al login
sudo apt-get install curl
sudo curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
sudo apt-get install git-flow
sudo apt-get install build-essential cmake
sudo apt-get install python-dev
sudo apt-get install python-pip
sudo apt-get install exuberant-ctags
sudo pip install --user git+git://github.com/Lokaltog/powerline
sudo apt-get install byobu
```
Nel caso il sistema non sia localizzato in UTF8 (come le caso di una installazioen non localizzata, 
vedi contenuto del file:
```bash
cat /etc/default/locale
```
Se il file contiene il valore LANG=C e non LANG=C.UTF-8 esso va reimpostato nel file e aggironato live con il comando
```bash
update-locale LANG=C.UTF-8
```

File da aggiungere sotto ~/.oh-my-zsh/themes come ducknorris.zsh-theme
```bash
# ducknorris custom theme
# FreeAgent puts the powerline style in zsh !

if [ "$POWERLINE_DATE_FORMAT" = "" ]; then
  POWERLINE_DATE_FORMAT=%D{%Y-%m-%d}
fi

if [ "$POWERLINE_RIGHT_B" = "" ]; then
  POWERLINE_RIGHT_B=%D{%H:%M:%S}
elif [ "$POWERLINE_RIGHT_B" = "none" ]; then
  POWERLINE_RIGHT_B=""
fi

if [ "$POWERLINE_RIGHT_A" = "mixed" ]; then
  POWERLINE_RIGHT_A=%(?."$POWERLINE_DATE_FORMAT".%F{red}✘ %?)
elif [ "$POWERLINE_RIGHT_A" = "exit-status" ]; then
  POWERLINE_RIGHT_A=%(?.%F{green}✔ %?.%F{red}✘ %?)
elif [ "$POWERLINE_RIGHT_A" = "date" ]; then
  POWERLINE_RIGHT_A="$POWERLINE_DATE_FORMAT"
fi

if [ "$POWERLINE_HIDE_USER_NAME" = "" ] && [ "$POWERLINE_HIDE_HOST_NAME" = "" ]; then
    POWERLINE_USER_NAME="%n@"'%M'
elif [ "$POWERLINE_HIDE_USER_NAME" != "" ] && [ "$POWERLINE_HIDE_HOST_NAME" = "" ]; then
    POWERLINE_USER_NAME="@%M"
elif [ "$POWERLINE_HIDE_USER_NAME" = "" ] && [ "$POWERLINE_HIDE_HOST_NAME" != "" ]; then
    POWERLINE_USER_NAME="%n"
else
    POWERLINE_USER_NAME=""
fi

POWERLINE_CURRENT_PATH="%d"

if [ "$POWERLINE_FULL_CURRENT_PATH" = "" ]; then
  POWERLINE_CURRENT_PATH="%1~"
fi

if [ "$POWERLINE_GIT_CLEAN" = "" ]; then
  POWERLINE_GIT_CLEAN="✔"
fi

if [ "$POWERLINE_GIT_DIRTY" = "" ]; then
  POWERLINE_GIT_DIRTY="✘"
fi

if [ "$POWERLINE_GIT_ADDED" = "" ]; then
  POWERLINE_GIT_ADDED="%F{green}✚%F{black}"
fi

if [ "$POWERLINE_GIT_MODIFIED" = "" ]; then
  POWERLINE_GIT_MODIFIED="%F{blue}✹%F{black}"
fi

if [ "$POWERLINE_GIT_DELETED" = "" ]; then
  POWERLINE_GIT_DELETED="%F{red}✖%F{black}"
fi

if [ "$POWERLINE_GIT_UNTRACKED" = "" ]; then
  POWERLINE_GIT_UNTRACKED="%F{yellow}✭%F{black}"
fi

if [ "$POWERLINE_GIT_RENAMED" = "" ]; then
  POWERLINE_GIT_RENAMED="➜"
fi

if [ "$POWERLINE_GIT_UNMERGED" = "" ]; then
  POWERLINE_GIT_UNMERGED="═"
fi

ZSH_THEME_GIT_PROMPT_PREFIX=" \ue0a0 "
ZSH_THEME_GIT_PROMPT_SUFFIX=""
ZSH_THEME_GIT_PROMPT_DIRTY=" $POWERLINE_GIT_DIRTY"
ZSH_THEME_GIT_PROMPT_CLEAN=" $POWERLINE_GIT_CLEAN"

ZSH_THEME_GIT_PROMPT_ADDED=" $POWERLINE_GIT_ADDED"
ZSH_THEME_GIT_PROMPT_MODIFIED=" $POWERLINE_GIT_MODIFIED"
ZSH_THEME_GIT_PROMPT_DELETED=" $POWERLINE_GIT_DELETED"
ZSH_THEME_GIT_PROMPT_UNTRACKED=" $POWERLINE_GIT_UNTRACKED"
ZSH_THEME_GIT_PROMPT_RENAMED=" $POWERLINE_GIT_RENAMED"
ZSH_THEME_GIT_PROMPT_UNMERGED=" $POWERLINE_GIT_UNMERGED"
ZSH_THEME_GIT_PROMPT_AHEAD=" ⬆"
ZSH_THEME_GIT_PROMPT_BEHIND=" ⬇"
ZSH_THEME_GIT_PROMPT_DIVERGED=" ⬍"

if [ "$POWERLINE_SHOW_GIT_ON_RIGHT" = "" ]; then
    if [ "$POWERLINE_HIDE_GIT_PROMPT_STATUS" = "" ]; then
        POWERLINE_GIT_INFO_LEFT=" %F{blue}%K{white}"$'\ue0b0'"%F{white}%F{black}%K{white}"$'$(git_prompt_info)$(git_prompt_status)%F{white}'
    else
        POWERLINE_GIT_INFO_LEFT=" %F{blue}%K{white}"$'\ue0b0'"%F{white}%F{black}%K{white}"$'$(git_prompt_info)%F{white}'
    fi
    POWERLINE_GIT_INFO_RIGHT=""
else
    POWERLINE_GIT_INFO_LEFT=""
    POWERLINE_GIT_INFO_RIGHT="%F{white}"$'\ue0b2'"%F{black}%K{white}"$'$(git_prompt_info)'" %K{white}"
fi

if [ $(id -u) -eq 0 ]; then
    POWERLINE_SEC1_BG=%K{red}
    POWERLINE_SEC1_FG=%F{red}
else
    POWERLINE_SEC1_BG=%K{green}
    POWERLINE_SEC1_FG=%F{green}
fi
POWERLINE_SEC1_TXT=%F{black}
if [ "$POWERLINE_DETECT_SSH" != "" ]; then
  if [ -n "$SSH_CLIENT" ]; then
    POWERLINE_SEC1_BG=%K{red}
    POWERLINE_SEC1_FG=%F{red}
    POWERLINE_SEC1_TXT=%F{white}
  fi
fi
PROMPT="$POWERLINE_SEC1_BG$POWERLINE_SEC1_TXT $POWERLINE_USER_NAME %k%f$POWERLINE_SEC1_FG%K{blue}"$'\ue0b0'"%k%f%F{white}%K{blue} "$POWERLINE_CURRENT_PATH"%F{blue}"$POWERLINE_GIT_INFO_LEFT" %k"$'\ue0b0'"%f "

if [ "$POWERLINE_NO_BLANK_LINE" = "" ]; then
    PROMPT="
"$PROMPT
fi

if [ "$POWERLINE_DISABLE_RPROMPT" = "" ]; then
    if [ "$POWERLINE_RIGHT_A" = "" ]; then
        RPROMPT="$POWERLINE_GIT_INFO_RIGHT%F{white}"$'\ue0b2'"%k%F{black}%K{white} $POWERLINE_RIGHT_B %f%k"
    elif [ "$POWERLINE_RIGHT_B" = "" ]; then
        RPROMPT="$POWERLINE_GIT_INFO_RIGHT%F{white}"$'\ue0b2'"%k%F{240}%K{white} $POWERLINE_RIGHT_A %f%k"
    else
        RPROMPT="$POWERLINE_GIT_INFO_RIGHT%F{white}"$'\ue0b2'"%k%F{black}%K{white} $POWERLINE_RIGHT_B %f%F{240}"$'\ue0b2'"%f%k%K{240}%F{255} $POWERLINE_RIGHT_A %f%k"
    fi
fi
```

Configurazione Avanzata di .zshrc
```bash
# Path to your oh-my-zsh configuration.
ZSH=$HOME/.oh-my-zsh

# Set name of the theme to load.
# Look in ~/.oh-my-zsh/themes/
# Optionally, if you set this to "random", it'll load a random theme each
# time that oh-my-zsh is loaded.
ZSH_THEME="ducknorris"

POWERLINE_HIDE_HOST_NAME=" "
POWERLINE_DETECT_SSH="true"
POWERLINE_GIT_CLEAN="✔"
POWERLINE_GIT_DIRTY="✘"
POWERLINE_GIT_ADDED="%F{green}✚%F{black}"
POWERLINE_GIT_MODIFIED="%F{blue}✹%F{black}"
POWERLINE_GIT_DELETED="%F{red}✖%F{black}"
POWERLINE_GIT_UNTRACKED="%F{yellow}✭%F{black}"
POWERLINE_GIT_RENAMED="➜"
POWERLINE_GIT_UNMERGED="═"

# Example aliases
alias zshconfig="mate ~/.zshrc"
alias ohmyzsh="mate ~/.oh-my-zsh"

# Set to this to use case-sensitive completion
CASE_SENSITIVE="true"

# Uncomment this to disable bi-weekly auto-update checks
# DISABLE_AUTO_UPDATE="true"

# Uncomment to change how often before auto-updates occur? (in days)
# export UPDATE_ZSH_DAYS=13

# Uncomment following line if you want to disable colors in ls
# DISABLE_LS_COLORS="true"

# Uncomment following line if you want to disable autosetting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment following line if you want to disable command autocorrection
# DISABLE_CORRECTION="true"

# Uncomment following line if you want red dots to be displayed while waiting for completion
# COMPLETION_WAITING_DOTS="true"

# Uncomment following line if you want to disable marking untracked files under
# VCS as dirty. This makes repository status check for large repositories much,
# much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment following line if you want to  shown in the command execution time stamp 
# in the history command output. The optional three formats: "mm/dd/yyyy"|"dd.mm.yyyy"|
# yyyy-mm-dd
HIST_STAMPS="dd/mm/yyyy"

# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
plugins=(git ssh-agent git-flow git-remote-branch github osx sublime sudo rsync python npm node macports zsh-syntax-highlighting)

# User configuration
# The next configuration path for go language
export GOROOT="/usr/local/go"
export GOPATH="/Users/wolf/Sviluppo/go"

# When install to Linux with pip use this PATH envearoment export
# export PATH=$HOME/.local/bin:$HOME/bin:/usr/local/bin:$PATH
export PATH=$GOPATH/bin:$GOROOT/bin:$HOME/bin:/opt/local/bin:/usr/local/bin:/opt/local/bin:$PATH

# export MANPATH="/usr/local/man:$MANPATH"

# # Preferred editor for local and remote sessions
if [[ -n $SSH_CONNECTION ]]; then
  export EDITOR='vim'
else
  export EDITOR='mvim'
fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

#  enable and load ssh-agent identities
zstyle :omz:plugins:ssh-agent identities id_rsa latini.giuliano@gmail.com

# Setup Powerline
# how install with command: sudo pip install --user git+git://github.com/Lokaltog/powerline 
. ~/.local/lib/python2.7/site-packages/powerline/bindings/zsh/powerline.zsh

# Setup Google Develop SDK
export FPATH="/Users/wolf/.oh-my-zsh/custom/plugins/gcloud-zsh-completion/src/:$FPATH"
autoload -U compinit compdef
compinit
# The next line updates PATH for the Google Cloud SDK.
. ~/google-cloud-sdk/path.zsh.inc
# The next line enables bash completion for gcloud.
. ~/google-cloud-sdk/completion.zsh.inc

# Setup oh-my-zsh
source $ZSH/oh-my-zsh.sh

if [ -f ~/.zsh/zshalias ]; then
    source ~/.zsh/zshalias
else
    print "404: ~/.zsh/zshalias not found."
fi

# ssh adding others keys
export SSH_KEYS_PATH="/Users/wolf/.ssh"
ssh-add $SSH_KEYS_PATH/test-pp02.key
ssh-add $SSH_KEYS_PATH/test-pp03.key

```

Configurazione di ViM

```vim
set nu!
autocmd FileType html setlocal shiftwidth=2 tabstop=2
autocmd FileType python setlocal expandtab shiftwidth=4 softtabstop=4
autocmd FileType css setlocal expandtab shiftwidth=2 softtabstop=2
autocmd FileType puppet setlocal expandtab shiftwidth=2 softtabstop=2
set tabstop=4 softtabstop=4 shiftwidth=4 noexpandtab
set paste
let mapleader = ","
set laststatus=2
set statusline=%{GitBranch()}
python from powerline.vim import setup as powerline_setup
python powerline_setup()
python del powerline_setup
set rtp+=~/.powerline/powerline/bindings/vim
let g:Powerline_symbols = 'fancy'
set encoding=utf-8
set t_Co=256
set fillchars+=stl:\ ,stlnc:\
set term=xterm-256color
set termencoding=utf-8
```
Installazione Pacchetto PATHOGEN:
```bash
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```

Configurazione ViM Estesa 
```vim
execute pathogen#infect('stuff/{}')
call pathogen#infect()

syntax on
filetype plugin indent on

let g:syntastic_always_populate_loc_list=1

"To enable Just puppet-lint and disable the parser uncomment next line
let g:syntastic_puppet_checkers=['puppetlint']
let g:Powerline_symbols = 'unicode'
let g:Powerline_theme="skwp"
let g:Powerline_colorscheme="skwp"
let g:Powerline_symbols = 'fancy'

set rtp+=.powerline/powerline/bindings/vim

set background=dark
set laststatus=2
set encoding=utf-8
set fillchars+=stl:\ ,stlnc:\

set term=xterm-256color
set termencoding=utf-8
set t_Co=256

set nu!
autocmd FileType html setlocal shiftwidth=2 tabstop=2
autocmd FileType python setlocal expandtab shiftwidth=4 softtabstop=4
autocmd FileType css setlocal expandtab shiftwidth=2 softtabstop=2
autocmd FileType puppet setlocal expandtab shiftwidth=2 softtabstop=2
set tabstop=4 softtabstop=4 shiftwidth=4 noexpandtab
set paste
let mapleader = ","
set laststatus=2
set statusline=%{GitBranch()}
```

Installazione Pacchetti PATHOGEN:
```bash
cd ~/.vim/bundle
git clone https://github.com/scrooloose/nerdtree.git

```
Installazione Pacchetto Vundle:
```bash
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
Installazione Pachetto YouComplete (switch di compilazione per OSX 10.10)
```bash
./install.sh --clang-completer --system-libclang --gocode-completer --system-boost
```

Configurazione ViM Estesa per sviluppo software (lingiaggi supportati: Puppet, Google Go, Python)
```vim
execute pathogen#infect('stuff/{}')
call pathogen#infect()

set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
"Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Avoid a name conflict with L9
"Plugin 'user/L9', {'name': 'newL9'}
" Adding YouCompleteMe plugin
Plugin 'Valloric/YouCompleteMe'
" Track the engine.
Plugin 'SirVer/ultisnips'
" Snippets are separated from the engine. Add this if you want them:
Plugin 'honza/vim-snippets'
" Adding Color Themes
Plugin 'flazz/vim-colorschemes'

" All of your Plugins must be added before the following line
call vundle#end()            " required
"filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line

syntax on
filetype plugin indent on

" GUI setting font case for [GTK2, Photon, KDE, X11]
if has("gui_running")
  if has("gui_gtk2")
    set guifont=Courier\ New\ 11
  elseif has("gui_photon")
    set guifont=Courier\ New:s11
  elseif has("gui_kde")
    set guifont=Courier\ New/11/-1/5/50/0/0/0/1/0
  elseif has("x11")
    set guifont=-*-courier-medium-r-normal-*-*-180-*-*-m-*-*
  else
"    set guifont=Courier_New:h11:cDEFAULT
	set guifont=Inconsolata\ for\ PowerLine:h16
  endif
endif

" Setting colorscheme and coursorline feauture
"colorscheme zenburn
"let g:zenburn_force_dark_Background=1

let g:molokai_original = 1
let g:rehash256 = 1

" Autostart NERDTreeToggle
au VimEnter * NERDTreeToggle
" Autoclose NERDTreeToggle when is only window opened
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif

" Mapping filetype markdown to *.md
au BufRead,BufNewFile *.md set filetype=markdown

" Map leader keyword to - key
let mapleader = ","

" Enable/Disable (using \c keys sequence) cross-indicator to identified row&column where is cursor
hi CursorLine   cterm=NONE ctermbg=darkred ctermfg=white guibg=darkred guifg=white
hi CursorColumn cterm=NONE ctermbg=darkred ctermfg=white guibg=darkred guifg=white
nnoremap <Leader>c :set cursorline! cursorcolumn!<CR>

augroup CursorLine
  au!
  au VimEnter,WinEnter,BufWinEnter * setlocal cursorline
  au WinLeave * setlocal nocursorline
augroup END

let g:syntastic_always_populate_loc_list=1

"To enable Just puppet-lint and disable the parser uncomment next line
let g:syntastic_puppet_checkers=['puppetlint']
let g:Powerline_symbols = 'unicode'
let g:Powerline_theme="skwp"
let g:Powerline_colorscheme="skwp"
let g:Powerline_symbols = 'fancy'

" Trigger configuration. Do not use <tab> if you use https://github.com/Valloric/YouCompleteMe.
let g:UltiSnipsExpandTrigger="<tab>"
let g:UltiSnipsJumpForwardTrigger="<c-b>"
let g:UltiSnipsJumpBackwardTrigger="<c-z>"

" If you want :UltiSnipsEdit to split your window.
let g:UltiSnipsEditSplit="vertical"

" Active tagbar plugin
nmap <F8> :TagbarToggle<CR>

" Vim-go personalization
au FileType go nmap <Leader>gd <Plug>(go-doc)
au FileType go nmap <Leader>gv <Plug>(go-doc-vertical)

au FileType go nmap <Leader>gb <Plug>(go-doc-browser)

au FileType go nmap <leader>r <Plug>(go-run)
au FileType go nmap <leader>b <Plug>(go-build)
au FileType go nmap <leader>t <Plug>(go-test)

au FileType go nmap gd <Plug>(go-def)

au FileType go nmap <Leader>ds <Plug>(go-def-split)
au FileType go nmap <Leader>dv <Plug>(go-def-vertical)
au FileType go nmap <Leader>dt <Plug>(go-def-tab)

au FileType go au BufWritePre <buffer> Fmt

" Autoregen ctags @closing
au BufWritePost *.go silent! !ctags -R &

" PowerLine activation
python from powerline.vim import setup as powerline_setup
python powerline_setup()
python del powerline_setup
set rtp+=~/.local/lib/python2.7/site-packages/powerline/bindings/vim

set background=dark
set laststatus=2
set encoding=utf-8
set fillchars+=stl:\ ,stlnc:\

"set term=xterm-256color
set termencoding=utf-8
set t_Co=256

set nu!
autocmd FileType html setlocal shiftwidth=2 tabstop=2
autocmd FileType python setlocal expandtab shiftwidth=4 softtabstop=4
autocmd FileType css setlocal expandtab shiftwidth=2 softtabstop=2
autocmd FileType puppet setlocal expandtab shiftwidth=2 softtabstop=2
" Beavher enviroment like bash for load menu
set wildmode=longest:full
set wildmenu

set foldmethod=syntax
set foldnestmax=10
set nofoldenable
set foldlevel=0

" Set tab default
set tabstop=4 softtabstop=4 shiftwidth=4 noexpandtab
" Set 10 lines before or after coursorline scroll
set so=10
set paste

set laststatus=2
set statusline=%{GitBranch()}
```


```bash
update_system='sudo apt-get update; sudo etckeeper commit "Aggiornamento Quotidiano"; sudo apt-get upgrade; sudo apt-get dist-upgrade; sudo apt-get check; sudo apt-get autoremove; sudo apt-get autoclean'
```

Configurazione base per un cloud-service [OpenCPU] su Ubuntu 14.04 LTS

```bash
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:opencpu/opencpu-1.4
sudo apt-get update
sudo apt-get install opencpu  
```

Configurazione servizio di Voting On-line [HELIOS]

```bash
apt-get install postgresql
apt-get install python-psycopg2
apt-get install python-django
su - postgres
createuser --superuser <$whoami>
exit
cd /usr/local/share
git clone --recursive git://github.com/benadida/helios-server.git
git clone http://github.com/openid/python-openid
cd python-openid
sudo python setup.py install
apt-get install python-setuptools
easy_install south
apt-get install rabbitmq-server
easy_install celery
easy_install django-celery
apt-get install python-virtualenv
cd /usr/local/share/helios-server
virtualenv venv
source venv/bin/activate
apt-get install postgresql-server-dev-all
pip install -r requirements.txt

```

Aggiungo il modulo puppet per gestire le infrastrutture in azure [windowsazure#help][PuppetForge#windowsazure].

```bash
puppet module install msopentech-windowsazure
etckeeper commit "Add Microsoft Azure Puppet module for manage"


```


Step 4. Get the PE console user name.
Run sudo grep ‘auth_user_email’ /etc/puppetlabs/installer/answers.install. Then, locate the user name in the answer file at this setting: q_puppet_enterpriseconsole_auth_user_email.
The user name is admin@<VM name>.cloudapp.net. For example,admin@pe-demo.cloudapp.net.

Step 5. Get the console password.
Run sudo grep ‘auth_password’ /etc/puppetlabs/installer/database_info.install Locate the setting, q_puppet_enterpriseconsole_auth_password, which has the password appended to it.
It looks similar to this: q_puppet_enterpriseconsole_auth_password=thos9Greu.
Copy the password for use in step 6.

pe-demo01.cloudapp.net

## Bibliografia

[Puppet Labs][Puppet#01]: Sito ufficiale.


[Puppet#01]: http://puppetlabs.com/		"Puppet Labs"
[PuppetForce#01]: https://forge.puppetlabs.com "Puppet Forge"
[PuppetForge#windowsazure]: https://forge.puppetlabs.com/msopentech/windowsazure "msopentech/windowsazure"
[Puppet&Azure#docs01]: http://info.puppetlabs.com/pe-azure-gsg.html
[azure-cli#01]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_account_download.png
[azure-cli#02]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_account_import.png
[azure-cli#03]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_vm_list.png
[azure-cli#04]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_vm_start.png
[azure-cli#site]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/
[azure-cli#install]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#install
[azure-cli#joinaccount]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#configure
[azure-cli#use]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#use
[git-scm.com#ssh-keygen]:http://git-scm.com/book/en/Git-on-the-Server-Generating-Your-SSH-Public-Key
[OpenCPU]: https://www.opencpu.org/
[HELIOS]: http://documentation.heliosvoting.org/

