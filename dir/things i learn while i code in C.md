## programmazione socket
[[dir/dir sistemi/md/Programmazione socket]]

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

- **run**: una volta compilato tutto, si può eseguire il codice da terminale facendo `./main`

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
 **1. allocazione statica**
- quando: a _compile time_
- dove: nella _data segment_ (o BSS se inizializzata a zero)
- come: la memoria esiste per tutta la durata del programma
- **esempio**:
 ```c
int x = 5;         // variabile globale o locale 'static'
static int y = 10; // persiste per tutta l'esecuzione
````
- uso: variabili globali, costanti, buffer permanenti

**2. allocazione automatica (stack)**
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

**3. allocazione dinamica (heap)**
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

## macro
- a **macro** is a name that gets replaced by some code or value before the compiler starts compiling, during the _preprocessing_ stage
- they’re created using the `#define` directive

types of macros
1. **Object-like macros**
	- behave like constants:
	- `#define PI 3.14159`
	- every time `PI` appears in the code, the preprocessor replaces it with `3.14159`

2. **Function-like macros**
    - work like inline functions, but are replaced textually
    - `#define SQUARE(x) ((x) * (x))`
    - `SQUARE(5)` becomes `((5) * (5))` before compilation

**key characteristics**:	
- **no type checking** — the preprocessor just does a text replacement
- **faster execution** (no function call overhead), but can cause bugs if not used carefully
- **global scope** — once defined, it’s visible until undefined with `#undef`

**advantages**:
- can make code more readable (e.g., `PORT` instead of `8080` everywhere)
- easy to change a value in one place and have it updated everywhere
**disadvantages**:
- no debugging info — once replaced, the compiler doesn’t know the macro name existed
- can lead to unintended side effects due to simple text substitution:
	- `#define SQUARE(x) x*x`, `int y = SQUARE(1+2);1` becomes `1+2*1+2 → 5, not 9`

**modern alternative**
- in modern C/C++, for constants it’s safer to use:
- `const int PORT = 8080;` 
- `constexpr double PI = 3.14;`
- `inline int square(int x) { return x * x; }` for functions

**some macro**:
```c
#define EXIT_SUCCESS 0
#define EXIT_FAILURE 1
```
- defined in `<stdlib.h>`
- used because of:
	- **portability**: while in practice `0` = success and `nonzero` = failure, the exact values may differ across systems
	- **readability**: `return EXIT_FAILURE; // clearer than: return 1;`

##### bonus arduino
quando viene compilato uno sketch Arduino, il sistema genera un vero `main()` in C++, che è qualcosa del genere:

```c
#include <Arduino.h>
int main(void) {
    init();        // inizializza hardware di base
    setup();       // la tua funzione setup() definita nello sketch
    for (;;) {     // corrisponde a un while true
        loop();    // la tua funzione loop() definita nello sketch
        if (serialEventRun) serialEventRun();
    }
    return 0;
```


- il `main()` viene chiamato automaticamente dal framework Arduino quando il microcontrollore si avvia
- `loop()` viene chiamata all’infinito finché il microcontrollore è acceso


## pezzi di codice

#### `strncmp()`
```c
if (strncmp(ack_buf, "ACK", 3) != 0)
```
cosa fa?
- controlla se le prime 3 lettere di `ack_buf` sono diverse da `"ACK"`
- `strncmp(s1, s2, n)` confronta al massimo i primi `n` caratteri delle due stringhe:
    - restituisce `0` se i primi `n` caratteri sono uguali
    - un valore diverso da `0` se c’è differenza

#### diff between `scanf()` and `getline()`
```c
scanf("%s", key)
```
- reads a word (sequence of non-whitespace characters)
- stops at space, tab, or newline
- example:
	- input → `my secret key`, `scanf("%s", key);` → only stores `"my"`
- if you don’t limit the size → _buffer overflow possible_
	- better:
```c
char key[256]; scanf("%255s", key);   // max 255 chars, then '\0'
``` 
- no dynamic allocation: memory lives on the stack
- `%255s` means: read at most 255 characters, then stop and add `'\0'`


```c
getline(&key, &len, stdin);
```
- reads a whole line, including spaces, until enter (`\n`)
- can handle arbitrary length (since it reallocates buffer)

so:

- if your key is a single word with no spaces:
	- `scanf("%s", key)` is enough (with width specifier to avoid overflow)
	- simple, fast, fine for short, single-word inputs

- if your key may contain spaces or could be very long:
	- prefer `getline`
	- safer, flexible, works with long or multi-word input, dynamic buffer

#### diff between`u_int64` and `unsigned long long int`
- `unsigned long long int`
	- standard C type (since C99) that guarantees at least 64 bits (usually exactly 64 bits on modern systems)
- `u_int64` 
    - this is not standard C, but a BSD / system-specific typedef (from `<sys/types.h>`)
    - it is typically defined as: `typedef unsigned long long u_int64;`

#### `strtoull()`
```c
unsigned long long strtoull(const char *nptr, char **endptr, int base);
```
- str to ull (unsigned long long)
- converts the string `nptr` into an `unsigned long long`
    - `base` = 2…36 or 0 (where 0 auto-detects `0x` as hex, `0` prefix as octal, else decimal)
    - `endptr` gets set to the first invalid character (useful for parsing mixed strings)

#### `htobe64()`
```c
#include <endian.h>
uint64_t x = 0x1122334455667788ULL;
uint64_t y = htobe64(x);
// On little-endian machine: y bytes reversed
// On big-endian machine: y == x

```

- macro/function to convert a 64-bit integer from _host byte order_ to  _big-endian_ byte order
- defined in `<endian.h>` (Linux, BSD)
why:
- different CPUs store multibyte integers differently:
    - **Little-endian** (x86, ARM in most cases) → least significant byte first
    - **Big-endian** (some network protocols, older architectures) → most significant byte first
- network protocols usually require **big-endian** ("network byte order")
#### term elucidation
**BSD**:
- Berkeley Software Distribution: variante storica di Unix sviluppata all’Università di Berkeley (anni ’70–’80)
- mportante perché molte librerie, convenzioni e funzioni di sistema derivate da BSD sono poi confluite in Linux e altri Unix-like
- esmpio: `u_int64`, `u_int32`

**dereferenziato**:
- dereferenziare un puntatore significa accedere al contenuno della memoria a cui il puntatore punta (ciò che si fa con l'operatore `*`)

#### osservazione
```c
uint64_t key_be = htobe64(*(uint64_t*)K);
```
- in questo contesto `K` viene interpretato come un puntatore a `uint64_t` e poi dereferenziato

`(uint64_t*)K`
- reinterpreta l'indirizzo di `K` (che in questo caso è un `char*`) come se fosse un `uint64_y`
- sostanzialmente dice al compilatore: "tratta questi 8 byte consecutivi come se fossero un unico intero senza segno da 64 bit"
	- `char*` → puntatore a byte singoli
	- `uint64_t*` → puntatore a blocchi da 8 byte

`*(uint64_t*)K`
- il `*` fa il **dereference**
	- legge effettivamente quegli 8 byte e li interpreta come `uint64_t`

`htobe64(...)` ("host to big endian 64")
- prende quel numero e lo converte in big endian
	- se il tuo host è già big endian → non fa niente
	- se il tuo host è little endian → ribalta l’ordine dei byte per portarlo in big endian
- serve poiché quando si mandano numeri via socket, c’è bisogno di un formato standard comune
- per convenzione, tutti i protocolli di rete usano big endian

```c
uint64_t key_be = htobe64(strtoull(K, NULL, 10));
```
`strtoull(K, NULL, 10)`
- `K` è una stringa che contiene un numero in ASCII
- la funzione interpreta quella stringa come numero decimale in base 10 e lo converte in `uint64_t`

`htobe64(...)` come sopra