Le due entità fondamentali in Linux sono:
- **file** - descrivono/rappresentano risorse
- **processi** - permettono di elaborare dati e usare le risorse

>quando un file eseguibile è in esecuzione prende il nome di processo

- sono esempi di processi quelli creati eseguendo i comandi presenti in [[Cheat sheet filesystem]]
- eccezione per `echo` e `cd` che sono eseguiti all'interno del processo di shell
- anche la *bash* è un processo
- lo stesso file può dare vita a più processi (multitasking)
- linux è multi-processo

>redirezione dell'output:
>possiamo usare i simboli `>` e `<` per redirigere l'output di un comando su di un file, con `ls > dirlist` andremo a scrivere l'output di `ls` nel file `dirlist`

> [!info]+ file descriptor standard
> | Nome                | Abbreviazione | Numero | Descrizione                                       |
| ------------------- | ------------- | ------ | ------------------------------------------------- |
| **Standard Input**  | `stdin`       | `0`    | Da dove legge il programma (di default: tastiera) |
| **Standard Output** | `stdout`      | `1`    | Dove va l’output normale (di default: schermo)    |
| **Standard Error**  | `stderr`      | `2`    | Dove va l’output di errore (di default: schermo)  | 
>
> - redirigere lo **stdout** in un file:
> 	`comando > output.txt` equivalente a `comando 1> output.txt`
>
> - redirigere lo **stderr** in un file:
> 	`comando 2> errori.txt`
> 
> - redirigere **stdout** e **stderr** insieme:
> 	`comando > tutto.txt 2>&1`
> 	- `> tutto.txt` → invia stdout nel file
> 	- `2>&1` → invia stderr **allo stesso posto** di stdout
> 
> - redirezione dello **stdin**:
>     `comando < input.txt` equivale a `comando 0< input.txt`
>     il comando leggerà i dati da `input.txt` invece che dalla tastiera
>     si può redigerlo da un file o da un altro comando
> 
>> [!NOTE]- esempi di redirezione
> > - `ls > dirlist`
> > 	- output di `ls` ridirezoinato in `dirlist`
> > - `2>&1 ls > dirlist`
> > 	- `stderr` ridirezionato in  `stdout`
> > 	- `stdout` di `ls` ridirezionato su `dirlist` dopo che `stderr` è stato redirezionato e quindi `dirlist` contiene solo lo `std` di `ls`
> > - `ls > dirlist 2>&1`
> > 	- output di `ls` ridirezionato in `dirlist`
> > 	- `stderr` ridirezionato in `stdout`
> > 	- quindi sia `stderr` che `stdout` sono ridirezionati su `dirlist`
> > - `ls 2>/dev/null` ??
> > - `2>/dev/null ls` ??

### Rappresentazione dei processi
- **PID** - Process Identifier
	- identificatore univoco di un processo
	- in un dato istante non ci possono essere 2 processi con lo stesso PID
	- terminato un processo il suo PID viene liberato
- **PCB** - Process Control Block
	- unico per ogni processo
	- contiene varie informazioni:
		- PID
		- PPID: Parent Process Identifier
		- Real UID: Real User identifier
		- Real GID: Real Group Identifier
		- Effective UID: Effective User ID (UID assunto dal processo in esecuzione)
		- Effective GID
		- Saved UID: Saved User ID (UID avuto prima dell'esecuzione del SetUID)
		- Saved GID
		- Current Working Directory
		- Umask: file mode creation mask
		- Nice: static priority of the process
	- differenza tra RUID, EUID, SUID:
		- RUID:
			- utente che ha lanciato il processo
			- serve per i controlli di accesso normali
		- EUID:
			- utente con cui il processo opera realmente
			- determina i privilegi effettivi (es. accesso ai file)
		- SUID:
			- quando attivo su un file eseguibile:
		    - alla sua esecuzione:
			    - il EUID diventa il proprietario del file, non il chiamante
			    - mentre il RUID è id di chi lo esegue
			- usato per far concedere privilegi temporanei

- **6 aree di memoria**
	- Text Segment: istruzioni da eseguire in linguaggio macchina
	- Data Segment: dati statici inizializzati e alcune costanti di ambiente
	- BSS: block started from symbol, dati statici non inizializzati
	- Heap: dati dinamici (allocati con `malloc` e simili)
	- Stack: chiamate a funzioni con i corrispondenti dati dinamici
	- MMS: Memory Mapping Segment, tutto ciò che riguarda librerie esterne dinamiche usate dal processo, estensione dell'heap in alcuni casi

alcune aree di memoria potrebbero essere condivise:
- il text segment tra più istanze dello stesso processo
- stesso BSS o Data segment o MMS per due processi
- lo stack non è mai condiviso
![[dir/dir sistemi/asset/file 11.png]]

### Stati di un processo
- **Running** (R): in esecuzione su un processore
- **Runnable** (R): 
	- pronto per essere eseguito
	- in attesa che lo scheduler lo (ri)seleziona per l'esecuzione
	- non è in attesa di alcun evento
- **Sleep** (interruptible)(S):
	- in attesa di un evento
	- non può essere scelto dallo scheduler
- **Zombie** (Z):
	- il processo è terminato
	- le sue 6 aree di memoria non sono più in memoria
	- il suo PCB viene ancora mantenuto dal kernel perché il processo padre non ha ancora richiesto il suo "exit status"
- **Stopped** (T):
	- caso particolare di sleeping
	- ha ricevuto un segnale STOP, in attesa di CONT
- **Traced** (t):
	- in esecuzione di debug, oppure in attesa di un segnale
	- altro caso particolare di sleeping
- **Uninterruptible sleep** (D):
	- come sleep ma sta facendo operazioni di IO su dischi lenti
	- non può né essere ucciso, né interrotto

### Modalità di esecuzione dei processi
- **Foreground**:
	- il comando può leggere l'input da tastiera e scrivere su schermo
	- finché non termina, il prompt non viene restituito e non si possono sottomettere altri comandi alla shell
	- (praticamente tutti i casi visti finora)
- **Background**
	- il comando non può leggere l'input da tastiera ma può scrivere su schermo
	- il prompt viene restituito immediatamente
	- mentre il job viene eseguito in background si possono dare altri comandi alla shell
<br>
- il comando `sleep` ci permette di generare un processo che si mette in pausa per un tempo specificato nell'argomento
	- possiamo scegliere la modalità, di base viene lanciato in foreground
	- se inseriamo `&` viene inserito in background
<br>
- possiamo vedere il numero di job in esecuzione con `jobs [-l] [-p]`
	- `bg`: porta un processo in background
	- `fg`: porta un processo in foreground
	- facendo `CTRL + Z` stoppiamo un processo, lo risvegliamo con `bg` o `fg`
	- `kill %job_number` termina un processo in background
- si possono identificare job anche con:
	- `%prefix` dove prefix è la parte iniziale del job desiderato (`%sl` per sleep)
	- `%+` oppure `%%` per l'ultimo job mandato
	- `%-` per il penultimo job mandato
<br>
- possiamo eseguire un job composto da più comandi contemporaneamente tramite il **pipelining**: 
	- `comando1 | comando2 | ... comando n`
	- lo standard output di un comando $i$ diventa l'input del comando $i+1$
	- se uso `|&` ridireziono lo standard error sullo standard input del comando successivo
	- il comando `i+1` non deve necessariamente usare l'output/stderror del comando `i` quindi quello precedente

--- 
###### Alcuni comandi
- `ps [opzioni] [pid]`: display information about a selection of the active processes
	- `-e`: tutti i processi di tutti gli utenti 
	- `-u {userlist}`: tutti i processi degli utenti specificati
	- `-p {pidlist}`: tutti i processi con PID nella lista specificata
	- `-o {field}`: visualizza solo alcuni campi:
		- PPID
		- C: parte intera della percentuale di uso della CPU
		- STIME: l’ora o la data in cui è stato invocato il comando
		- TIME: tempo di uso della CPU finora
		- CMD: comando con argomenti
		- F: flags associati al processo
		- S: modalità del processo
		- UID: utente che ha lanciato il processo
		- PRI: attuale priorità del processo
		- NI: valore di nice da aggiungere alla priorità
		- ADDR: indirizzo di memoria del processo
		- SZ: dimensione totale del processo in numero di pagine sia in memoria che su disco
		- WCHAN: se il processo è in attesa di un qualche segnale o in sleep
<br>
- `top` è un `ps` interattivo:
	- `-b`: non accetta più comandi interattivi ma continua a fare refresh ogni pochi secondi
	- `-n num`: effettua `num` refresh
	- `-p`: funziona come in `ps
	- da aperto `?` per avere una lista di comandi accettati
<br>
- `kill [-l [signal]] [-signal] [pid...]`: send a signal to a process
	- `-l`: mostra la lista dei segnali
	- un segnale è identificato dal numero oppure da nome `+ [SIG]`
	- i segnali saranno presi in considerazione se il real user corrisponde con chi invia il segnale, o se lo invia un superuser
	- alcuni segnali sono:
		- `SIGSTOP, SIGSTP`: sospensione processo (inviabile con `ctrl + z`)
		- `SIGCONT`: continuazione di processi stoppati
		- `SIGKILL, SIGINT`: terminazione processi (`SIGINT` = `ctrl + c`)
		- `bg` invia `SIGCONT` al job indicato
		- `SIGUSR1` e `SIGUSR2` sono impostati per essere usati dall’utente per le proprie necessità, consentono una semplice forma di comunicazione tra processi [^1]
<br>
- `nice [-n num] [comando]`: run a program with modified scheduling priority
	- usato senza opzioni dice quant'è il *niceness* di partenza, ossia il valore che va aggiunto alla priorità del processo
		- default impostato a $0$
		- varia da $-19$ a $+20$
	- `nice comando` lancia comando con niceness uguale a 0 o a `num` se dati
- `renice priority {pid}`:
	- interviene su processi già in esecuzione
- `strace [-p pid] [comando]`:
	- lancia un comando visualizzando tutte le sue system call
	- oppure visualizza system call del processo `pid`
	- `-o filename` ridireziona l'output su un file
	- utile per il debug dei programmi che utilizzano system call


[^1]: ad esempio in un programma P1 possiamo assegnare un pezzo di codice a `SIGUSR1` e se un programma P2 invia un `SIGUSR1` a P1 questo eseguirà quel codice
