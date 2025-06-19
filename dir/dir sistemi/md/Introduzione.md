### Shell
La Shell è un interprete di comandi quindi un programma che serve ad eseguire altri comandi, ne esistono vari tipi:
- Thompson / Bourne shell: sh
- Bourne-Again shell: bash
- KornSheel: ksh

per verificare la versione della shell che stiamo usando: `echo $0`

### Prompt
Una volta aperta la shell questa scrive un prompt all’utente in attesa di ricevere un comando, un prompt tipico ha la seguente struttura:

- `nomeutente@nomemacchina:~cammino$`
	dove `cammino` indica il path dalla directory home fino alla directory attuale
	- la tilde ~, indica la home dir
	- ove non vi trovassimo in sub-dir della home avremmo l'absolute path

### Comandi
Ogni comando viene eseguito nel seguente modo: `comando [opzioni] argomenti_obbligatori`

- i parametri tra le parentesi quadre indicano argomenti opzionali
	- potremmo sceglierne più di uno contemporaneamente e quindi dovremmo separarli con un carattere speciale indicato dal comando
- se un argomento fra parentesi graffe ne deve essere almeno 1
- le flag:
	- sono composte da  `-char` oppure `--parola` che indicano la stessa cosa: `-r` è uguale a `--recursive`
	- alcune flag possono avere degli argomenti: `-k1`, `-k 1`, `--key=1` (same meaning)
	- se sono senza argomento possono essere raggruppabili: `-b -r -c` equivalente a `-brc`
	- esistono anche delle opzioni BSD-style[^1] che si scrivono senza trattino: `tar xfz nomefile.tgz`

[^1]: Berkeley Software Distribution

### User
- durante un'installazione di linux è necessario specificare almeno un utente[^2]
- _non tutti gli utenti possono fare login_
	- root[^3] non può fare il login, ma un altro utente può invece acquisire i poteri di root con `su` o `sudo`
		- se viene inserita una password sbagliata, il tentativo viene registrato
- gli utenti sono organizzati in gruppi, e ogni utente appartiene almeno a un gruppo
	- esistono gruppi definiti per scopi amministrativi `groups [nomeutente]`
<br>
- Creazione di altri utenti:
	 `adduser nuovoutente`[^4], di default l'utente creato non appartiene al gruppo sudo, si può fare solo da super user
	 oppure `useradd utente`:
		 non crea una cartella per l'utente e non richiede di impostare una password, che potremmo fare in un secondo momento con: `passwd utente` (non è molto comodo dal punto di vista della sicurezza dato che un utente dovrebbe poter accedere soltanto alla sua cartella)
- aggiungere un utente ad un gruppo
	`adduser nuovoutente gruppo`
- cambiare utente
	`su [ - | -l | --login] nomeutente`
- diventare root:
	`su -`
	`su - root`
	`su -l root`


[^2]: alcune versioni lo creano in automatico

[^3]: utente con poteri di amministratore (super user)

### Sudo
Gli utenti che possono eseguire il comando `sudo` sono definiti **sudoer** appartengono al gruppo predefinito _sudo_ e possono eseguire i comandi root

- il comando `sudo` prende come input un altro comando
![[dir/dir sistemi/asset/file.png]]

`sudo su -` dice che voglio diventare root user, chiede la pass dell'user attuale

`su`: substitute user, serve per cambiare utente nella shell, solitamente per accedere come root
- se non si specifica nessun user il comando proverà a cambiare all'utente `root`

per il log out: `exit` o `ctrl+b` 

`sudo`:
- runs a **single command** with root privileges
- only `apt update` is run as root
- you remain your current user
- environment variables are mostly preserved from your current user

- **use case:** when you want to run just one command as root

`sudo su`:
- runs the `su` command **as root**, effectively switching you to the **root user shell**
- you're now logged in as `root`
- any commands you run afterward will be executed as root
- you get the root user's environment

- **use case:** when you want to do multiple commands as root without typing `sudo` each time

ogni tentativo di accedere alla root viene registrato in `/var/log/auth.log`

[^4]: primarily available on Debian-based systems