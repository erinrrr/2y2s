- [[Prima lezione#Shell|Shell]]
- [[Prima lezione#Prompt|Prompt]]
- [[Prima lezione#Comandi|Comandi]]
- [[Prima lezione#User|User]]

### Shell
La Shell è un interprete di comandi quindi un programma che serve ad eseguire altri comandi, ne esistono vari tipi:
- Thompson / Bourne shell: sh
- Bourne-Again shell: bash
- KornSheel: ksh

### Prompt
Una volta aperta la shell questa scrive un prompt all’utente in attesa di ricevere un comando, un prompt tipico ha la seguente struttura:

- `nomeutente@nomemacchina:~cammino$`
	dove `cammino` indica il path dalla directory home fino alla directory attuale.
	- la tilde ~, indica la home dir, ove non vi trovassimo in sub-dir della home avremmo l'absolute path

### Comandi

Ogni comando viene eseguito nel seguente modo: `comando [opzioni] argomenti_obbligatori`

- i parametri tra le parentesi quadre indicano argomenti opzionali
	- potremmo sceglierne più di uno contemporaneamente e quindi dovremmo separarli con un carattere speciale indicato dal comando
- se un argomento fra parentesi graffe ne deve essere almeno 1
- le flag:
	- sono composte da  `-char` oppure `--parola` che indicano la stessa cosa: `-r` è uguale a `--recursive`
	- alcune flag possono avere degli argomenti: `-k1`, `-k 1`, `--key=1` (same meaning)
	- se sono senza argomento possono essere raggruppabili: `-b -r -c` equivalente a `-brc`
	- Esistono anche delle opzioni BSD-style[^1] che si scrivono senza trattino: `tar xfz nomefile.tgz`

[^1]: Berkeley Software Distribution

### User
- durante un'installazione di linux è necessario specificare almeno un utente[^2]
- non tutti gli utenti possono fare login
	- root[^3] non può fare il login, ma un altro utente può invece acquisire i poteri di root con `su` o `sudo`
- gli utenti sono organizzati in gruppi, e ogni utente appartiene almeno a un gruppo
	- esistono gruppi definiti per scopi amministrativi `groups [nomeutente]`

- Creazione di altri utenti:
	 `adduser nuovoutente`, di default l'utente creato non appartiene al gruppo sudo
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

#### Sudo
Gli utenti che possono eseguire il comando `sudo` sono definiti _sudoer_ appartengono al gruppo predefinito _sudo_ e possono eseguire i comandi root

- il comando `sudo` prende come input un altro comando
![[Pasted image 20250310125557.png]]


