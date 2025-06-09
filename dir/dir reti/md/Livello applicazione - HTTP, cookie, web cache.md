i protocolli dal layer applicazione sono spesso parte di un applicazione, la più famosa è il _World Wide Web_:
- applicazione nata dalla necessità di scambio e condivisione di informazioni tra ricercatori universitari di varie nazioni
- inizialmente basata su:
	- documenti di testo collegati fra loro tramite collegamenti ipertestuali
	- motori di ricerca che permettono di navigare sui vari siti
- si basa su un funzionamento **on demand**:
	- un client invia una richiesta per una pagina web quando ne ha bisogno
	- il server risponde inviando solamente ciò che è stato richiesto
The World Wide Web application composta da:
- Web client: i browser che sono l'interfaccia con l'utente
- Web server: dove sono memorizzate le applicazioni vere e proprie
- HTML: linguaggio per scrivere pagine web
- HTTP: protocollo per la comunicazione tra client e server Web

![[dir/dir reti/asset/file 32.png]]

## Componenti
una pagina web è costituita da:
- _oggetti_ che possono essere:
	- un file HTML, un’immagine JPEG, un’applet Java ecc…
- un file base HTML che include diversi oggetti referenziati
- ogni oggetto è referenziato da un URL
    - es. `www.someschool.edu/someDept/pic.gif`

**URL**:
- Uniform Resource Locator
- composto da 3 parti:
	1. il protocollo
	2. il nome della macchina (hostname) in cui è situata la pagina
	3. il percorso del file all'interno della macchina (quindi nel server)
- quindi:
	- `protocol://host/path` quando la porta è standard
	- `protocol://host:porta/path`: se viene utilizzata una porta diversa
	- la porta è un numero che identifica un servizio specifico all'interno di un computer/server
- originariamente vi era il problema di non riuscire a localizzare una pagina, poiché attraverso un identificatore univoco pur sapendo come si chiamasse una pagina non sapevamo di preciso dove andare a cercarla

![[dir/dir reti/asset/file 33.png]]

**documenti web**, ne esistono di diversi:
- documento statico:
	- contenuto predeterminato memorizzato sul server
	- quando il client lo richiede, il server lo invia senza modificarlo
- documento dinamico:
	- contenuto creato dal web server nel momento della ricezione della richiesta
- documento attivo:
	- contiene script o programmi che vengono eseguiti nel browser, quindi sulla macchina del client
	- serve per interagire col sito senza bisogno di ricaricarlo

## HTTP (RFC 2616)
- RFC 2616 è il documento ufficiale che lo definisce
- protocollo che lavora a livello di applicazione del web
- definisce come i client web richiedono pagine ai server web e come questi le trasferiscono ai client
- basato su scambi di messaggi, modello client-server (request-response)
- lato client:
	- il browser web implementa l'interfaccia verso l'utente e il protocollo applicativo
	1. il browser determina l'url