> [!WARNING] possibile domanda d'esame
> con l'aiuto del mount cercare di capire le varie opzioni di mounting dei vari file system

#### Esercizio 1
- creare l'intero albero di directory `home/utente1/dir1/dir3/dir7` e `file1`, `file3`, `file7` :
	- `mkdir dir1`
		- `touch dir1/file1`
	- `mkdir dir1/dir3`
		- `touch dir1/dir3/file3`
	- `mkdir dir1/dir3/dir7`
		-  `touch dir1/dir3/dir7/file3`
- `dir3` diventa di `utente3`
- tutti i file dentro `dir3` diventano di `utente2`
- `dir7` diventa di `utente2` e file7 diventa di `utente3`
- provare a cambiare i permessi di `file3` e `dir7`

#### Esercizio 2
- verificare che `utente3` non sia presente in `/etc/passwd`,
- crearlo
- controllare che sia presente
- verificare che non sia nel gruppo sudo
- aggiungerlo al gruppo sudo
- controllare che ora sia presente

#### Esercizio 3
- creare un file, aprirlo e modificarlo
	- `touch file`
	- `gedit file`
	- se volessimo editarlo con nano: `nano file`
- leggerlo
	- verificare che i valori per atime ed mtime cambiano

#### Esercizio 4
- capire perché il comando `adduser` va usato solo da un utente amministratore, mentre il comando `groups` no
	- il comando `adduser` deve andare a modificare il file di sistema come `passwd`, che sono protetti e possono essere modificati solo dall'utente root, inoltre non ha il `setuid` quindi viene eseguito con i privilegi dell'utente attuale.
	- il comando `groups` invece serve solo a leggere i gruppi a cui appartiene un utente, le informazioni sui gruppi sono contenute nel file `/etc/group`, che ha permessi di lettura per tutti `-rw-r--r--`, anche per il file `passwd` abbiamo gli stessi permessi, non possiamo scrivere

#### Esercizio 5
- creare un file e settare i permessi a `rws r-S -w-`
- poi settarli a `rwx r-- -wT`
- togliere il permesso di lettura ad un file e poi provare ad aprirlo con gedit/vi
- verificare che il comando `chmod` modifica il ctime del file

#### Esercizio 6
- creare cartella con e copiarci `file.script`
 - eseguire il comando `bash file.script`
 - eseguire `ls -lR`
	 - ci sono 8 file, uno per ogni permesso, e 8 directory, una per ogni permesso all'interno di ciascuna directory, si ripete lo schema: 8 file e 8 directory, ciascuna di queste ultime con dentro 8 file
	 - testare i vari permessi, come sono riportati nelle tabelle introdotta precedentemente. Ad esempio, provare a leggere un file solo eseguibile, o ad appendere testo ad un file solo leggibile, o ad attraversare una directory senza permesso di esecuzione..

#### Esercizio 7
- provare a fare un esempio funzionante di sticky bit.
A tal proposito, creare da root (usando sudo) una directory avente tutti i diritti per tutti i gruppi, e crearci dentro un file, togliendo a quest'ultimo i permessi per il gruppo “other"; infine, provare a cancellarlo. Fare lo stesso dopo aver aggiunto lo sticky bit alla directory 

#### Esercizio 8
- provare a fare un esempio funzionante di setuid bit. 
A tal proposito, copiate /bin/cat in una directory, e abilitategli il setuid bit. Create un file da root (usando sudo), e dategli il solo permesso di lettura per il solo gruppo “owner". Provare a vedere il contenuto di questo file usando il cat di sistema: fallira. Ora provate a farlo usando ./cat..