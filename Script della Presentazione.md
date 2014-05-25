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
apt-get install git etckeeper

cd /etc
git init
vi /etc/etckeeper/etckeeper.conf

git config --global user.name "Administrator"
git config --global user.email "root@localhost.localnets"
git config -l

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



## Bibliografia

[Puppet Labs][Puppet#01]: Sito ufficiale.


[Puppet#01]: http://puppetlabs.com/		"Puppet Labs"
[PuppetForce#01]: https://forge.puppetlabs.com "Puppet Forge"
[azure-cli#01]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_account_download.png
[azure-cli#02]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_account_import.png
[azure-cli#03]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_vm_list.png
[azure-cli#04]: https://raw.githubusercontent.com/giulianolatini/DevCamp2014/master/img/azure_vm_start.png
[azure-cli#site]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/
[azure-cli#install]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#install
[azure-cli#joinaccount]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#configure
[azure-cli#use]: http://azure.microsoft.com/en-us/documentation/articles/xplat-cli/#use
[git-scm.com#ssh-keygen]:http://git-scm.com/book/en/Git-on-the-Server-Generating-Your-SSH-Public-Key

