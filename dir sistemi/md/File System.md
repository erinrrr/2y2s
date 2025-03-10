È un modo di organizzare la memoria basata su file e directory:
- directory contiene file e directory
	- struttura gerarchica ad albero
	- solo le directory possono avere figli
- file sono sequenze di bit e sono
	- regolari: ASCII, binari
	- non regolari: link 
	- dobbiamo conoscere come sono organizzati questi bit per poterli leggere

Linux ha un solo filesystem principale e la radice delle directory è `/` che si chiama **root**:
- tutti i file sono contenuti direttamente o indirettamente in `/`
- nella stessa directory è vietato creare:
	- 2 file/directory con lo stesso nome
	- un file ed una directory con lo stesso nome
- i nomi dei file e delle directory sono case sensitive: `file.txt` $\ne$ `File.txt`
- all’interno di una directory troviamo dei file nascosti[^1], iniziano con un punto, come:
	- `.bash_history`: Contiene i precedenti comandi usati nella shell
	- `.bashrc`: Contiene la configurazione della shell
- il percorso fino alla cartella utente è solitamente sostituito da `~`: - `/home/utente1/dir1/dir2/file.pdf` $=$ `~/dir1/dir2/file.pdf`
- il path assoluto è il percorso dalla root fino alla **cwd**
- il path relativo è il percorso dalla cwd fino a destinazione, quindi cambiando cwd potrebbe non essere più valido

Per cambiare il la cwd si usa il comando `cd [path]`:
- il comando `cd` senza argomento ci riporta a `~`
- `.`: Indica la cartella corrente
- `..`: Indica la cartella precedente 

Per creare una directory usiamo il comando `mkdir` seguito dal percorso della directory:

- `mkdir [OPTION] DIRECTORY`

Se vogliamo creare un intero path e creare anche le cartelle che mancano usiamo l’opzione `-p`

Per creare i file invece possiamo usare `touch nomefile` [^2]
che sarà un `.txt` di default 

Se vogliamo visualizzare ad albero tutte le directory ed i file che contengono possiamo usare l’opzione `-R` per il comando `ls` oppure installare una utility esterna chiamata `Tree`
	- `Tree`

[^1]: tipicamente config file, o file usati a supporto di comandi e applicazioni

[^2]: il vero utilizzo del comando touch è quello di modificare gli attributi temporali del file come data creazione, ultimo accesso in lettura ecc… però il comando se viene eseguito su un file che non esiste lo crea
