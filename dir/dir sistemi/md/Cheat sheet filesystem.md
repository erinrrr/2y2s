![[dir/dir sistemi/asset/file 10.png]]

- `umask` definisce i permessi di default che vengono assegnati ai file e alle directory quando vengono creati, codifica ottale
	- `000` indica nessuna restrizione sui permessi
	- `777` massimo delle restrizioni per le directory, `666` per i file poiché non viene settato il diritto di esecuzione (le operazioni speciali non hanno effetto)
	- con `umask` di `022` avremo :
		- file = `666` - `022` = standard - `umask` = `644`
		- directory = `777` - `022` = standard - `umask` = `755`

- `cp [-r] [-i] [-a] [-u] {source} dest`
	- copy file or directory
	- `-r` recursive per le directory
	- `-i` interactive per essere avvisati in caso di sovrascrittura
	- `-u` update, consente la sovrascrittura solo nel caso in cui `mtime` della source è più recente di quello della destinazione

- `mv [-i] [-u] [-f] {source} dest`
	- move files or rename nel caso source e dest sono nella stessa dir ma con nomi diversi
		- move: `mv file1 /path/to/destination/`
		- rename: `mv file1 file2`
	- `-i` interactive
	- `-u` update
	- `-f` force, forza il comando senza chiedere conferma, anche se il file di destinazione esiste già, sovrascrive senza alcun avviso

- `rm [-f] [-i] [-r] {file}`
	- `f` force, forza la cancellazione senza chiedere conferma, non esiste il cestino
	- `r` recursive
	- `i` interactive

- `ln [-s] source [dest]`
	- Soft/symbolic e hard link
	- un link $l_{1}$ ad un file $f_{1}$ è un nuovo file che punta al file di destinazione $f_{1}$, è un puntatore, permettendo così di risparmiare spazio
		- con i soft link creiamo un nuovo file con un inode diverso
		- gli hard link non hanno il concetto di puntatore e puntato ma abbiamo lo stesso inode
	- di default hard link
	- `-s` soft link

- `touch [-a] [-m] [-t timestamp] {file}`
	- serve per modificare i timestamp di un file
	- se però non esiste lo crea
	- può essere applicato anche su directory
	- `-a`: change only the access time
	- `-m`: change only the modification time
	- `-t`: setta il timestamp desiderato

- `du [-c] [-s] [-a] [-h] [--exclude=PATTERN] [files...]`
	- estimate file space usage e/o directories passate come argomento
	- `-c`: total, mostra anche il totale dello spazio occupato alla fine dell'output.
	- `-s`: summarize, mostra solo il totale dello spazio occupato da ogni directory, senza elencare i singoli file al suo interno
	- `-a`: all, mostra lo spazio elencato da tutti i file e non solo dalle directory
	- `-h`: human-readable: mostra le dimensioni in un formato leggibile,KB, MB, GB invece che in blocchi di $1024$ byte
	- `--exclude=PATTERN`: esclude file o directory che corrispondono al pattern specificato

- `df [-h] [-l] [-i] [file]` (disk free)
	- mostrare lo spazio disponibile e utilizzato nei filesystem montati
	- `-h`: human-readable
	- `-l`: local, mostra solo i filesystem locali (esclude quelli di rete come NFS)
	- `-i`: inodes, mostra il numero di inode invece dello spazio su disco
	- `file`, se specificato, mostra solo lo spazio del filesystem in cui si trova quel file o directory

- `dd [opzioni]`
	- convert and copy a file
		- spesso usato per creare immagini di dischi, copiare dispositivi di blocco (pendrive, hard disk) e per la gestione avanzata dei file
		- oltre che per le conversioni si usa per copiare file speciali che non possono essere copiati con `cp`
	- le opzioni sono una sequenza `variabile=valore`, le più importanti sono:
		- `bs=SIZE`, imposta la dimensione del blocco di lettura/scrittura
		- `count=N`, copia solo i primi `N` blocchi di dati
		- `convert=`, il valore in questo caso specifica una conversione
		- `if=`: input file, specifica il file di input se non dato legge da tastiera
		- `of=`: output file, specifica il file di output se non dato scrive su schermo
	- potrebbe essere utile per:
		- cancellare dati completamente da un supporto di memoria
		- preparare un file ad essere formattato
		- copiare una parte di un file

- `mkfs [-t type fsoptions] device`
	- build a linux filesystem on device, su un dispositivo di archiviazione
		- spesso chiamato formattazione perché organizza lo spazio su disco secondo una struttura specifica, rendendolo pronto per memorizzare file in un determinato formato
	- `-t`: specifica il tipo di filesystem (es. `ext4`, `xfs`, `vfat`)
	- ` device`: device su cui creare il filesystem, device file in `/dev` oppure file regolare, creato con `dd`
		- se è un device non deve essere montato
	- alternativa `/sbin/mkfs`
	- distrugge tutti i dati esistenti sulla partizione/dispositivo