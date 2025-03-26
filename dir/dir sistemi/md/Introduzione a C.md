- C è un linguaggio ad alto livello

## Ambiente di sviluppo e di esecuzione in C
- gcc (GNU Compiler Collection) è il compilatore C che svolge attività di pre-processamento, compilazione e linking
![[dir/dir sistemi/asset/file 12.png]]
1. nell'editor scriviamo il codice e lo memorizziamo nel disco
2. il pre-processamento ha tante funzioni, gestisce gli `include`, i condizionali, i commenti ecc, modella il codice già esistenti
3. il compiler prende l'output del processamento e genera del codice assembly, che viene assemblato generando il codice macchina, detto anche _object code_ (`.o`)
4. il linker:
	- unisce gli object code in un eseguibile
	- collega librerie esterne se utilizzate 
	- genera l'entry point per il programma (`main`)
	- esistono due tipi di linking:
		- statico: il codice esterno viene copiato all'interno del file finale
		- dinamico: il codice non viene compilato ma viene scritto il nome della libreria cui fa riferimento
![[dir/dir sistemi/asset/file 13.png]]
per quanto riguarda l'esecuzione:
5. il loader prende il programma dal disco e lo carica in RAM
6. la CPU esegue tutte le istruzioni quando possibile

quindi le fasi e i tool per creare ed eseguire un programma in C sono:
- edit (editor di testo)
- pre-process (gcc)
- compile (gcc)
- link (gcc)
- load (SO)
- execute (SO)

##### Differenze con Java e con Python
- la compilazione di un programma in Java con javac produce codice interpretabile dalla JVM, contenuto nel `.class`
	- quando voglio eseguire un `.class` lo devo dare in pasto alla JVM, quindi il processo in esecuzione è la JVM
- mentre la compilazione di un programma C con gcc produce come visto prima "object code" che è un file eseguibile
	- il processo creato sarà indipendente da gcc, quindi posso eseguirlo anche su altri sistemi senza ricompilarlo
	- stessa cosa per C++

### Struttura di un programma C

- Main function: il punto da cui parte il programma quindi dove
	- dove risiede tutto il codice
	- oppure dove vi sono le chiamate di altre funzioni
- funzioni: blocchi di codice che svolgono determinate azioni, indicate da un nome univoco

possono entrambe trovarsi all'interno dello stesso file `.c`

#### Functions
ogni funzione è composta da header e basic block
- _header_:
```C 
<return-type> fn-name (parameter-list)
	basic block
```

- _basic block_:
	- `;` usato per terminare uno statement
	- un blocco può contenere 0 o più statement
	- i blocchi possono essere innestati
```C 
{
	declaration of variables
	executable statements
}
```

- return statement:
	- `return expression`
	- ritorna al caller/invoker il valore di ritorno in base a un espressione

### Compilazione, esecuzione, precompilazione, compilazione e linking
- compilazione: `gcc -Wall program-name.c`
	- aggiungiamo `-lm` se ci sono librerie esterne che vogliamo includere
	- vengono stampati i messaggi di warning se presenti
	- il risultato è un file eseguibile `a.out`
		- per specificarlo: `gcc -Wall program-name.c -o executable-name.o`
- precompilare senza eseguire: `cpp program-name.c > precompiled.c`
	- elimina commenti 
	- esegue le direttive del compilatore
- compilazione di un precompilato: `gcc -c precompiled.c -o output.o`
	- controlla la sintassi
	- controlla che esista l'header corrispondente per ogni chiamata a funzione
	- crea il codice macchina, ma solo per il contenuto delle funzioni
	- ogni chiamata a funzione ha una destinazione simbolica, finché non facciamo il linking
- precompilazione + compilazione: `gcc -c file.c -o file.o`
- linking: `gcc file.o`
	- risolve tutte le chiamate a funzione, ora ogni funzione chiamata avrà bisogno del corrispettivo basic block oltre all'header
	- l'implementazione può essere data dal programmatore o da fornita da librerie di sistema, come per `printf`
	- l'inclusione delle librerie può essere automatica o specificata dall'utente
	- può essere eseguito anche su più file contemporaneamente: `gcc file1.o file2.o file3.o` (si possono combinare file `.c` e file `.o`)

#### Direttive al preprocessore
Una riga preceduta da `#` indica una direttiva per il preprocessore:
- `#include filename`: include il contenuto di `filename` al posto di `#`
- questi tipi di file solitamente sono `.h` e sono detti header file
- `#include <stdio.h>`
	- `<>` indica che il file header è un file standard di C
	- `""` indica che il file header è dell'utente e si trova nella directory corrente
	- `-I` indica di specificare le directory in cui cercare gli header file