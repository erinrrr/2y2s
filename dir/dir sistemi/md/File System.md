## Organizzazione
È un modo di organizzare la memoria basata su file e directory:
- directory contiene file e directory
	- struttura gerarchica ad albero
	- solo le directory possono avere figli
- file sono sequenze di bit e sono
	- regolari: ASCII, binari
	- non regolari: link[^10]
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
- le directory sono file speciale che mappano nomi di file ai rispettivi inode

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

[^1]: tipicamente config file, o file usati a supporto di comandi e applicazioni

[^2]: il vero utilizzo del comando touch è quello di modificare gli attributi temporali del file come data creazione, ultimo accesso in lettura ecc… però il comando se viene eseguito su un file che non esiste lo crea

#### Mounting e partizioni
il filesystem root contiene elementi eterogenei:[^8]
- disco interno solido o magnetico
- filesystem su disco esterno (usb)
- filesystem di rete
- filesystem virtuali (usati dal kernel per gestire le risorse)
- filesystem in memoria principale

il meccanismo del mounting permette di trasformare una qualsiasi directory $D$ in un punto di mount per un nuovo filesystem $F$
- ciò avviene se e solo se la directory root di $F$ diventa accessibile da $D$
- se $D$ era vuota, dopo il mount conterrà esclusivamente $F$
- diversamente, i file preesistenti torneranno visibili solo dopo l’`unmount` di $F$

su linux, i dispositivi esterni sono accessibili tramite la cartella `/dev` e possono essere montati in una directory preesistente utilizzando il comando `mount`

Il comando `mount` se usato da solo ci mostra i filesystem montati nel sistema

un singolo disco può essere diviso in una o più partizioni:
 - una partizione $A$ può contenere il sistema operativo
 - mentre una partizione $B$ contiene i dati degli utenti
 - quindi ad esempio la partizione $A$ verrà montata su `/` mentre la partizione $B$ su `/home`

- nel caso in cui volessimo reinstallare il sistema operativo in $A$ non tocco i dati e rimonto $B$ in home
- o se volessimo installare una diversa lezione di Linux in un'altra partizione o un altro disco possiamo sempre montare $B$ su `/home` del nuovo filesystem senza dover ricopiare i dati 

#### Tipi di filesystem
![[dir/dir sistemi/asset/file 1.png]]
per l’utente finale il filesystem può indicare:
- la dimensione massima delle partizioni
- la dimensione massima dei file presenti su disco
- lunghezza massima dei nomi dei file
- se c’è journaling o no[^9]

per un programmatore invece indica il tipo di codifica dei dati
![[dir/dir sistemi/asset/file 2.png|dir sistemi/asset/file 2.png]]i principali FS di Windows sono:
- NTFS, MSDOS, FAT32, FAT64 - FAT e NTFS possono essere montati anche su Linux

diversi modi per visualizzare i file system montati:
- `mount`
- `cat /etc/mtab`
- `cat /etc/fstab` - questo visualizza i dischi da montare al boot del sistema
![[dir/dir sistemi/asset/file 3.png]]

#### Passwd e group
i file passwd e group si trovano in:
- `/etc/passwd` - contiene tutti gli utenti
- `/etc/group` - contiene tutti i gruppi

- hanno una struttura definita con la quale i programmatori possono “interfacciarsi” ad esempio come fa il comando `adduser`
- entrambi sono organizzate per _righe_:
	- una riga è una sequenza di caratteri che termina con un carattere speciale chiamato `line feed` o `LF` ed è il valore `0x0A`
	- ogni riga contiene dei campi separati da `:`
- il file `passwd` ha la seguente struttura:
	- `username:password:uid:gid:gecos:homedir:shell` 
	- dove al posto della password avremo una `x` per indicare che questa è cifrata
- il file `group` invece:
	- `groupname:password:groupID:listautenti` 
	- dove gli utenti nella lista sono separati da `,`
	- anche qui troveremo una password cifrata e quindi indicata da una `x`
- se una riga inizia per `#` significa che contiene un commento o che il suo contenuto non deve essere interpretato dalle applicazioni che usano il file

## Informazioni sui file
- ogni file del filesystem viene identificato da una struttura dati **inode**[^11] ed univocamente identificato da un _inode number_
- la cancellazione di un file libera l'inode number che verrà riutilizzato quando necessario per un nuovo file

un file ha diversi _attributi_:
- Typo - regular, block, fifo
- User ID - ID dell’utente proprietario
- Group ID - ID del gruppo a cui è associato il file
- Mode - permessi di accesso al file da parte di utenti e gruppi
- Size - dimensione in byte del file
- Timestamps:
    - mtime - indica l’ultima volta che il file è stato modificato (content modification time)
    - ctime - indica l’ultima volta che è stato modificato un attributo del file (inode changing time), non viene modificato nel caso di accesso in lettura al file
    - atime - indica l’ultima volta che è stato letto qualcosa dal file (content access time)
- Link count - numero di hard link che puntano a quel file
- Data Pointers - puntatore alla lista di blocchi che compongono il file, se si tratta di una directory allora il contenuto su disco è costituito da una tabella di 2 colonne: 
	- nome del file / directory
	- inode number

Ad esempio un filesystem ext2, è organizzato su disco nel seguente modo:
![[dir/dir sistemi/asset/file 4.png|dir sistemi/asset/file 4.png]]
la tabella inode si trova all'inizio del disco

i primi 1024 bytes del disco sono riservati per la partizione di boot e non vengono usati quindi dal filesystem, il resto è separato in diversi blocchi

per accedere a un file dobbiamo andare a leggere gli inode delle directory sul percorso, nell'immagine viene cercato il path: `/home/ealtieri/hello.txt`
![[dir/dir sistemi/asset/file 5.png]]
1. root si trova in inode 2 che ci porta al blocco 0 (leggiamo l'inode della directory root(`/`) per trovare l'inode della directory `/home`)
2. home si trova all’inode 13 che si trova al blocco 0
3. `ealtieri` si trova all’inode 31 sempre al blocco 0
4. troviamo il file `hello.txt` che è collegato all’inode 53

> gli inode mantengono “le informazioni” del file ovvero dove si trova mentre i blocchi contengono i dati veri e propri

con il comando `ls -i -l file`vediamo:
- flag `-i`: l'inode number del file 
- flag `-l`: 
	- diritti
	- il valore successivo indica:
		- per le directory quanti file contengono (contando anche `.` e `..`)
			- per i file questo vale 1
	- user
	- group
	- time
	- size
		- per le directory sarebbe la dimensione del file speciale che le descrive
		- quello contenente la tabella di coppie $(\text{nome\_file},\text{inode\_number})$
- il valore `total ...` in alto invece indica la dimensione della directory in blocchi su disco[^3], solo quella directory senza comprendere le sub-directory di cui verrà considerata solo la dimensione del file speciale
- flag `-n`: like -l, but list numeric user and group IDs
- flag `-l` +:
	- `-c`: timestamp ctime
	- `-u`: timestamp atime
	- senza niente mostra anche il timestamp mtime

[^3]: solitamente un blocco ha dimensione tra 1kB e 4kB

per le informazioni estese usare il comando `info ls` ad esempio

un altro comando per ricavare informazioni sui file è `stat [-c format] file`, che restituisce varie informazioni mentre con `stat -c %B filename` per ricavare la dimensione del blocco su disco che coincide con la dimensione di un settore di disco

## Permessi di accesso
#### File
- utente proprietario - chi crea il file/directory
- gruppo proprietario - gruppo primario dell'utente proprietario

il proprietario definisce i permessi di accesso:
- chi può leggere
- scrivere
- eseguire un file/directory

sono definiti dalla terna User-Group-Others ognuna delle quali può avere i valori `r,w,x`, esempio: ![[dir/dir sistemi/asset/file 6.png]]
- il primo carattere è `d` se è una directory
- altrimenti è un file e troviamo `-`

generalmente:
![[dir/dir sistemi/asset/file 7.png|dir sistemi/asset/file 7.png]]

#### Directory
![[dir/dir sistemi/asset/file 8.png]]

| perm | rappresentazione | significato pratico         | azioni possibili                                                                                                                                           |
| ---- | ---------------- | --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | `---`            | nessun accesso              | nulla: né leggere, né scrivere, né attraversare, nemmeno che la directory esiste                                                                           |
| 1    | `--x`            | solo attraversamento        | solo entrare nella dir (`cd`, accesso diretto sapendo il nome)                                                                                             |
| 2    | `-w-`            | solo scrittura              | inutile da solo (senza `x`, non serve a niente) (posso eliminare solo con sudo a quanto pare (boh))                                                        |
| 3    | `-wx`            | scrittura + attraversamento | **entri**, **crei**, **elimini** o **rinomini** file/dir all’interno (creare e cancellare file, ma non listare il contenuto) se si conosce il nome di essi |
| 4    | `r--`            | solo lettura<br>            | **listare la directory** (`ls`), ma non puoi entrare né modificare                                                                                         |
| 5    | `r-x`            | lettura + attraversamento   | puoi **elencare** e **entrare**, ma **non scrivere**, accesso ai file se si conosce il nome                                                                |
| 6    | `rw-`            | lettura + scrittura         | puoi **elencare** e **scrivere**? no: senza `x` non entri né modifichi                                                                                     |
| 7    | `rwx`            | tutti i permessi            | **tutto**: elenchi, entri, crei, elimini, rinomini                                                                                                         |

#### Permessi speciali
Possono essere applicati sia a file che a directory, essi sono:
- **sticky bit** - `t`
- **setuid bit** - `s`
- **setgid bit** - `s`

##### sticky bit
- inutile sui file
- applicato su una directory corregge il comportamento di `w+x` in modo da non permettere la cancellazione di file anche senza permessi sul file stesso

siano:
- $D$ una directory
- $U$ e $U’$ due utenti diversi
- $D$ appartiene a $U$
- $D$ non appartiene a $U’$ né al gruppo di $U’$
- $f$ un file in $D$
se $U’$ cerca di cancellare $f$ allora:
- senza sticky bit su $D$ sarà sufficiente avere i diritti di scrittura su $D$ anche se non li si hanno su $f$
- con lo sticky bit sono necessari anche i permessi di scrittura su $f$ per cancellarlo

##### setuid bit
- _si usa solo per file eseguibili_
- dato un file con il setuid bit settato, il file opererà con i _permessi del proprietario_ di quel file e _non dell’esecutore_ quindi:
	- se il proprietario è root il programma viene eseguito con privilegi di root indipendentemente dall’esecutore
- il comando `passwd` ha il setuid, permette a un utente di cambiare solo la propria password
	- l'utente root può cambiare la password di qualsiasi utente
	- il proprietario è root
- generalmente non vogliamo che questo comportamento sia possibile, poiché come root possiamo eliminare dei file su cui non abbiamo permessi di scrittura o di cui non siamo proprietari, comando `rm`
- i programmi con setuid implementano dei controlli interni per garantire che gli utenti non possano abusarne

##### setgid bit
- analogo al setuid ma con i gruppi
- i privilegi sono quelli del gruppo che è proprietario del file eseguibile
- può essere applicato ad una directory e allora tutti i file creati al suo interno ereditano il gruppo della directory, anziché il gruppo primario dell'utente che li crea

Per visualizzare gli attributi di accesso possiamo usare il comando `ls` o `stat`:
![[dir/dir sistemi/asset/file 9.png|dir sistemi/asset/file 9.png]]

I permessi speciali vengono visualizzati al posto del bit di execute(`x`):
- il `setuid` nella terna user
- il `setgid` nella terna group
- lo `sticky` nella terna other

caso:
- abbiamo i permessi di esecuzione (c'è `x`):
	- allora `s` o `t` saranno minuscoli
- non abbiamo i permessi di esecuzione
	- `s` o `t` saranno minuscoli
- ovviamente rispettando la cardinalità

permessi default:
- di un file appena creato sono `rw- r-- r--` = 666 - 022 (valore di umask generally)
- di una directory sono: `d rwx r-x r-x`

### Cambiare i permessi di un file
il comando `chmod mode [, mode...] filename` setta i diritti di accesso a file o directory, ci sono due modi:
- formato ottale:
	- 4 numeri tra 0 e 7 come la tabella vista prima
	- il primo numero indica setuid(4), setgid(2) e sticky(1)
	- gli altri sono per utente, gruppo e other
	- si possono fornire 3 numeri se si assume setuid, setgid e sticky settati
	- dobbiamo effettuare somme in binario per decidere i permessi speciali[^4]
- modalità simbolica:
	- format: `chmod [ugoa][+-=][perms...] filename`[^5]
		- **+**: aggiungere permessi
		- **-**: rimuovere permessi
		- **=**: assegnare permessi esplicitamente, sovrascrivendo quelli esistenti con quelli specificati
	- dove perms è:
		- zero 
		- una o più lettere `rwxXst` per i permessi[^6]
		- una lettera dell'insieme `{ugo}` 

- `X`: (execute condizionale), aggiunge `x` solo se:
	- il file è una directory
	- o se almeno un utente aveva già il permesso di esecuzione[^7]

[^4]: | Valore   | Significato                                   |
| -------- | --------------------------------------------- |
| 1XXX | Sticky Bit (`+t`) attivo                      |
| 2XXX | SetGID (`+s` sul gruppo) attivo               |
| 4XXX | SetUID (`+s` sul proprietario) attivo         |
| 6XXX | SetUID + SetGID attivi (4+2=6)                |
| 7XXX | SetUID + SetGID + Sticky Bit attivi (4+2+1=7) |

[^5]: `a = u+g+o` (all) la `a` tiene conto del masking (we don't know it yet)

[^6]: - r: lettura (read)
	- w: scrittura (write)
	- x: esecuzione (execute)
	- X: (execute condizionale), aggiunge `x` solo se il file è una directory o se almeno un utente aveva già il permesso di esecuzione[^7]
	- s: SetUID (per i file eseguibili) o SetGID (per i file o directory)
	- t: Sticky bit (per directory, ad esempio `/tmp`)

[^7]: si usa per:
	- rendere eseguibili solo le directory (ma non i file normali)
	- aggiungere `x` solo ai file che già lo avevano per qualcuno
	- non succede niente se un file non aveva il permesso di esecuzione per nessuno, `+X` non lo rende eseguibile


_esempi_
- per settare `rws r-S -w-` 
	- usiamo `chmod 6742 filename
	- in modalità simbolica: `chmod u=rS,g=rws,o=w filename` oppure:
	    - `chmod g+rwsx filename`
	    - `chmod u+rs filename`
	    - `chmod o+w filename

- per settare `rwx r-- -wT` usiamo `chmod 1742 filename`

###### Cambiare owner e gruppo di un file
- `chown [-R] proprietario {file}`
- `chgrp [-R] gruppo {file}`

- questi comandi possono essere usati solo da `root`
- l'opzione `-R` esegue il comando ricorsivo su tutte le sub-directory

[[Cheat sheet filesystem]]

[^8]: nella root convivono diverse tipologie di filesystem, ognuna con caratteristiche e scopi differenti

[^9]: Il journaling è una funzionalità avanzata dei filesystem progettata per aumentare l'affidabilità e ridurre il rischio di corruzione dei dati in caso di eventi imprevisti come spegnimenti improvvisi, crash o blackout

[^10]: i link sono dei riferimenti a file
	
	**hard link**:
	- un hard link è un altro nome per lo stesso file (cioè per lo stesso inode)
	- due (o più) hard link puntano allo stesso contenuto su disco
	- cancellare uno dei link non cancella i dati, finché almeno uno rimane
	- sono indistinguibili dal file originale
	  
	**soft link**:
	- un soft link è un file speciale che contiene il percorso a un altro file
	- funziona come un collegamento simbolico (come i collegamenti di Windows)
	- se il file originale viene cancellato, il link diventa "spezzato"

[^11]: (abbreviazione di _index node_) è una struttura dati che memorizza informazioni su un file o una directory, ad eccezione del nome del file e del suo contenuto effettivo
