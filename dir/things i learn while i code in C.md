## header file
 `.h`:
 - contiene dichiarazioni:
	- prototipi di funzioni (`void mia_funzione(int a);`)
	- definizioni di costanti (`#define MAX 100`)
	- tipi (`struct`, `typedef`, `enum`)
- in genere non contiene la logica delle funzioni
- serve per dire al compilatore “questa funzione esiste e ha questa firma”

`.c`:
- contiene l'implementazione vera e propria delle funzioni dichiarate nel `.h`
- può includere il proprio `.h` per assicurarsi che le firme corrispondano

_uso in un programma_:
- il `main.c` include il `.h` per sapere cosa può chiamare
- durante la compilazione vanno compilati tutti i `.c` che contengono le implementazioni, il linker (ld) le unirà
- esempio: se in `main.c` abbiamo `#include "client.h"` allora durante la compilazione si farà `gcc main.c client.c -o main`
- _trucco_: ogni `.c` include sempre il proprio `.h` all’inizio così:
	- se cambi la firma di una funzione nel `.c` ma ti dimentichi di aggiornarla nel `.h`, il compilatore se ne accorge subito

## funzioni file
**apertura e chiusura:**
- `fopen(path, mode)` → apre un file (`"r"`, `"w"`, `"a"`, `"rb"`, `"wb"`, ecc.)
- `freopen(new_path, mode, file)` → riapre/associare un file già aperto a un altro file o modo
- `fclose(file)` → chiude il file e libera le risorse

_esempio usato nel codice_:
```c
FILE *file = fopen(file_name, "r");
if (!file) {
	perror("Failed to open file");
	return; // importante per evitare crash
}
```

- `FILE *f`
    - `FILE` è un tipo di dato definito dalla libreria standard C che rappresenta una struttura interna usata dal sistema per gestire un file aperto
    - `*f` significa che `f` è un puntatore a un oggetto FILE, non contiene il file stesso, ma un riferimento (handle) che la libreria userà per leggere/scrivere

- **`fopen("file_name", "rb")`**
     - `fopen` apre un file e restituisce un puntatore `FILE*`
    - `"file_name"` → è il nome (e percorso) del file da aprire
    - `"rb"` → è la modalità di apertura:
        - `"r"` → read (lettura), fallisce se il file non esiste
        - `"b"` → binary mode, che dice al sistema di non fare conversioni (in particolare su Windows, evita la traduzione `\n` ↔ `\r\n`).
- risultato
    - se il file esiste e si apre correttamente, `f` conterrà un puntatore valido a una struttura `FILE`
    - se fallisce (file inesistente o permessi mancanti), `fopen` restituisce `NULL`
    - Quindi dopo questa riga, si fa quasi sempre:
    - `if (f == NULL) { perror("Errore apertura file"); return 1; }`


_controllo bonus sulla lettura dei file_:
possiamo istanziare la variabile `read_bytes`:
- `size_t read_bytes = fread(F, 1, *L, file); if (read_bytes != *L) {     perror("Failed to read file");     ... }`

- `fread` ritorna quanti byte effettivamente letti dal file sono stati copiati in `F`
- può essere che legga meno byte di quelli richiesti (per esempio file troncato, errori di I/O, EOF prematuro)

quindi:
- assegno il valore di ritorno di `fread` a `read_bytes`
- controllo se è uguale alla lunghezza attesa `*L`
- se no, segnalo errore e gestisco la situazione

in sintesi: variabile per controllare l’effettivo numero di byte letti e garantire che la lettura sia andata a buon fine

**lettura e scrittura di blocchi in binario**:
- `fread(void *ptr, size_t size, size_t nmemb, FILE *stream)` → legge blocchi di dati binari
- `fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)` → scrive blocchi di dati

**funzioni per spostare e leggere la posizione corrente di un file**:
- `fseek(FILE *f, long offset, int origin)`: sposta il “cursore” di lettura/scrittura del file
	- `offset` = di quanti byte ti sposti
    - `origin` = da dove parti (`SEEK_SET` inizio, `SEEK_CUR` posizione corrente, `SEEK_END` fine)
- `ftell(FILE *f)`: restituisce la posizione corrente (in byte dal inizio), se sei alla fine, equivale alla dimensione del file in byte
- `rewind(FILE *f)`: riporta il cursore all’inizio del file (equivale a `fseek(file, 0, SEEK_SET)` ma azzera anche eventuali errori sullo stream)

_esempio di codice_:
```c
fseek(file, 0, SEEK_END); // vai alla fine 
long L = ftell(file);     // leggi dimensione 
rewind(file);             // torna all’inizio`
```
questo pattern serve per sapere **quanto è grande il file** e poi fa il `rewind` per poterlo leggere

possiamo anche:
`L = (int)file_size`, dato che `ftell` restituisce un `long` in quanto la dimensione del file può essere più grande di un int

## alcune librerie
- `#include <stdlib.h>` → funzioni standard di utilità (es. `malloc`, `free`, `exit`, conversioni `atoi`, `strtol`)
- `#include <stdint.h>` → tipi interi con dimensione fissata (`uint8_t`, `int32_t`, ecc.), utili per portabilità
- `#include <string.h>` → funzioni per manipolazione di stringhe e memoria (`strlen`, `strcpy`, `memcmp`, `memset`)
- `#include <unistd.h>` → Chiamate di sistema POSIX di basso livello (`read`, `write`, `close`, `fork`, `exec`, `sleep`)
- `#include <errno.h>` → definisce la variabile globale `errno` e costanti degli errori di sistema, usata per capire il motivo di un fallimento di una funzione di sistema

- `#include <arpa/inet.h>` → funzioni per conversione di indirizzi IP e numeri di porta tra formati _testuale ↔ binario_ e _host ↔ network_ 
- `#include <netinet/in.h>` → strutture e costanti per protocolli di rete Internet (IPv4/IPv6), come `sockaddr_in`, `sockaddr_in6`, `INADDR_ANY`
- `#include <sys/socket.h>` → definizioni per la creazione e gestione di socket (strutture `sockaddr`, costanti `AF_INET`, `SOCK_STREAM`, funzioni `socket`, `bind`, `connect`)
- `#include <sys/types.h>` → tipi di dati usati in chiamate di sistema (`size_t`, `ssize_t`, `off_t`, `pid_t`), spesso incluso insieme ad altri header POSIX

POSIX stands for **Portable Operating System Interface** standard che specifica un insieme di API, comandi e comportamenti per rendere i programmi portabili tra diversi os di tipo unix e altri che lo implementano


## mem
| funzione                       | scopo                                                     | note                                                           |
| ------------------------------ | --------------------------------------------------------- | -------------------------------------------------------------- |
| `memcpy(dest, src, n)`         | copia n byte da `src` a `dest`                            | più veloce di `memmove`, ma **non** sicura su aree sovrapposte |
| `memmove(dest, src, n)`        | copia n byte gestendo **aree sovrapposte**                | più sicura di `memcpy`, leggermente più lenta                  |
| `memset(ptr, value, n)`        | riempie con `value` (byte) i primi n byte di `ptr`        | spesso usata per azzerare (`memset(buf, 0, size)`)             |
| `memcmp(ptr1, ptr2, n)`        | confronta n byte tra 2 blocchi di memoria                 | restituisce `<0`, `0`, `>0` come `strcmp`                      |
| `memchr(ptr, value, n)`        | cerca il **primo byte** uguale a `value` nei primi n byte | restituisce puntatore o `NULL`                                 |
| `memrchr(ptr, value, n)` (GNU) | come `memchr` ma cerca **da destra verso sinistra**       | non standard POSIX                                             |

## allocazione di memoria
 **1. Allocazione statica**
- quando: a _compile time_
- dove: nella _data segment_ (o BSS se inizializzata a zero)
- come: la memoria esiste per tutta la durata del programma
- **esempio**:
 ```c
int x = 5;         // variabile globale o locale 'static'
static int y = 10; // persiste per tutta l'esecuzione
````
- uso: variabili globali, costanti, buffer permanenti

**2. Allocazione automatica (stack)**
- quando: a _runtime_ all’entrata della funzione
- dove: nello _stack_
- come: viene liberata automaticamente quando la funzione ritorna.
- **esempio**:
```c
void foo() {
    int a = 10;   // locale, su stack
    char buf[256]; // buffer temporaneo
} // le variabili a e buf scompaiono qui
```
 **uso**: variabili temporanee, più veloce dell’heap

**3. Allocazione dinamica (heap)**
- quando: a _runtime_, su richiesta esplicita
- dove: nell’_heap_
- come: il programmatore deve liberarla manualmente (`free`)
- **esempio**:
```c
int *p = malloc(10 * sizeof(int));
if (!p) { /* errore allocazione */ }
/* ... */
free(p);
```

**funzioni principali**:
- `malloc(size)` → alloca blocco non inizializzato
- `calloc(n, size)` → come `malloc` ma azzera i byte 
	- `n` = numero di elementi da allocare
	- esempio: `calloc(10, sizeof(int))` → alloca spazio per 10 interi e li azzera
- `realloc(ptr, new_size)` → ridimensiona blocco
	- - `ptr` → puntatore a un blocco di memoria _già allocato_ con `malloc`/`calloc`/`realloc`
	- se più grande → prova a estendere il blocco
	- se più piccolo → tronca i dati in eccesso
	- se `ptr == NULL` → si comporta come `malloc(new_size)`
	- se `new_size == 0` e `ptr != NULL` → libera il blocco come `free(ptr)`
- `free(ptr)` → libera blocco
	- dopo `free(ptr)`, il puntatore diventa _dangling_ (non valido)
	- spesso si imposta a `NULL` per sicurezza
	- se `ptr == NULL` → non fa nulla