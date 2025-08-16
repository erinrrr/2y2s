## blocco condizionale
il codice che segue è un **blocco di compilazione condizionale** in C/C++ per gestire differenze tra Windows e sistemi Unix-like nella programmazione di rete

```
#ifdef _WIN32
#include <winsock2.h>
#pragma comment(lib, "ws2_32.lib")
#else
#include <arpa/inet.h>
#endif
```

- `#ifdef _WIN32`:
	- controlla se è definita la macro `_WIN32`
	- definita automaticamente dal compilatore quando si compila su Windows a 32 o 64 bit
	- se è vera, significa che il codice sta compilando su Windows

- `#include <winsock2.h>`
	- include l’header principale per Winsock, l’API di rete di Windows
	- fornisce funzioni per socket, connessioni TCP/UDP, conversione di indirizzi IP, ecc

- `#pragma comment(lib, "ws2_32.lib")`
	- istruzione specifica per il compilatore Microsoft Visual C++
	- indica di collegare automaticamente la libreria `ws2_32.lib` (necessaria per usare Winsock)
	- senza questo, dovresti linkare manualmente la libreria durante la compilazione

- `#else`
	- se non stiamo compilando su Windows, eseguiamo il blocco seguente (Linux, macOS, BSD...)

- `#include <arpa/inet.h>`
	- include funzioni di rete POSIX (come `inet_addr`, `inet_ntoa`, `htonl`, `htons`, ecc.) disponibili su Unix-like
	- su Linux non serve una libreria extra come in Windows, perché le funzioni di rete fanno parte della libreria standard C (`libc`)

in sintesi:
- serve a scrivere codice portabile per socket, scegliendo automaticamente l’header e le librerie corrette in base al sistema operativo

### definizioni
- **file descriptor**:
	- intero che rappresenta un socket aperto
	- usato per leggere, scrivere o gestire la comunicazione attraverso quel socket, proprio come si fa con i file
- un file descriptor **full duplex** significa che:
	- lo stesso descrittore può essere usato sia per leggere che per scrivere dati

- **SSD** (Server Socket Descriptor):
	- il file descriptor del socket del server che ascolta (`listen_fd`)
	- serve solo per accettare connessioni tramite `accept()`
    
- **CSD** (Client Socket Descriptor):
	- il file descriptor della connessione specifica con un client (`fd_client` restituito da `accept()`)
	- serve per leggere/scrivere dati col client
- **NBO** (Network Byte Order): most significant byte first
- **HBO** (Host Byte Order) (i386): last significant byte first

> [!example] esempio `0x12345678`
> - NBO → `[12] [34] [56] [78]`
> - HBO → `[78] [56] [34] [12]`

le socket consentono la comunicazione tra processi nel paradigma client-server

## system call di interesse
##### 1. socket()
```c
int sd = socket(int domain, int type, int protocol);
int sd = socket(AF_UNIX, SOCK_STREAM, 0);
```
- crea un endpoint di comunicazione e restituisce un file descriptor che rappresenta il socket

_parametri principali_:
- `domain`: famiglia di protocolli (es. `AF_INET` per IPv4, `AF_INET6` per IPv6) 
- `type`: tipo di socket (es. `SOCK_STREAM` per TCP, `SOCK_DGRAM` per UDP)
- `protocol`: protocollo specifico (0 per default)

_valore restituito_(`fd`):
- $≥ 0$ → descrittore del socket (SSD o CSD)
- $-1$ → errore

##### 2. bind()
```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```
- associa un socket a un indirizzo locale (IP + porta)
- per il server, rende il socket identificabile dai client

_parametri principali_:
- `sockfd`: descrittore del socket (ritornato da `socket()`) (id unnamed socket)
- `addr`: puntatore a una struttura di tipo `sockaddr` che contiene IP/nome della socket
- `addrlen`: dimensione della struttura `sockaddr` contenuta in `addr`

_valore restituito_:
- $0$ → successo
- $-1$ → errore

solo `AF_INET` socket possono fare binding su `IP_address:porta`

[[#struttura codice client|esempio di `struct sockaddr`]]

##### 3. listen()
```c
int listen(int sockfd, int backlog);
```
- mette il socket `sockfd` in modalità **passiva**, ovvero pronto ad accettare connessioni in arrivo (solo per socket TCP)

_parametri principali_:
- `sockfd`: socket creato con `socket()` e già bindato
- `backlog`: numero massimo di connessioni in attesa (coda)

_valore restituito_:
- $0$ → successo
- $-1$ → errore

##### 4. accept()
```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```
- usata per i socket con connessione (non per `SOCKET_DGRAM`)
- estrae la prima richiesta di connessione nella coda delle richieste in attesa sulla coda di listening del socket `sockfd`
- crea un nuovo socket con connessione e ritorna un nuovo file descriptor
	- il nuovo socket non è in ascolto, ma dedicato alla comunicazione col client
- il socket `sockfd` continua il suo lavoro

_parametri principali_:
- `sockfd`: descrittore del socket in ascolto (SSD)
- `addr`: (opzionale) struttura dove salvare l’indirizzo del client
- `addrlen`: puntatore alla variabile con la dimensione della struttura `addr`

_valore restituito_:
- $≥ 0$ → nuovo file descriptor per il client (CSD)
- $-1$ → errore

## socket
```c
#include <sys/types.h>
#include <sys/socket.h>
int sock = socket(int domain, int type, int protocol);
```
`#include <sys/types.h>`:
- serve a importare la definizione di diversi tipi di dato usati nelle funzioni di sistema (system calls) di Unix/Linux
`#include <sys/socket.h>`:
- include tutte le definizioni e le dichiarazioni necessarie per lavorare con i socket in C

tipologia socket definita da 3 attributi (as said):
1. _domain_ (or family) - modalità di collegamento
	- `AF_LOCAL` (or `AF_UNIX`): client e server devono risedere sulla stessa macchina
	- `AF_INET`: client e server comunicano in una rete con protocollo `IPV4`
	- `AF_INET6`: client e server comunicano in una rete con protocollo `IPV6`
2. _type_ - semantica del collegamento
	- `SOCK_STREAM`: flusso bidirezionale di byte, affidabile, connection oriented (socket TCP)
	- `SOCK_DGRAM`: supporta la comunicazione datagram (invio di pacchetti indipendenti e connection-less), inaffidabile (socket UDP)
	- `SOCK_RAW`: permette di interagire direttamente con il livello di rete, bypassando gran parte del protocollo TCP/IP standard
3. _protocol_ - protocollo usato
	- se il protocollo è **quello predefinito per il tipo di socket**, puoi mettere 0
		- per `SOCK_STREAM` imposta automaticamente TCP
		- per `SOCK_DGRAM` imposta automaticamente UDP
	- tipo socket: `SOCK_STREAM` → protocollo: `IPPROTO_TCP`
	- tipo socket: `SOCK_DGRAM` → protocollo: `IPPROTO_UDP`
	- tipo socket: `SOCK_RAW` → protocollo: quello che vuoi leggere/scrivere, es. `IPPROTO_ICMP`, `IPPROTO_TCP`, `IPPROTO_UDP`

la scrittura/lettura su un socket chiuso genera un errore **`SIGPIPE`**

### anatomia server TCP
1. creare il socket → `socket()`
	- il server chiama `socket()` per creare una struttura dati che rappresenta il "punto di comunicazione"
	- all’inizio è un **unnamed socket** → esiste, ma non ha ancora un indirizzo IP o una porta associata
2. assegnare un indirizzo → `bind()`
	- con `bind()` il server associa al socket un nome: tipicamente **IP + porta**
	- da unnamed socket si passa a **named socket** (ora sappiamo “dove” contattarlo in rete)
3. mettere in ascolto → `listen()`
	- il server dichiara che è pronto a ricevere connessioni e specifica la **lunghezza della coda**: 
		- il numero massimo di richieste in attesa che può gestire contemporaneamente (connessioni "pending")
4. accettare connessioni → `accept()`
	- `accept()` **blocca** il programma fino a che un client non si connette
	- quando arriva una connessione, `accept()` restituisce un file descriptor (fd) dedicato alla comunicazione con quel client
5. gestire la comunicazione
	- il server può creare un **processo figlio** (o un thread) per gestire quel client usando l’`fd` restituito
	- la comunicazione avviene con `read()` e `write()` (o funzioni equivalenti)
	- intanto il processo principale resta libero di accettare altre connessioni
6. tornare in ascolto
	- dopo aver gestito una connessione, il server torna a `accept()` per aspettare un nuovo client

### anatomia client TCP
1. creare il socket → `socket()`
	- il client chiama `socket()` per creare un **unnamed socket** (come il server, all’inizio non ha ancora un indirizzo associato)
	- qui il file descriptor creato viene chiamato ad esempio `csd` (Client Socket Descriptor)
2. _preparare l’indirizzo del server_
	- il client compila una struttura dati (`struct sockaddr_in`) con:
	    - indirizzo IP del server
	    - porta su cui il server è in ascolto
	    - tipo di protocollo (IPv4, TCP)
	- questa struttura serve per dire “dove” vogliamo connetterci
3. connettersi al server → `connect()`
	- il client invoca `connect(sockfd, sockaddr *addr, socklen_t addrlen);` passando:
		- `sockfd`: socket file descriptor returned by the `socket()` function
	    - l’indirizzo del server preparato prima
	    - la lunghezza dell'indirizzo
	- se la connessione ha successo, il socket ora è collegato al server e possiamo usarlo per comunicare
	- se fallisce, `connect()` restituisce `-1` (errore)
4. comunicazione (lettura/scrittura)
	- ora il client può:
	    - **scrivere** dati al server → `write(sockfd, ...)`
	    - **leggere** la risposta → `read(sockfd, ...)` 
5. chiudere la connessione → `close()`
	- quando il client ha finito di comunicare, chiude la connessione con `close(sockfd)`

![[dir/dir sistemi/asset/file 29.png]]![[dir/dir sistemi/asset/file 30.png|400]]
### struttura codice server
```c
int main() {
	int sd = socket(AF_INET, SOCKET_STREAM, 0);
	bind(sd, ...);
	listen(sd, MAX_QUEUED); // MAX_QUEUED è una macro
	// disabilito il segnale SIGCHLD per evitare di creare zombie process
	//signal(SIGCHLD, SIG_IGN);
	while (1) {
		int client_sd=accpet(sd, ...);
		if (client_sd==-1) {
			perror("Errore accettando connessione dal client");
			continue;
		}
		if (fork()==0) { // eseguito dal client
			// read/write (o send) su client_sd
			close(client_sd);
			exit(0);
		}
	}
}
```

`signal(SIGCHLD, SIG_IGN);` dice al sistema: _"Ignora SIGCHLD"_:
- quando un processo figlio termina, invia un segnale **`SIGCHLD`** al padre
- se il padre non lo intercetta e non fa `wait()`, il figlio morto resta in stato zombie (occupando una entry nella tabella dei processi)

`fork() == 0`:
- `fork()` crea un nuovo processo duplicando quello attuale 
- viene chiamato _processo padre_ quello che continua a eseguire dopo la `fork()` con il _PID del figlio_ come valore di ritorno
- nel processo figlio, `fork()` restituisce 0
- quindi funziona che:
	- il **padre** rimane in `while(1)` per continuare ad accettare nuove connessioni (`accept()`)
	- il **figlio** gestisce un solo client (`read/write` su `client_sd`) e poi termina


### struttura codice client
```c
int main (){
	int cfd;
	int cfd = socket(AF_INET, SOCK_STREAM,0);
	// set sockaddr_in structure
	if (connect(cfd,….)!=0) {
		perror(“connessione non riuscita”);
	}
	// read(cfd,…) e write(cfd,…) da/verso server
	close(cfd);	
}
```

`struct sockaddr_in server_addr;` contiene":
- `sin_family`: famiglia di indirizzi (per IPv4 è `AF_INET`)|
- `sin_port`: porta del server (in network byte order, quindi va convertita con `htons(numeroPorta)`)
- `sin_addr.s_addr`: indirizzo IP del server (in network byte order, tipicamente ottenuto con `inet_addr("IP")` o `inet_pton()`)
- `sin_zero`: padding (riempito con zeri, non usato)

esempio:
- forma dei dati in memoria:
```c
struct sockaddr_in {
	sa_family_t sin_family; /* address family: AF_INET */
	in_port_t sin_port; /* port in network byte order */
	struct in_addr sin_addr; /* internet address */
};
/* Internet address. */
struct in_addr {
	uint32_t s_addr; /* address in network byte order */
};
```

- codice eseguibile:
```c
struct sockaddr_in server_addr;

server_addr.sin_family = AF_INET;     // IPv4
server_addr.sin_port = htons(12345);  // Porta 12345 in formato di rete
server_addr.sin_addr.s_addr = inet_addr("192.168.1.10"); // IP del server
memset(server_addr.sin_zero, 0, sizeof(server_addr.sin_zero));


```
poi nella `connect()` viene passato l’indirizzo di questa struttura:
`connect(cfd, (struct sockaddr*)&server_addr, sizeof(server_addr));`

è possibile assegnare un valore a`sin_addr` assegnandogli una delle seguenti macro:
- `INADDR_LOOPBACK` (`127.0.0.1`)
- `INADDR_ANY` (`0.0.0.0`)
sono quindi necessarie delle funzioni per convertire gli indirizzi in NBO: 
 - `uint_t htonl(uint32_t hostlong)`: converte un `unsigned int` in formato NBO
 - `int inet_aton(const char *cp, struct in_addr *inp)`: converte un indirizzo `cp="X.Y.Z.W"` in NBO
 - `struct hostent *gethostbyname (const char *name)`: dato un nome logico, `mio.dominio.toplevel` o indirizzo `X.Y.Z.W` ritorna una struttura `hostent` che contiene l’indirizzo in formato NBO

#### connect()
```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen)
```
- invocata dal client per associare un indirizzo `addr` ad un unnamed socket `sockfd`

_parametri principali_:
- `addrlen`: dimensione di `addr` ovvero `sockaddr`

_valore restituito_:
- $0$ → è andata a buon fine, quindi `sockfd` può essere utilizzato come file descriptor per leggere e scrivere sul server
- $-1$ → errore