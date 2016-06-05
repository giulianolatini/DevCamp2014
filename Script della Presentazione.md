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
# nel caso vada popolato un repository github o altro servizio cloud
git branch --set-upstream-to=origin/master master
git pull
git push
# oppure
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
sudo apt-get -y install locate vim-addon-manager vim-youcompleteme vim-puppet vim-scripts vim-nox zsh curl git-flow build-essential cmake python-dev python-pip exuberant-ctags byobu
sudo curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
sudo pip install git+git://github.com/Lokaltog/powerline
sudo apt-get remove vim-tiny

sudo chsh -s $(which zsh) 
sudo updatedb
# Comandi Singoli

sudo apt-get install vim-addon-manager vim-puppet vim-scripts vim-syntax-go vim-nox
sudo apt-get remove vim-tiny
sudo apt-get install locate
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
sudo updatedb
```
Nel caso il sistema non sia localizzato in UTF8 (come le caso di una installazioen non localizzata, 
vedi contenuto del file:
```bash
cat /etc/default/locale
--- /etc/default/locale ---
LANG=C.UTF-8
LC_ALL=C.UTF-8
LANGUAGE=C.UTF-8
--- END ---
```
Se il file contiene il valore LANG=C e non LANG=C.UTF-8 esso va reimpostato nel file e Riavviato il sistema, usando il comando
```bash
update-locale
```
si effettua il check per verificare la corretta assegnazione delle variabili di localizzazione.

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
plugins=(go golang docker docker-compose postgres \
git ssh-agent git-flow git-remote-branch github git-extras git-prompt gitfast gitignore \ 
sudo rsync python pip \
tmux tmuxinator ubuntu urltools vim-interaction colored-man-pages man colorize history \
zsh_reload zsh-navigation-tools zsh-syntax-highlighting)

# User configuration
# The next configuration path for go language
export GOROOT="/usr/local/go"
export GOPATH="/root/Sviluppo/go"

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
#zstyle :omz:plugins:ssh-agent identities id_rsa latini.giuliano@gmail.com
zstyle :omz:plugins:ssh-agent identities id_rsa

# Setup Powerline
# how install with command: sudo pip install --user git+git://github.com/Lokaltog/powerline 
. /usr/local/lib/python2.7/dist-packages/powerline/bindings/zsh/powerline.zsh

# Setup oh-my-zsh
source $ZSH/oh-my-zsh.sh

if [ -f ~/.zsh/zshalias ]; then
    source ~/.zsh/zshalias
else
    print "404: ~/.zsh/zshalias not found."
fi

# ssh adding others keys
export SSH_KEYS_PATH="/root/.ssh"
#ssh-add $SSH_KEYS_PATH/test-pp02.key
#ssh-add $SSH_KEYS_PATH/test-pp03.key
```

Configurazione di ViM

Installazione Pacchetti PATHOGEN:
```bash
mkdir -p ~/.vim/autoload/ ; mkdir -p ~/.vim/bundle/ ; cd ~/.vim/autoload/  
wget https://raw.githubusercontent.com/tpope/vim-pathogen/master/autoload/pathogen.vim  
```
Installazione filebrowser interno a Vim
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
Installazione colorscheme ~/.vim/color/zenburn.vim
```vim
" Vim color file
" Maintainer:   Jani Nurminen <slinky@iki.fi>
" URL:          http://kippura.org/zenburnpage/
" License:      GNU GPL <http://www.gnu.org/licenses/gpl.html>
"
" Nothing too fancy, just some alien fruit salad to keep you in the zone.
" This syntax file was designed to be used with dark environments and
" low light situations. Of course, if it works during a daybright office, go
" ahead :)
"
" Owes heavily to other Vim color files! With special mentions
" to "BlackDust", "Camo" and "Desert".
"
" To install, copy to ~/.vim/colors directory.
"
" Alternatively, you can use Vimball installation:
"     vim zenburn.vba
"     :so %
"     :q
"
" For details, see :help vimball
"
" After installation, use it with :colorscheme zenburn.
" See also :help syntax
"
" Credits:
"  - Jani Nurminen - original Zenburn, maintainer
"  - Steve Hall & Cream posse - higher-contrast Visual selection
"  - Kurt Maier - 256 color console coloring, low and high contrast toggle,
"                 bug fixing
"  - Charlie - spotted too bright StatusLine in non-high contrast mode
"  - Pablo Castellazzi - CursorLine fix for 256 color mode
"  - Tim Smith - force dark background
"  - John Gabriele - spotted bad Ignore-group handling
"  - Zac Thompson - spotted invisible NonText in low contrast mode
"  - Christophe-Marie Duquesne - suggested making a Vimball,
"    suggested support for ctags_highlighting.vim
"  - Andrew Wagner - noted the CursorColumn bug (guifg was unintentionally set),
"                    unify CursorColumn colour
"  - Martin Langasek - clarify the license, whitespace fixes
"  - Marcin Szamotulski - support autocomplete for Zenburn configuration
"                         parameters
"  - Clayton Parker (claytron) - Convinced by Kurt Maier to use Zenburn. Point
"    out issues with LineNr, fix directory styles, and their usage in MacVim.
"  - Paweł Piekarski - Spotted bad FoldColumn and TabLine. Made better 
"                      FoldColumn colors, fixed TabLine colors.
"  - Jim - Fix for missing Include group for terminal
"  - Peter (Sakartu) - ColorColumn fixes
"  - Please see git log for the others not listed here
"
" CONFIGURABLE PARAMETERS:
"
" You can use the default (don't set any parameters), or you can
" set some parameters to tweak the Zenburn colours.
"
" To use them, put them into your .vimrc file before loading the color scheme,
" example:
"    let g:zenburn_high_Contrast=1
"    colors zenburn
"
" You can also do ":let g:zenburn" then hit Ctrl-d or Tab to scroll through the
" list of configurable parameters.
"
" * You can now set a darker background for bright environments. To activate, use:
"      let g:zenburn_high_Contrast = 1
"
" * For transparent terminals set the background to black with:
"      let g:zenburn_transparent = 1
"
" * For example, Vim help files uses the Ignore-group for the pipes in tags
"   like "|somelink.txt|". By default, the pipes are not visible, as they
"   map to Ignore group. If you wish to enable coloring of the Ignore group,
"   set the following parameter to 1. Warning, it might make some syntax files
"   look strange.
"
"      let g:zenburn_color_also_Ignore = 1
"
" * To get more contrast to the Visual selection, use
"
"      let g:zenburn_alternate_Visual = 1
"
"   Note: this is enabled only if the old-style Visual
"   if used, see g:zenburn_old_Visual
"
" * To use alternate colouring for Error message, use
"
"      let g:zenburn_alternate_Error = 1
"
" * The new default for Include is a duller orange. To use the original
"   colouring for Include, use
"
"      let g:zenburn_alternate_Include = 1
"
" * To disable underlining for Labels, use
"
"      let g:zenburn_disable_Label_underline = 1
"
" * Work-around to a Vim bug, it seems to misinterpret ctermfg and 234 and 237
"   as light values, and sets background to light for some people. If you have
"   this problem, use:
"
"      let g:zenburn_force_dark_Background = 1
"
" * By default the CursorColumn is of a lighter colour. I find it more readable
"   that way, but some people may want to align it with the darker CursorLine
"   color, for visual uniformity. To do so, use:
"
"      let g:zenburn_unified_CursorColumn = 1
"
"   Note: you can ignore this unless you use
"   ":set cursorline cursorcolumn", since otherwise the effect won't be
"   seen.
"
" * New (dark) Visual coloring has been introduced.
"   The dark Visual is more aligned with the rest of the colour scheme,
"   especially if you use line numbers. If you wish to use the 
"   old Visual coloring, use
"
"      let g:zenburn_old_Visual = 1
"
"   Default is to use the new Visual.
"
"  * EXPERIMENTAL FEATURE: Zenburn would like to support TagHighlight
"    (an evolved ctags-highlighter) by Al Budden (homepage:
"    http://www.cgtk.co.uk/vim-scripts/taghighlight).
"    Current support status is broken: there is no automatic detection of
"    TagHighlight, no specific language support; however there is some basic
"    support for Python. If you are a user of TagHighlight and want to help,
"    please enable:
"
"      let g:zenburn_enable_TagHighlight=1
"
"    and improve the corresponding block at the end of the file.
"
" NOTE:
"
" * To turn the parameter(s) back to defaults, use UNLET or set them to 0:
"
"      unlet g:zenburn_alternate_Include
"   or 
"      let g:zenburn_alternate_Include = 0
"
"
" That's it, enjoy!
"
" TODO
"   - Visual alternate color is broken? Try GVim >= 7.0.66 if you have trouble
"   - IME colouring (CursorIM)

" Finish if we are in a term lacking 256 color support
if ! has("gui_running") && &t_Co <= 255
    finish
endif

" Set defaults, but keep any parameters already set by the user
if ! exists("g:zenburn_high_Contrast")
    let g:zenburn_high_Contrast = 0
endif

if ! exists("g:zenburn_transparent")
    let g:zenburn_transparent = 0
endif

if ! exists("g:zenburn_color_also_Ignore")
    let g:zenburn_color_also_Ignore = 0
endif

if ! exists("g:zenburn_alternate_Error")
    let g:zenburn_alternate_Error = 0
endif

if ! exists("g:zenburn_force_dark_Background")
    let g:zenburn_force_dark_Background = 0
endif

if ! exists("g:zenburn_alternate_Visual")
    let g:zenburn_alternate_Visual = 0
endif

if ! exists("g:zenburn_alternate_Include")
    let g:zenburn_alternate_Include = 0
endif

if ! exists("g:zenburn_disable_Label_underline")
    let g:zenburn_disable_Label_underline = 0
endif

if ! exists("g:zenburn_unified_CursorColumn")
    let g:zenburn_unified_CursorColumn = 0
endif

if ! exists("g:zenburn_old_Visual")
    let g:zenburn_old_Visual = 0
endif

if ! exists("g:zenburn_enable_TagHighlight")
    let g:zenburn_enable_TagHighlight = 0
endif

" -----------------------------------------------

set background=dark

hi clear
if exists("syntax_on")
    syntax reset
endif
let g:colors_name="zenburn"

hi Boolean         guifg=#dca3a3                              ctermfg=181
hi Character       guifg=#dca3a3 gui=bold                     ctermfg=181 cterm=bold
hi Comment         guifg=#7f9f7f gui=italic                   ctermfg=108
hi Conditional     guifg=#f0dfaf gui=bold                     ctermfg=223 cterm=bold
hi Constant        guifg=#dca3a3 gui=bold                     ctermfg=181 cterm=bold
hi Cursor          guifg=#000d18 guibg=#8faf9f gui=bold       ctermfg=233 ctermbg=109 cterm=bold
hi Debug           guifg=#bca3a3 gui=bold                     ctermfg=181 cterm=bold
hi Define          guifg=#ffcfaf gui=bold                     ctermfg=223 cterm=bold
hi Delimiter       guifg=#8f8f8f                              ctermfg=245
hi DiffAdd         guifg=#709080 guibg=#313c36 gui=bold       ctermfg=66  ctermbg=237 cterm=bold
hi DiffChange      guibg=#333333                              ctermbg=236
hi DiffDelete      guifg=#333333 guibg=#464646                ctermfg=236 ctermbg=238
hi DiffText        guifg=#ecbcbc guibg=#41363c gui=bold       ctermfg=217 ctermbg=237 cterm=bold
hi Directory       guifg=#9fafaf gui=bold                     ctermfg=109 cterm=bold
hi ErrorMsg        guifg=#80d4aa guibg=#2f2f2f gui=bold       ctermfg=115 ctermbg=236 cterm=bold
hi Exception       guifg=#c3bf9f gui=bold                     ctermfg=249 cterm=bold
hi Float           guifg=#c0bed1                              ctermfg=251
hi FoldColumn      guifg=#93b3a3 guibg=#3f4040
hi Folded          guifg=#93b3a3 guibg=#3f4040
hi Function        guifg=#efef8f                              ctermfg=228
hi Identifier      guifg=#efdcbc                              ctermfg=223 cterm=none
hi IncSearch       guifg=#f8f893 guibg=#385f38                ctermfg=228 ctermbg=23
hi Keyword         guifg=#f0dfaf gui=bold                     ctermfg=223 cterm=bold
hi Macro           guifg=#ffcfaf gui=bold                     ctermfg=223 cterm=bold
hi ModeMsg         guifg=#ffcfaf gui=none                     ctermfg=223 cterm=none
hi MoreMsg         guifg=#ffffff gui=bold                     ctermfg=231 cterm=bold
hi Number          guifg=#8cd0d3                              ctermfg=116
hi Operator        guifg=#f0efd0                              ctermfg=230
hi PmenuSbar       guibg=#2e3330 guifg=#000000                ctermfg=16  ctermbg=236
hi PmenuThumb      guibg=#a0afa0 guifg=#040404                ctermfg=232 ctermbg=151
hi PreCondit       guifg=#dfaf8f gui=bold                     ctermfg=180 cterm=bold
hi PreProc         guifg=#ffcfaf gui=bold                     ctermfg=223 cterm=bold
hi Question        guifg=#ffffff gui=bold                     ctermfg=231 cterm=bold
hi Repeat          guifg=#ffd7a7 gui=bold                     ctermfg=223 cterm=bold
hi Search          guifg=#ffffe0 guibg=#284f28                ctermfg=230 ctermbg=22
hi SignColumn      guifg=#9fafaf gui=bold                     ctermfg=109 cterm=bold
hi SpecialChar     guifg=#dca3a3 gui=bold                     ctermfg=181 cterm=bold
hi SpecialComment  guifg=#82a282 gui=bold                     ctermfg=108 cterm=bold
hi Special         guifg=#cfbfaf                              ctermfg=181
hi SpecialKey      guifg=#9ece9e                              ctermfg=151
hi Statement       guifg=#e3ceab gui=none                     ctermfg=187 cterm=none
hi StatusLine      guifg=#313633 guibg=#ccdc90                ctermfg=236 ctermbg=186
hi StatusLineNC    guifg=#2e3330 guibg=#88b090                ctermfg=235 ctermbg=108
hi StorageClass    guifg=#c3bf9f gui=bold                     ctermfg=249 cterm=bold
hi String          guifg=#cc9393                              ctermfg=174
hi Structure       guifg=#efefaf gui=bold                     ctermfg=229 cterm=bold
hi Tag             guifg=#e89393 gui=bold                     ctermfg=181 cterm=bold
hi Title           guifg=#efefef gui=bold                     ctermfg=255 ctermbg=NONE cterm=bold
hi Todo            guifg=#dfdfdf guibg=NONE    gui=bold       ctermfg=254 ctermbg=NONE cterm=bold
hi Typedef         guifg=#dfe4cf gui=bold                     ctermfg=253 cterm=bold
hi Type            guifg=#dfdfbf gui=bold                     ctermfg=187 cterm=bold
hi Underlined      guifg=#dcdccc gui=underline                ctermfg=188 cterm=underline
hi VertSplit       guifg=#2e3330 guibg=#688060                ctermfg=236 ctermbg=65
hi VisualNOS       guifg=#333333 guibg=#f18c96 gui=bold,underline ctermfg=236 ctermbg=210 cterm=bold
hi WarningMsg      guifg=#ffffff guibg=#333333 gui=bold       ctermfg=231 ctermbg=236 cterm=bold
hi WildMenu        guifg=#cbecd0 guibg=#2c302d gui=underline  ctermfg=194 ctermbg=236 cterm=underline

" spellchecking, always "bright" term background
hi SpellBad   guisp=#bc6c4c guifg=#dc8c6c  ctermfg=209 ctermbg=237
hi SpellCap   guisp=#6c6c9c guifg=#8c8cbc  ctermfg=103 ctermbg=237
hi SpellRare  guisp=#bc6c9c guifg=#bc8cbc  ctermfg=139 ctermbg=237
hi SpellLocal guisp=#7cac7c guifg=#9ccc9c  ctermfg=151 ctermbg=237

if exists("g:zenburn_high_Contrast") && g:zenburn_high_Contrast
    " use new darker background
    hi Normal        guifg=#dcdccc guibg=#1f1f1f           ctermfg=188 ctermbg=234
    hi ColorColumn   guibg=#33332f                         ctermbg=235
    hi CursorLine    guibg=#121212 gui=bold                ctermbg=233 cterm=none
    hi CursorLineNr  guifg=#f2f3bb guibg=#161616           ctermfg=229 ctermbg=233
    if exists("g:zenburn_unified_CursorColumn") && g:zenburn_unified_CursorColumn
        hi CursorColumn  guibg=#121212 gui=bold            ctermbg=233 cterm=none
    else
        hi CursorColumn  guibg=#2b2b2b                     ctermbg=235 cterm=none
    endif
    hi FoldColumn    guibg=#161616                         ctermbg=233 ctermfg=109
    hi Folded        guibg=#161616                         ctermbg=233 ctermfg=109
    hi LineNr        guifg=#9fafaf guibg=#161616           ctermfg=248 ctermbg=233
    hi NonText       guifg=#404040 gui=bold                ctermfg=238
    hi Pmenu         guibg=#242424 guifg=#ccccbc           ctermfg=251 ctermbg=235
    hi PmenuSel      guibg=#353a37 guifg=#ccdc90 gui=bold  ctermfg=187 ctermbg=236 cterm=bold
    hi MatchParen    guifg=#f0f0c0 guibg=#383838 gui=bold  ctermfg=229 ctermbg=237 cterm=bold
    hi SignColumn    guibg=#181818                         ctermbg=233
    hi SpecialKey    guibg=#242424
    hi TabLine       guifg=#88b090 guibg=#313633 gui=none  ctermbg=236 ctermfg=108 cterm=none
    hi TabLineSel    guifg=#ccd990 guibg=#222222           ctermbg=235 ctermfg=186 cterm=bold
    hi TabLineFill   guifg=#88b090 guibg=#313633 gui=none  ctermbg=236 ctermfg=108 cterm=none
else
    " Original, lighter background
    hi Normal        guifg=#dcdccc guibg=#3f3f3f           ctermfg=188 ctermbg=237
    hi ColorColumn   guibg=#484848                         ctermbg=238
    hi CursorLine    guibg=#434443                         ctermbg=238 cterm=none
    hi CursorLineNr  guifg=#d2d39b guibg=#262626           ctermfg=230 ctermbg=235
    if exists("g:zenburn_unified_CursorColumn") && g:zenburn_unified_CursorColumn
        hi CursorColumn  guibg=#434343                     ctermbg=238 cterm=none
    else
        hi CursorColumn  guibg=#4f4f4f                     ctermbg=239 cterm=none
    endif
    hi FoldColumn    guibg=#333333                         ctermbg=236 ctermfg=109
    hi Folded        guibg=#333333                         ctermbg=236 ctermfg=109
    hi LineNr        guifg=#9fafaf guibg=#262626           ctermfg=248 ctermbg=235
    hi NonText       guifg=#5b605e gui=bold                ctermfg=240
    hi Pmenu         guibg=#2c2e2e guifg=#9f9f9f           ctermfg=248 ctermbg=235
    hi PmenuSel      guibg=#242424 guifg=#d0d0a0 gui=bold  ctermfg=187 ctermbg=235 cterm=bold
    hi MatchParen    guifg=#b2b2a0 guibg=#2e2e2e gui=bold  ctermfg=145 ctermbg=236 cterm=bold
    hi SignColumn    guibg=#343434                         ctermbg=236
    hi SpecialKey    guibg=#444444
    hi TabLine       guifg=#d0d0b8 guibg=#222222 gui=none  ctermbg=235 ctermfg=187 cterm=none
    hi TabLineSel    guifg=#f0f0b0 guibg=#333333 gui=bold  ctermbg=236 ctermfg=229 cterm=bold
    hi TabLineFill   guifg=#dccdcc guibg=#101010 gui=none  ctermbg=233 ctermfg=188 cterm=none

    hi StatusLine    ctermbg=144
endif

if exists("g:zenburn_force_dark_Background") && g:zenburn_force_dark_Background
    " Force dark background, because of a bug in VIM:  VIM sets background
    " automatically during "hi Normal ctermfg=X"; it misinterprets the high
    " value (234 or 237 above) as a light color, and wrongly sets background to
    " light.  See ":help highlight" for details.
    set background=dark
endif

if exists("g:zenburn_transparent") && g:zenburn_transparent
    hi Normal             ctermbg=0     guibg=#000000
    hi Statement          ctermbg=NONE
    hi Title              ctermbg=NONE
    hi Todo               ctermbg=NONE
    hi Underlined         ctermbg=NONE
    hi DiffAdd            ctermbg=NONE
    hi DiffText           ctermbg=NONE
    hi ErrorMsg           ctermbg=NONE
    hi LineNr             ctermbg=NONE
endif

if exists("g:zenburn_old_Visual") && g:zenburn_old_Visual
    if exists("g:zenburn_alternate_Visual") && g:zenburn_alternate_Visual
        " Visual with more contrast, thanks to Steve Hall & Cream posse
        " gui=none fixes weird highlight problem in at least GVim 7.0.66, thanks to Kurt Maier
        hi Visual          guifg=#000000 guibg=#71d3b4 gui=none  ctermfg=16  ctermbg=79  cterm=none
        hi VisualNOS       guifg=#000000 guibg=#71d3b4 gui=none  ctermfg=16  ctermbg=79  cterm=none
    else
        " use default visual
        hi Visual          guifg=#233323 guibg=#71d3b4 gui=none  ctermfg=235 ctermbg=79  cterm=none
        hi VisualNOS       guifg=#233323 guibg=#71d3b4 gui=none  ctermfg=235 ctermbg=79  cterm=none
    endif
else
    " new Visual style
    if exists("g:zenburn_high_Contrast") && g:zenburn_high_Contrast
        " high contrast
        "hi Visual        guibg=#304a3d
        "hi VisualNos     guibg=#304a3d
        "TODO no nice greenish in console, 65 is closest. use full black instead,
        "although i like the green..!
        hi Visual        guibg=#0f0f0f  ctermbg=232
        hi VisualNOS     guibg=#0f0f0f  ctermbg=232
        if exists("g:zenburn_transparent") && g:zenburn_transparent
            hi Visual ctermbg=235
        endif
    else
        " low contrast
        hi Visual        guibg=#2f2f2f  ctermbg=235
        hi VisualNOS     guibg=#2f2f2f  ctermbg=235
    endif
endif

if exists("g:zenburn_alternate_Error") && g:zenburn_alternate_Error
    " use more jumpy Error
    hi Error    guifg=#e37170 guibg=#664040 gui=bold  ctermfg=210 ctermbg=52 cterm=bold
else
    " default is something more zenburn-compatible
    hi Error    guifg=#e37170 guibg=#3d3535 gui=bold  ctermfg=167 ctermbg=236 cterm=bold
endif

if exists("g:zenburn_alternate_Include") && g:zenburn_alternate_Include
    " original setting
    hi Include  guifg=#ffcfaf gui=bold                ctermfg=223 cterm=bold
else
    " new, less contrasted one
    hi Include  guifg=#dfaf8f gui=bold                ctermfg=180 cterm=bold
endif

if exists("g:zenburn_disable_Label_underline") && g:zenburn_disable_Label_underline
    hi Label    guifg=#dfcfaf                         ctermfg=187
else
    hi Label    guifg=#dfcfaf gui=underline           ctermfg=187 cterm=underline
endif

if exists("g:zenburn_color_also_Ignore") && g:zenburn_color_also_Ignore
    " color the Ignore groups
    " note: if you get strange coloring for your files, turn this off (unlet)
    if exists("g:zenburn_high_Contrast") && g:zenburn_high_Contrast
        hi Ignore                ctermfg=238
    else
        hi Ignore guifg=#545a4f  ctermfg=240
    endif
endif

" EXPERIMENTAL TagHighlight support
" link/set sensible defaults here;
"
" For now I mostly link to subset of Zenburn colors, the linkage is based
" on appearance, not semantics. In later versions I might define more new colours.
"
" HELP NEEDED to make this work properly.

if exists("g:zenburn_enable_TagHighlight") && g:zenburn_enable_TagHighlight
        " CTag support may vary, but the first step is to start using it so
        " we can fix it!
        "
        " Consult /plugin/TagHighlight/data/kinds.txt for info on your
        " language and what's been defined.
        "
        " There is potential for language indepedent features here. (Acutally,
        " seems it may be required for this to be useful...) This way we can
        " implement features depending on how well CTags are currently implemented
        " for the language. ie. Global problem for python is annoying.  Special
        " colors are defined for special language features, etc..
        "
        " For now all I care about is python supported features:
        "   c:CTagsClass
        "   f:CTagsFunction
        "   i:CTagsImport
        "   m:CTagsMember
        "   v:CTagsGlobalVariable
        "
        "   Note: TagHighlight defaults to setting new tags to Keyword
        "   highlighting.

        " TODO conditionally run each section
        " BEGIN Python Section
        hi link Class        Function
        hi link Import       PythonInclude
        hi link Member       Function
        "Note: Function is already defined

        " Highlighter seems to think a lot of things are global variables even
        " though they're not. Example: python method-local variable is
        " coloured as a global variable. They should not be global, since
        " they're not visible outside the method.
        " If this is some very bright colour group then things look bad.
        " hi link GlobalVariable    Identifier

        " Because of this problem I am disabling the feature by setting it to
        " Normal instead
        hi link GlobalVariable Normal
        " END Python Section

        " Starting point for other languages.
        hi link GlobalConstant    Constant
        hi link EnumerationValue  Float
        hi link EnumerationName   Identifier
        hi link DefinedName       WarningMsg
        hi link LocalVariable     WarningMsg
        hi link Structure         WarningMsg
        hi link Union             WarningMsg
endif

" TODO check for more obscure syntax groups that they're ok
```

Configurazione ViM Estesa per sviluppo software (lingiaggi supportati: Puppet, Google Go, Python)
```vim
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
" Adding PaperColor
Plugin 'NLKNguyen/papercolor-theme'

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

syntax enable
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
set t_Co=256   " This is may or may not needed.

"set background=light
"set background=dark
"colorscheme PaperColor
"colorscheme material-theme
colorscheme zenburn
let g:zenburn_force_dark_Background=1

let g:molokai_original = 1
"let g:rehash256 = 1

" Autostart NERDTreeToggle
"au VimEnter * NERDTreeToggle
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
set rtp+=/usr/local/lib/python2.7/dist-packages/powerline/bindings/vim

set background=dark
set laststatus=2
set encoding=utf-8
set fillchars+=stl:\ ,stlnc:\

"set term=xterm-256color
set termencoding=utf-8

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

Alias di aggiornamento completo del sistema
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

