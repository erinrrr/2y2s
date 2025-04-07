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

## Struttura di un programma C

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

## Compilazione, esecuzione, precompilazione, compilazione e linking
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

## Input e Output
input 
- letto da tastiera, o da un altro dispositivo e memorizzato in variabili

output
- visualizzato a monitor o inviato a un altro dispositivo, prendono valori da variabili
- `printf("format string", value-list)`: comando per mandare in output del testo
	- value-list può contenere:
		- sequenze di caratteri
		- variabili
		- costanti
		- operazioni logico-matematiche
	- `printf` riceve valori. ma permette di manipolare anche indirizzi di memoria e passarli come input a funzioni
	- `scanf`: per stampare il contenuto di una locazione di memoria di cui si consce l'indirizzo
	- possiamo usare dei caratteri speciali speciali per gestire la spaziatura
![[dir/dir sistemi/asset/file 14.png|250]]


- tutte le funzioni essenziali per l'I/O sono nel file `stdio.h`
	- l'ambiente run-time di $C$ quando esegue un programma apre i file:
		- `stdin`
		- `stdout`

## Variabili ed espressioni
- vi sono gli identificatori che sono parole utilizzate per identificare una variabile o costante
- vi sono anche però dei vincoli in quanto vi sono parole riservate che fanno parte dell'alfabeto del linguaggio
![[dir/dir sistemi/asset/file 15.png|450]]
regole per costruire identificatori validi:
- il primo carattere deve essere:
	- una lettere
	- o un underscore
	- e può essere seguito solo da qualsiasi lettera, numero o underscore
- case-sensitive, lettere maiuscole e minuscole sono distinte
- non consentito:
	- virgole
	- spazi vuoi
	- le parole chiave
	- iniziare con delle cifre
	- avere un identificatore con lunghezza superiore a 31 caratteri

### Variabili
le variabili sono locazioni di memoria dove vengono memorizzati valori
- possono contenere un solo valore per volta
- ma esso può essere modificato più volte durante il ciclo di vita del programma
esistono diversi modi per dichiarare le variabili:
- parametri: dichiarate nell'header delle funzioni
- globali: dichiarate fuori dalle funzioni
- locali: all'interno di blocchi di funzioni
	- all'inizio della funzione[^1]
	- prima di essere usate[^2]

[^1]: vantaggi:
	- historical context: convenzione del linguaggio
	- memory allocation: permette al compilatore di allocare tutta la memoria necessaria una sola volta, memory layout più semplice e può essere ottimizzato più facilmente
	- scope visibility: scopo chiaro e facili da trovare
	- error prevention: preveniamo errori del tipo usare variabili non ancora allocate
	- code clarity: codice più facile da leggere e da mantenere
	svantaggi:
	- meno chiaro dove vengono inizializzate e utilizzate per la prima volta
	- promuove l'uso di variabile per più scopi, non un ottima abitudine

[^2]: vantaggi:
	- riduce la loro vita il più possibile
	- limita il riuso
	svantaggi:
	- non mostrare le variabili in un unico punto

#### Dichiarazione di variabili e costanti
- `optional_modifier data_type name_list;`
	- `optiona_modifier`:
		- signed, unsigned
		- short, long
		- const
	- `data_type`:
		- specifichiamo il tipo di valore:
			- `int x;`,`double y;`, `char c;` $\dots$
	- `name _list`:
		- lista di nomi di variabili
		- buona norma avere un nome che descrive al funzione della variabile

##### Assegnazione di valori
- quando una variabile viene dichiarata e non inizializzata assume un valore indeterminato
- l'assegnazione di un valore può essere fatta:
	- in fase di dichiarazione: `int x = 3, y = 2;`, `double dd = 9.3;` 
	- in fase di elaborazione con l'operatore di assegnazione `=`
	- leggendo un valore in input, ad esempio con `scanf`
- `=` è un operatore binario:
	- `l_value = r_value`
		- `l_value`: solitamente una variabile o un puntatore
		- `r_value`: valore o un espressione
	- è possibile fare una doppia assegnazione: `a = b = 0;` [^3]
	- un espressione è un qualcosa del tipo: `x = y + 5`

#### data type for numbers:
- whole number (Integer):
	- short
	- int
	- long
- real numbers (Floating-point)
	- float
	- double
	- long double

![[dir/dir sistemi/asset/file 16.png|450]]

| Type        | Storage Size | Value Range                     | Precision         |
| ----------- | ------------ | ------------------------------- | ----------------- |
| float       | 4 byte       | 1.2E - 38 to  <br>3.4E + 38     | 6 decimal places  |
| double      | 8 byte       | 2.3E - 308 to  <br>1.7E + 308   | 15 decimal places |
| long double | 10 byte      | 3.4E - 4932 to  <br>1.1E + 4932 | 19 decimal places |


###### Char
- per i caratteri si usa il tipo `char`
- occupano 1 byte
- possiamo rappresentare tutti i caratteri ASCII
	- https://www.asciitable.com/

###### Boolean
- il tipo `_Bool` può memorizzare solo `0` e `1`
	- valori diversi da 0 vengono memorizzati come 1
- `bool` richiede l'uso di `<stdbool.h>`
	- memorizza `true` e `false`
- in ogni espressione logica
	- `0` significa `false`
	- `1` (o qualsiasi valore da 0) significa `true`

[^3]: verrà fatto prima `b = 0` poi il valore di `b` viene assegnato ad `a`


##### Output di variabili e placeholder
- con `printf` possiamo scrivere il valore di una variabile su `stdout`
- `printf(format_string, expression_list)`
	- `format_string`: deve contenere dei _placeholder_
	- iniziano con `%` e indicano che al loro posto ci andrà:
		- il valore di una variabile
		- e il tipo di dato che deve essere scritto
	- ad esempio `print("%d, %l, integerVariable, longVariable);`
	- placeholder più comuni per integer e float
		- `%d` o `%i`: Integer
		- `%l`: long
		- `%x`: per integers in esadecimale
		- `%o`: integers in ottale
		- `%f`,`%e`,`%g`: float, nello specifico:
		    - `f`: formato standard
		    - `e`: notazione scientifica
		    - `g`: automatico
		- `%lf`: per double
	- formato completo di un placeholder:
		- `%[parameter] [flags] [width] [.precision] [length] type`

#### Scanf
- `scanf(format-string, address-list)`
	- `format-string`:
		- contiene placeholder che dicono a `scanf` in che tipo di dato la stringa in input viene convertita
	- `address-list`:
		- contiene gli indirizzi di memoria in cui devono essere memorizzati i valori ricevuti in input
- esempio `scanf("%d", &var);`
	- `var`: è una variabile intera
	- `&`: estrae il suo indirizzo di memoria e lo passa a `scanf`

##### Operatori
- aritmetici:
	- `+`, `-`, `*`, `/`, `%`
		- per `/`, il risultato dipende dai tipi di dato degli operandi, è lo stesso tipo di dato dell'operando più grande in termini di tipo di dato
- relazionali:
	- `==`, `!=`, `<`, `<=`, `>`, `>=`
- logici:
	- `!`, `&&`, `||`
- bitwise:
	- `&`, `|`, `~`, `^`
- shift
	- `<<`, `>>`

- troncamento: `int x;`, `x = 3.14;`, `x` conterrà solo 3, non da warning

- `++x`: pre-incremento
- `x++`: post-incremento

## Loop

while:
- condizione valutata prima di entrare nel loop
- `while (expression){basic block}`: expression è un valore `true` o `false`
- LCV: loop control variable è la variabile il cui valore controlla la ripetizione del loop
	- deve essere dichiarata e inizializzata prima del loop
	- testata nella condizione del loop
	- aggiornata nel body del loop

for:
- il numero di iterazioni è noto a priori (anche se può essere modificato)
- `for (initialization; test; increment) {basic block}`

do-while:
- simile al while ma la condizione è testata alla fine
- il body è eseguito almeno una volta
- `do{statements} while (condition);`


*break statement*:
- si usa per uscire da un ciclo
- si usa anche per uscire dallo `switch`
- quando viene eseguito il `break`, il ciclo termina indipendentemente dal valore della condizione
_continue statement_:
- il controllo passa all'iterazione successiva


## Stringhe
- le stringhe in $C$ sono un array (speciale) di caratteri
- ogni elemento dell'array è un carattere della stringa
- ultimo elemento dell'array è il carattere di fine stringa `\0`, detto anche `NULL`

inizializzazione:
- `char s[10] = "Lezione 9";`![[dir/dir sistemi/asset/file 17.png]]
	- `\0` inserito automaticamente
- `char r[10] = {"L","9", " ", "4", "a", "p", "r"};`
	- è un errore! poiché non viene inserito `\0` in maniera automatica quindi non è una stringa
- facendo `char r[10] = "L9 4apr"` il valore `r[8]` è indeterminato
- mentre facendo `char r[] = "L9 4 apr"` viene allocata una stringa di dimensione 8

gestione delle stringhe:
- nella libreria `string.h` sono presenti diverse funzioni comode per poter gestire le stringhe
- lunghezza: `size_t strlen(const char *s);`
- copia
	- creare una copia: `char *strcpy(char *dest, const char *src);`
	- copiare al più n byte `char *strncpy(char *dest, const char *src, size_t n);`
- confronto:
	- `int strcmp(const char *s1, const char *s2);`
	- confronta i primi $n$ byte di $s1$ e $s2$ `int strcmp(const char *s1, const char *s2, size_t n);`
	- `strcmp()` restituisce un valore che indica la relazione tra le due stringhe:
		- $value >0 \Rightarrow s1>s2$
		- $value =0 \Rightarrow s1=s2$
		- $value <0 \Rightarrow s1<s2$
- concatenazione:
	- `char *strcat(char *dest, const char *src);`
	- concatenati al più i primi $n$ byte `char *strncat(char *dest, const char *src, size_t n);`

input/output
- oltre a `printf` con placeholder `%s`, abbiamo opzioni più flessibili per I/O
- output
	- `int putchar(int c);`
		- stampa a schermo un singolo carattere (restituisce il carattere stampato o errore)
- `int puts(const char *s)`
	- scrive su `stdout` una stringa, inoltre aggiunge il carattere di nuova riga `\n` e non mostra il carattere null `\0`
- `int fputs(const char *s, FILE *stream)`
	- scrive la stringa `s` sullo stream indicato, non scrive il carattere di fine stringa `\0`
- input
	- `char *gets(char *s);` (deprecated)
		- legge una linea da `stdin` e la memorizza in `s`
	- `char *fgets(char *s, int size, FILE *stream)`
		- legge i caratteri dal flusso indicato posizionandosi fino al primo carattere di nuova riga incluso e ne salva il contenuto in `s`
	- `int getchar(void)`
		- legge un carattere da `stdin` e lo restituisce in output


## Puntatori e allocazione dinamica della memoria
- i **puntatori** sono variabili che contengono l’indirizzo di una locazione di memoria: `<tipo> *var_name;`
- **valore diretto**:
	- indirizzo di un'altra cella di memoria
	- vi si accede mediante il nome della variabile
- **valore indiretto**:
	- è il valore contenuto nella cella di memoria il cui indirizzo è memorizzato nel puntatore

> [!info] esempio
> ```C
> int *ptr = &x;   // ptr contiene l'indirizzo di x
> printf("%p\n", ptr);   // valore diretto → stampa l’indirizzo (es. 0x4d56)
printf("%d\n", *ptr);  // valore indiretto → stampa il valore che si trova nell'indirizzo (se all'indirizzo `0x4d56` c'è `4`, allora `*ptr = 4`)
> ```

**operatori**:
- operatore `*`
	- assegna alla locazione di memoria puntata dalla variabile il valore a dx dell'uguale
- operatore `&`
	- assegna alla variabile a sx dell'uguale l'indirizzo della variabile `&var`
> [!info] esempio
> ```C
> numPtr =& num; //assegna a numPtr l’indirizzo di num
• *numPtr = 10; //assegna alla locazione di memoria puntata da numPtr il valore 10
> ```


**aritmetica dei puntatori**:
- un puntatore contiene un indirizzo quindi un numero
- ne deriva che possiamo applicare alcune operazioni aritmetiche 
```C
int *ptr;
ptr = ptr + 1; //ptr viene incrementato di 4 locazioni di memoria
ptr = ptr + n; //ptr viene incrementato di 4*n locazioni di memoria
```


**vettori e puntatori**:
```C
int vect[10];
int *ptr = NULL;
ptr = &vect[0]; //puntatore al primo elemento
ptr = vect; //puntatore al vettore
```
- in C, il nome dell'array (`vect`) si comporta come un puntatore al primo elemento (`&vect[0]`)
- infatti _puntatore al primo elemento_ e _puntatore al vettore_ sono la stessa cosa


## Allocazione dinamica
- vettori e altre variabili sono allocate nello Stack a compile-time
- possiamo allocare memoria a run-time, essa verrà allocata nell' Heap
```C
void *calloc(size_t, nmemb, size_t size);
void *malloc(size_t size);
void free(void *ptr);
```
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::to-finish