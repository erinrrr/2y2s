Le due entità fondamentali in Linux sono:
- file - descrivono/rappresentano risorse
- processi - permettono di elaborare dati e usare le risorse

quando un file eseguibile è in esecuzione prende il nome di processo
- sono esempi di processi quelli creati eseguendo i comandi presenti in [[Cheat sheet filesystem]]
- eccezione per `echo` e `cd` che sono eseguiti all'interno del processo di shell
- anche la *bash* è un processo
- lo stesso file può dare vita a più processi (multitasking)

>ridirezione dell'output:
>possiamo usare i simboli `>` e `<` per redirigere l'output di un comando su di un file, con `ls > dirlist` andremo a scrivere l'output di `ls` nel file `dirlist`

### Rappresentazione dei processi
- **PID** - Processi Idetifier
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
		- Current Working DIrectory
		- Umask: file mode creation mask
		- Nice: static priority of the process
- **6 aree di memoria**
	- Text Segment: istruzioni da eseguire in linguaggio macchina
	- Data Segment: dati statici inizializzati e alcune costanti di ambiene
	- BSS: block started from symbol, dati statici non inizializzati
	- Heap: dati dinamici (allocati con `malloc` e simili)
	- Stack: chiamate a funzioni con i corrispondenti dati dinamici
	- MMS: Memory Mapping Segment, librerie esterne dinamiche usate dal processo, estensione dell'heap in alcuni casi

alcune aree di memoria potrebbero essere condivise:
- il text segment tra più istanze dello stesso processo
- stesso BSS o Data segment o MMS per due processi
- lo stack non è mai condiviso
![[dir/dir sistemi/asset/file 11.png]]

### Stati di un processo
- Running (R): in esecuzione su un processore
- Runnable (R): pronto per essere eseguito, in attesa che lo scheduler lo (ri)seleziona per l'esecuzione (non è in attesa di alcun evento)
- Sleep (interruptible)(S): in attesa di un evento e non può essere scelto dallo scheduler
- Zombie (Z): il processo è terminato ma il suo PCB viene ancora mantenuto dal kernel perché il processo padre non ha ancora richiesto il suo exit status
- Stopped (T): caso particolare di sleeping, ricevuto STOP, in attesa di CONT
- Traced (t): in esecuzione di debug, oppure in attesa di un segnale, altro caso particolare di sleeping
- Uninterruptible sleep (D)_ come sleep ma sta facendo operazioni di IO su dischi lenti e non può né essere ucciso né interrotto

### Modalità di esecuzione dei processi
- Foreground:
	- il comando può leggere l'input da tastiera e scrivere su schermo
	- finché non termina, il prompt non viene restituito e non si possono sottomettere altri comandi alla shell
	- (praticamente tutti i casi visti finora)
- Background
	- il comando non può leggere l'input da tastiera ma può scrivere su schermo
	- il prompt viene restituito immediatamente
	- mentre il job viene eseguito in background si possono dare altri comandi alla shell
<br>
- il comando `sleep` ci permette di generare un processo che si mette in pausa per un tempo specificato nell'argomento