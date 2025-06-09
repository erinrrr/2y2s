## cose che palesemente non ricorderò
- il **throughput** ci indica quanto velocemente riusciamo a inviare i dati. non é un’indicazione basata sulla tecnologia utilizzata come il bitrate o larghezza di banda, è quanto riusciamo ad inviare _realmente_ in quel momento

- differenza trasmissione-propagazione:
	- la trasmissione è il tempo necessario a un nodo per far uscire il pacchetto quindi varia in base al rate del link e alla lunghezza del pacchetto.
	- la propagazione invece indica il tempo che impiega un bit per percorrere il percorso una volta fuori dal nodo, varia quindi in base alla distanza.

## La rete
La rete come detto prima è organizzata a **layer** costruiti l’uno sull’altro, lo scopo di ogni layer è quello di offrire servizi agli strati superiori nascondendo i dettagli implementativi.
Le entità che formano questi strati prendono il nome di **peer**, quindi ad esempio due layer di **collegamento** formano un peer.

- Stack protocollare TCP-IP, composto da 5 livelli:

	1. Applicazione - Livello 5 il più vicino all’utente
	2. Trasporto
	3. Rete
	4. Collegamento
	5. Fisico - Livello 1

- _Applicazione_ - È dove si trovano le applicazioni di rete come server HTTP, SMTP, FTP, DNS. Qui i pacchetti prendono il nome di **messaggi**.
- _Trasporto_ - Si occupa di trasferire i messaggi dell’applicazione tra il client e il server, troviamo ad esempio i due protocolli UDP e TCP. Il TCP è più sicuro ma meno veloce e i pacchetti prendono il nome di segmenti mentre UDP è meno sicuro ma più veloce, qui i pacchetti prendono il nome di **datagramma utente**.
- _Rete_ - Si occupa di trovare un percorso che metta in collegamento il nodo di origine e il nodo di arrivo, usiamo il protocollo IP e altri di instradamento. Qui i pacchetti prendono il nome di **datagrammi**.
- _Link (collegamento)_ - Molto simile alla rete ma non si occupa del percorso origine - destinazione ma del come far comunicare due nodi all’interno del percorso quindi ad esempio se usare una connessione ethernet o un Wi-Fi. I pacchetti prendono il nome di **frame**.
- _Fisico_ - È il trasferimento vero e proprio dei bit

 I vari dispositivi che necessitano di comunicare implementano tutto lo stack, i router invece non hanno bisogno dei livelli più alti dato che devono solo gestire la parte meno vicina all’utente, così facendo riduciamo la complessità della rete


> [!NOTE]- servizio - protocollo
> **Servizio** - insieme di primitive che uno strato offre a quello superiore, definisce quindi quali operazioni si possono usare ma non il come ovvero l’implementazione.
> **Protocollo** - È un insieme di regole che controllano il formato e significato dei pacchetti o in generale i messaggi che vengono scambiati all’interno dello strato.

- Quando un layer riceve un pacchetto dal livello superiore questo lo incapsula aggiungendo un header in modo da renderlo leggibile al livello con cui è in peer.
- Il router è quel dispositivo che dovrà effettuare sia incapsulamento che decapsulamento dato che è collegato a due link.
- Aggiungendo questi header permettiamo quindi ad ogni livello di riconoscere su quali pacchetti devono agire, se ad esempio il livello di trasporto aggiunge come header `4`, gli altri livelli di trasporto sanno che dovranno leggere soltanto i pacchetti con header `4`.
	- I nomi che diamo ai pacchetti nei vari livelli dipendono appunto dall’header presente:
		- _messaggio_ - nessuna intestazione
		- _segmento_ o datagramma utente - header trasporto + messaggio
		- _datagramma_ - header rete + segmento
		- _frame_ - header collegamento + datagramma

- **Multiplexing**: Un protocollo può incapsulare i pacchetti ottenuti da più protocolli di livello superiore
- **Demultiplexing**: Un protocollo può decapsulare e consegnare i pacchetti a più protocolli del livello superiore

	- TCP può incapsulare pacchetti di tipo FTP, HTTP 
	- UDP può incapsulare DNS e SNMP

# Livello applicazione
### Intro TCP-IP
Il livello applicazione serve ad offrire dei servizi agli utenti finali

Indirizzamento TCP-IP:
- Dato che utilizziamo una comunicazione logica tra coppie di livelli abbiamo bisogno di due indirizzi, uno per la sorgente e uno per la destinazione:
![[dir/dir reti/asset/file 37.png]]

tra i protocolli possiamo individuare:
- _Protocolli Standard_ - Sono diversi protocolli standardizzati e documentati dagli enti responsabili della gestione Internet. Ognuno di questi protocolli è costituito da una coppia di programmi che interagiscono con l’utente e con il livello di trasporto per offrire un servizio. Troviamo ad esempio applicazioni web con protocollo HTTP.
- _Protocolli non standard_ - È possibile scrivere applicazioni non standard scrivendo due programmi che forniscono servizi agli utenti facendo uso dei servizi di trasporto. in questo caso non serve chiedere autorizzazioni dato che ogni azienda può sviluppare il proprio software che utilizza il proprio protocollo di comunicazione.

**Paradigma client-server:**
- Client - È il componente che richiede il servizio, può essercene più di uno e vanno in esecuzione soltanto quando richiesto.
- Server - È quello che offre i servizi, è quindi sempre in esecuzione in attesa di richieste.

### HTTP, cookie, web cache
Il WWW è formato da varie componenti:
- **Web Client** - ovvero i browser che utilizza l’utente
- **Web Server** - Dove sono memorizzate le applicazioni vere e proprie (Apache)
- **HTML**: Linguaggio per scrivere pagine web
- **HTTP**: Protocollo per la comunicazione tra client e server

la rete si basa su un funzionamento _on demand_ ovvero un client invia una richiesta per una pagina web e il server gliela invia

architettura generale di un browser:
![[dir/dir reti/asset/file 38.png]]

##### Elementi di una pagina web
costituita da _oggetti_ che possono essere:
- Un file HTML, un’immagine JPEG, un’applet Java ecc…
- Ogni pagina però di base è un file HTML che include diversi oggetti referenziati
- Gli oggetti sono referenziati tramite degli _URL_ (Uniform Resource Locator)
    - es. `www.someschool.edu/someDept/pic.gif`

Gli URL sono composti da:
- Il protocollo da usare per richiedere quella pagina
- Nome della macchina dove è situata la pagina
- Percorso del file all’interno di quella macchina
![[dir/dir reti/asset/file 39.png]]

Per _comunicare con una macchina_ questa “apre delle porte” ognuna per un tipo di protocollo o servizio, i protocolli hanno delle porte standard che vengono utilizzate se non le specifichiamo dopo il nome della macchina, quindi ad esempio:
- `protocol://host/path` userà la porta standard del protocollo
- `protocol://host:porta/path` utilizzerà la porta specificata

esistono _vari tipi di documenti web_:
- Statici - Contenuto predeterminato e già memorizzato sul server
- Dinamico - Il contenuto viene creato dal server nel momento della ricezione di una richiesta
- Attivo - Contiene degli script che vengono eseguiti sulla macchina del client

#### HTTP (RFC 2616)
HTTP si basa sul modello Client - Server, il protocollo definisce in che modo i client devono richiedere le pagine e come i server devono trasferirle ai client. 

Nello specifico:
- **Lato Client**
    - Il browser determina dall’URL host e nome del file
    - Esegue la connessione TCP alla porta 80 (standard)
    - Invia una richiesta per il file
    - Riceve il file
    - Chiude la connessione
    - Visualizza il file
- **Lato Server**
    - Accetta la connessione TCP dal client
    - Riceve il nome del file richiesto
    - Recupera il file dal disco
    - Invia il file al client
    - Rilascia la connessione

Non tutte le risorse che il client richiede si troveranno sullo stesso server, in modo trasparente all’utente il client richiederà le informazioni anche da altri server: ![[dir/dir reti/asset/file 40.png]]

esistono **2 tipi di connessioni HTTP**:
- Connessioni **non persistenti** - Per ogni oggetto che richiediamo va aperta una nuova connessione TCP e chiusa a fine trasferimento.
- Connessioni **persistenti** - È la modalità usata di default, tramite una singola connessione possono essere trasmessi più oggetti. La connessione non viene più chiusa a fine trasferimento ma quando rimane inattiva per molto tempo.


> [!info] RTT - Round Trip Time
> - tempo impiegato da un piccolo pacchetto per andare dal client al server e tornare indietro
> - include i ritardi di propagazione, accodamento e di elaborazione del pacchetto


Il tempo di risposta di una _connessione non persistente_ è dato da:
- Un RTT per inizializzare la connessione TCP
- Un RTT per la richiesta HTTP e i primi byte della risposta
- Tempo di trasmissione del file
- Quindi in totale: $2RTT+tempo trasmissione$
![[dir/dir reti/asset/file 41.png|400]]

Gli svantaggi delle connessioni non persistenti sono:
- Richiedono 2RTT per oggetto
- Overhead nel sistema dato che per ogni oggetto va creata una connessione
- I browser spesso aprono connessioni parallele per caricare oggetti referenziati

Con le connessioni persistenti andiamo a risolvere alcuni dei problemi delle non persistenti, infatti:

- Il server lascia aperta la connessione dopo l’invio di una risposta
- In questo modo i successivi invii tra client-server usano la stessa connessione
- Non abbiamo quindi bisogno di un RTT per ogni oggetto ma di uno soltanto per la connessione e uno per ogni oggetto ricevuto.
---

**Formato messaggi in una richiesta HTTP**:
![[dir/dir reti/asset/file 42.png]]

```HTTP
GET /somedir/page.html HTTP/1.1
Host: www.someschool.edu
Connection: close
User-agent: Mozilla/4.0
Accept-language:fr
```

1. La prima riga è **riga di richiesta** dove vengono inseriti i comandi:
2. GET - POST - HEAD
3. Le successive sono **righe di intestazione**
4. A fine messaggio troviamo **carriage return** e un **life feed** (flusso continuo di dati)

![[file 43.png]]


##### Tipi di Metodi - HTTP/1.1
- **GET**:
	- Viene usato quando il client vuole scaricare un documento dal server
	- Il documento viene specificato all’interno dell’URL
- **HEAD**:
	- Utilizzato quando il client non vuole scaricare l’intero documento ma solo alcune informazioni di esso
	- Il server quindi in risposta non invia l’intero documento ma soltanto gli header richiesti
- **POST**:
	- Utilizzato per fornire input al server, questi vengono inseriti dall’utente in dei campi di un form
	- L’input arriva nel corpo del messaggio
	- Come detto prima è possibile utilizzare anche il GET inserendo i dati nell’URL[^1] 
- **PUT**:
	- È utilizzato per memorizzare un documento nel server
	- Il documento viene fornito nel corpo del messaggio e la posizione di memorizzazione nell’URL

[^1]: Alternativamente al metodo POST, possiamo usare il GET inserendo i dati del form nell’URL richiesto: `www.sito.com/animalsearch?moneys&banana`


Alcune intestazioni che possiamo trovare:
![[file 44.png]]

Formato dei messaggi di risposta:
![[file 45.png]]

In questo caso i nomi diventano:

1. La prima riga prende il nome di **riga di stato**
2. Troviamo sempre le **righe di intestazione**
3. Possiamo trovare altri dati come ad esempio gli oggetti richiesti```python

```HTTP
HTTP/1.1 200 OK
Connection close
Date: Thu, 06 Aug 1998 12:00:15 GMT
Server: Apache/1.3.0 (Unix)
Last-Modified: Mon, 22 Jun 1998 ...
Content-Lenght: 6821
Content-Type: text/html
---
(altri dati)
```

![[file 46.png]]


###### Codici di stato nella risposta
Nella riga di stato troviamo i **codici di stato** che ci indicano lo stato della risposta ricevuta, alcuni esempi:
![[file 47.png]]

intestazioni nella risposta:
![[file 48.png]]

##### Esempi
GET:
![[file 49.png]]
il client preleva un documento: viene usato il metodo GET per ottenere l’immagine individuata dal percorso. `/usr/bin/image1`
la riga di richiesta contiene:
- il metodo GET
- l’URL 
- la versione 1.1 del protocollo HTTP
- L’intestazione specifica che il client accetta immagini nei formati GIF e JPEG. Il messaggio non ha un body

La risposta contiene la riga di stato e quattro righe di intestazione:
- Data
- Server
- Metodo di codifica
- Lunghezza del documento
- Il corpo del messaggio segue l’intestazione
--- 

PUT:
![[file 50.png]]
Il client spedisce al server una pagina web da pubblicare utilizzando il metodo PUT

La riga di richiesta contiene:
- il metodo PUT
- l’URL
- la versione 1.1 di HTTP
- l’intestazione è composta da 4 righe
- il corpo contiene la pagina web inviata

La risposta contiene:
- la riga di stato
- 4 righe di intestazione
- il documento è incluso nel body della risposta

### Cookie
HTTP è un protocollo **stateless**:il server dopo aver servito il client se ne dimentica, quindi non mantiene informazioni su di esso

I protocolli che mantengono lo stato sono più complessi, perché il server deve ricordare tutto quello che fa il client. Se succede un errore, il client e il server potrebbero non essere più d'accordo su cosa è successo, creando confusione

In molti casi è necessario che il server si ricordi degli utenti ad esempio per:

- Offrire contenuti personalizzati in base alle preferenze di ogni utente
- Mantenere il carrello degli utenti nei siti di e-commerce

Per fare questo però gli indirizzi IP non sono adatti dato che gli utenti possono lavorare su dispositivi diversi e molti ISP assegnano lo stesso IP ai pacchetti in uscita provenienti da tutti gli utenti (tramite il NAT)

La soluzione è utilizzare i **Cookie (RFC 6265)**

---

Tramite i cookie possiamo creare una sessione di richieste e risposte HTTP con stato. La **sessione** rappresenta un concetto più esteso rispetto alle semplici richieste e risposte.

Ci possono essere diversi tipi di sessione in base al tipo di informazioni scambiate e la natura del sito, in generale però troviamo delle caratteristiche comuni:

- Inizio e fine
- Tempo di vita relativamente corto
- Client e server possono terminare la sessione
- La sessione è implicita nello scambio di informazioni di stato

Una sessione quindi non va vista come una connessione persistente ma come una connessione logica creata da richieste e risposte, possiamo quindi crearne una sia su connessioni persistenti che non

###### Interazione utente-server con i cookie
Abbiamo bisogno di 4 componenti:
- Una riga di intestazione nel messaggio di risposta HTTP
- Una riga di intestazione nel messaggio di richiesta HTTP
- Un file cookie mantenuto sul terminale del client e gestito dal browser
- Un database sul server
 ![[file 51.png]]
Il server mantiene tutte le informazioni riguardanti il client su un file e gli assegna un cookie ovvero un identificatore che verrà assegnato al client.

Questo identificatore, per evitare che vada in mano ad utenti maligni, è composto da una stringa complessa di numeri.

Ad ogni richiesta il browser consulta il file del cookie, estrae quello che necessario per quel sito web e lo inserisce in ogni richiesta HTTP, a questo punto il server può riconoscerlo e può mostrargli dei contenuti personalizzati.

Quindi a seconda del sito il browser sceglie il cookie con il giusto identificativo.

---

- Ma quanto dura un cookie?

Quando il server chiude una sessione invia al client un’intestazione `Set-Cookie` nel messaggio dove all’interno troviamo `Max-Age=delta-seconds` dove al posto di `delta-seconds` troviamo i secondi dopo i quali il client dovrebbe rimuovere il cookie
se troviamo `0` allora il cookie dovrebbe essere rimosso subito.

- Cosa possiamo fare con i cookie?
    - Memorizzare carte per acquisti
    - Preferenze dell’utente
    - Autorizzazioni di accesso
    - Mantengono lo stato del mittente e del ricevente per più transazioni
- Ovviamente va tenuta sotto controllo la privacy, il sito chiede sempre se l’utente vuole che vengano creati dei cookie per memorizzare queste informazioni oppure no.


Un’altra soluzione potrebbe essere quella di non usare i cookie e ad ogni richiesta inviare i dati che sarebbero stati nei cookie
- Questo è sicuramente più semplice da implementare ma:
- Può generare grandi scambi di dati
- Il server deve re-inizializzare le risorse ad ogni richiesta

### web caching
Vogliamo fare caching per migliorare le prestazioni dell’applicazione web. Per fare caching intendiamo caricare pagine in anticipo.

- Un modo potrebbe essere quello di salvare le pagine richieste per riutilizzarle in seguito
- Porterà sicuramente a dei vantaggi per quanto riguarda pagine caricate molto spesso
- Il caching può essere eseguito dal Browser o da dei server Proxy.

**Browser Caching**:
Con questo metodo è il browser a mantenere una cache delle pagine visitate, possiamo gestirla in diversi modi:

- L’utente può impostarla in modo tale che dopo alcuni giorni i contenuti vengano cancellati
- La pagina può essere mantenuta in base alla sua ultima modifica
- Si possono utilizzare informazioni nei campi intestazione dei messaggi per gestire la cache

**Server Proxy**:
Il server proxy è un intermediario fra i vari client e i server, il proxy ha una memoria che serve a mantenere a gestire le richieste e mantenere le pagine in cache
Se ad esempio 3 client richiedono una pagina, il proxy riceve le 3 richieste ma il server vero e proprio ne riceve soltanto una dal proxy
![[file 52.png|350]]
Il server proxy possiamo trovarlo anche all’interno di una LAN o anche nelle reti degli ISP per velocizzare i loro servizi

La cache serve quindi a:
- ridurre i tempi di risposta per i client
- ridurre il traffico sul collegamento ad Internet
- permettere ad ISP meno efficienti di fornire i dati in maniera veloce

**Internet delay**: per recuperare un file sulla internet ci vogliono 2 secondi
- Il tempo totale di risposta è quindi LAN Delay + Access delay + Internet delay

--- 

Il client invia un messaggio di richiesta HTTP alla cache:

``` HTTP
GET /page/figure.gif
Host: www.sito.com
```

La cache non ha l’oggetto e invia quindi una richiesta HTTP al server il quale risponde con l’oggetto richiesto. La cache memorizza la pagina per richieste future e spedisce la risposta al client.

Se invece la cache ha l’oggetto in memoria allora può inviarlo al client, ma prima deve **validarlo** per verificare che non sia stato modificato dal server. Per fare questo tipo di verifica viene utilizzato il **GET condizionale** che utilizza degli if.

Questo serve per fare in modo che se la cache ha una copia aggiornata allora l’oggetto non viene rinviato altrimenti si.

![[file 53.png]]

### Indirizzi IP

Sono composti da 32 bit e vengono usati per indirizzare i datagrammi, per noi umani sono più scomodi da usare ma sono più adatti alla macchine.

Possiamo rappresentarlo come una stringa dove ogni byte (4 byte = 32 bit) rappresenta un numero decimale compreso tra 0 e 255 ed ognuno di questi 4 numeri è separato da un punto (`.`), ad esempio: `121.34.230.94`

Questo formato serve anche a creare una gerarchia negli indirizzi infatti vedremo che tramite l’indirizzo IP possiamo ricavare la rete di appartenenza e l’indirizzo del nodo.

Ma quindi come facciamo da un indirizzo IP a ricavare un hostname facilmente utilizzabile da noi umani? Utilizziamo il **DNS*

### DNS - Domain Name System

