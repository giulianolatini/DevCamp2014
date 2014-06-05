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



![azure vm list][azure-cli#03]

![azure vm start][azure-cli#04]

### Script di installazione

```code

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

Aggiungo la parte pubblica della chiave appena generata (`id_rsa.pub`) al db che openssh-server utilizza per ricavare le parti pubbliche utilizzate per decodificare la richiesta di connessione. Se la parte pubblica della propria chiave è presente, la decodifica della richiesta di accesso (codificata con la propria parte privata) ha successo, l'accesso alla shell è consentito senza la richiesta di password, l'esempio classico d'uso di questa modalità è lo scripting di attività da un sistema ad un altro remoto.

```code
cd ~/.ssh
cat id_rsa.pub >> authorized_keys

cd /etc
git remote add origin root@localhost:/var/git/localconf_etc.git
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

```code
apt-get install vim-addon-manager vim-puppet vim-scripts
apt-get install zsh
vi /etc/passwd # Modifica della shell chiamata al login
apt-get install curl
curl -L https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh | sh
apt-get install git-flow
apt-get install python-pip
apt-get install exuberant-ctags
apt-get install byobu
```

```vim
set nu!
set runtimepath=~/.vim-scripts,/usr/share/vim-scripts,$VIMRUNTIME
autocmd FileType html setlocal shiftwidth=2 tabstop=2
autocmd FileType python setlocal expandtab shiftwidth=4 softtabstop=4
autocmd FileType css setlocal expandtab shiftwidth=2 softtabstop=2
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

```code
update_system='sudo apt-get update; sudo etckeeper commit "Aggiornamento Quotidiano"; sudo apt-get upgrade; sudo apt-get dist-upgrade; sudo apt-get check; sudo apt-get autoremove; sudo apt-get autoclean'
```


Aggiungo il modulo puppet per gestire le infrastrutture in azure [windowsazure#help][PuppetForge#windowsazure].

```code
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

