# cose che palesemente non ricorderò

IMPORTANTE: https://gaia.cs.umass.edu/kurose_ross/interactive/index.php

- il **throughput** ci indica quanto velocemente riusciamo a inviare i dati. non é un’indicazione basata sulla tecnologia utilizzata come il bitrate o larghezza di banda, è quanto riusciamo ad inviare _realmente_ in quel momento

- differenza trasmissione-propagazione:
	- la trasmissione è il tempo necessario a un nodo per far uscire il pacchetto quindi varia in base al rate del link e alla lunghezza del pacchetto.
	- la propagazione invece indica il tempo che impiega un bit per percorrere il percorso una volta fuori dal nodo, varia quindi in base alla distanza

- DNS
	- Organizziamo i server in 3 livelli:
		1. Root
		2. Top-level Domain (TLD)
		3. Authoritative
Infine troviamo i server locali delle applicazioni che sono quelli che fanno le richieste.

![[file 54.png]]

>Un **proxy riceve le richieste dal tuo computer, le inoltra a Internet**, riceve le risposte e te le rimanda indietro[^4]
>- se chiedi una pagina web, il tuo computer → **chiede al proxy** → il proxy va su Internet → prende la pagina → te la restituisce

server authoritative = server di competenza

Una **query** è una richiesta che un client (come un programma o un utente) invia a un server o a un database per ottenere dati o una risposta [^5]

- la combinazione di indirizzo IP + porta prende il nome di **socket address**

Un **socket TCP** è un **endpoint** per una connessione di rete che utilizza il protocollo TCP (Transmission Control Protocol), i socket permettono a due programmi (di solito su macchine diverse) di comunicare tra loro.

# La rete
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
## Intro TCP-IP
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

## HTTP, cookie, web cache
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

### HTTP (RFC 2616)
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

## Indirizzi IP

Sono composti da 32 bit e vengono usati per indirizzare i datagrammi, per noi umani sono più scomodi da usare ma sono più adatti alla macchine.

Possiamo rappresentarlo come una stringa dove ogni byte (4 byte = 32 bit) rappresenta un numero decimale compreso tra 0 e 255 ed ognuno di questi 4 numeri è separato da un punto (`.`), ad esempio: `121.34.230.94`

Questo formato serve anche a creare una gerarchia negli indirizzi infatti vedremo che tramite l’indirizzo IP possiamo ricavare la rete di appartenenza e l’indirizzo del nodo.

Ma quindi come facciamo da un indirizzo IP a ricavare un hostname facilmente utilizzabile da noi umani? Utilizziamo il **DNS*

## DNS - Domain Name System
Il protocollo DNS serve quindi ad associare ad un indirizzo IP un nome host, notiamo però che con l’indirizzo IP possiamo rappresentare 232 indirizzi in totale e tutte queste coppie devono essere accessibili

Per quanto riguarda la _memorizzazione_:
- Si utilizza un **database distribuito** su una gerarchia di server DNS 

Per quanto riguarda _l’accessibilità_:
- Si usa un **protocollo a livello applicazione** che consente agli host di interrogare i database dei server ed eseguire le traduzioni

Il DNS viene utilizzato anche dagli altri protocolli di livello applicazione per tradurre gli hostname e utilizza come protocollo per il trasporto l’UDP dato che ci serve più velocità che sicurezza per questo tipo di dati, di base utilizza la porta 53

Utilizziamo UDP perché come vedremo più avanti abbiamo meno overhead ovvero pacchetti aggiuntivi utilizzati non per trasmettere dati ma per gestire le connessioni. Infatti i messaggi DNS sono molto corti e una connessione TCP richiede più tempo, inoltre questa connessione verrebbe usata per scambiare un solo messaggio

_In generale (Esempio con HTTP)_:
1. Un client HTTP richiede un URL ad esempio `www.google.com`
2. Il browser prende il nome dell’host dall’URL e lo passa al lato client DNS
3. Il client DNS invia una query ad un server DNS
4. Il client DNS riceve una risposta che contiene l’indirizzo IP corrispondente all’hostname
5. Adesso il browser può dare inizio alla connessione TCP verso il server HTTP localizzato a quell’indirizzo IP

DNS:
- è un protocollo del livello applicazione
- viene eseguito dagli end systems secondo il paradigma client-server
- utilizza un protocollo di trasporto end-to-end per traferire messaggi tra gli end system (UDP)
- non è un’applicazione con cui gli utenti interagiscono direttamente (eccetto amministratori di rete)
- fornisce una funzionalità di base di internet per le applicazioni utente
- rispecchia la filosofia di concentrare la complessità nelle parti periferiche della rete
- offre anche altri tipi di servizi oltre a questa traduzione

##### Servizi DNS: Aliasing
questo permette di associare un nome (o più di uno) più semplice ad un nome più complesso, ad esempio `server1.euw.azienda.com` potrebbe avere dei sinonimi più semplici come `azienda.com`

quindi il primo, più “complesso” è l’**hostname canonico** mentre il secondo si chiama **alias**. Il funzionamento è uguale a quello per la traduzione tra indirizzo IP e nome host, quindi un client se chiede un alias riceverà nome canonico e indirizzo IP

di solito questo servizio viene utilizzato per i server mail delle aziende dove sia mail che web server hanno lo stesso alias ma nomi canonici diversi

##### Servizi DNS: Distribuzione del Carico
spesso le grandi aziende non hanno un unico server perché potrebbe rallentare notevolmente il servizio che offrono

anche in questo caso il DNS può aiutarci: l’azienda può creare più server uguali sparsi per il mondo tutti con indirizzi IP diversi e a tutti questi viene associato lo stesso hostname canonico

quando un server DNS viene interrogato questo restituisce tutti gli indirizzi ma in ordine diverso in modo da spartire il traffico su più server, inoltre se uno non è funzionante possiamo sempre collegarci ad un altro

##### Gerarchia Server DNS
Adesso DNS è un’applicazione che gira su un gran numero di server sparsi per il mondo, non può essere centralizzato per vari motivi:
- se crasha il server, Internet diventa praticamente inutilizzabile
- troppe richieste da gestire per un singolo server
- un solo server non potrà mai essere vicino ad ogni client del mondo e quindi rallenterebbe le persone più lontane
- il server va aggiornato spesso per aggiungere nuovi nomi e con uno singolo sarebbe molto difficile anche mantenerlo online
- un server centralizzato, non è scalabile


si utilizza una gerarchia di server, in questo modo:
- nessun server deve mantenere tutti le coppie hostname - indirizzo IP
- mapping (traduzione) distribuito su svariati server
- come memorizziamo le $2^{32}$ coppie in modo da renderle velocemente accessibili da tutti i client?

Raggruppiamo i nomi in base al dominio, ad esempio, possiamo quindi raggruppare in un server solo gli indirizzi `.it` in uno solo quelli `.com` e così via…

Organizziamo i server in 3 livelli: [^2]

1. Root
2. Top-level Domain (TLD)
3. Authoritative

Infine troviamo i server locali delle applicazioni che sono quelli che fanno le richieste 

![[file 54.png]]

[^2]: Quindi se ad esempio richiediamo la pagina `www.amazon.com`:
	1. La richiesta arriva al server root il quale gli dirà dove trovare il server `.com`
	2. Il server TLD gli dirà dove trovare il server `amazon.com`
	3. Il server di amazon gli dirà dove trovare `www.amazon.com`

###### Top Level Domain
Sono i server che si occupano dei domini `com, org, net, edu...`, ad esempio in Italia abbiamo il `Registro.it` che ha sede a Pisa nel CNR e gestisce il dominio `.it`
[^3]

[^3]: ![[file 55.png]]
###### Server di Competenza (authoritative server)
Ogni organizzazione avrà il suo authoritative server in modo da rendere pubblici i suoi hostname, questi sono gestiti direttamente dall’organizzazione e di solito ce ne sono due, uno primario e uno secondario

###### server DNS locale
Questo non appartiene direttamente alla gerarchia dei server dato che sono interni agli ISP o organizzazioni, questo è anche detto **default name server**. Quando un host effettua una richiesta DNS la query viene inviata al suo server DNS locale, questo agisce come un proxy e inoltra la query in una gerarchia di server.



[^4]: serve a:
		- **Nasconde l’indirizzo IP** del client
		- **Filtra o controlla** l’accesso a siti web (es. nelle reti aziendali o scolastiche) 
		- **Accelera la navigazione** (memorizzando copie delle pagine richieste: caching) 
		- **Anonimato e sicurezza** nella navigazione


#### Query
Ci sono diversi modi in cui possiamo gestire le query all’interno della gerarchia

- **Query Iterativa**:
	- con questo meccanismo:
	- l’host fa la richiesta per risolvere un nome del dominio
	- inoltra la richiesta al server DNS locale
	- il server DNS locale inizia la risoluzione chiedendo ai server DNS root
	- il server root risponde indicando dove trovare il server TLD,(`.com`)
	- il server TLD indica dove si trova il server authoritative per quel dominio specifico
	- il server authoritative fornisce l’indirizzo IP associato al nome richiesto 
	- il DNS locale restituisce l’indirizzo IP all’host, che può così raggiungere la pagina web
	- per fare la mappatura di un hostname sono stati inviati 8 messaggi
	- ![[file 56.png|350]]
- **Query ricorsiva**:
	- in questo modo alleggeriamo il server locale e lasciamo il compito della traduzione all’ultimo server contattato:
	- ![[file 57.png|350]]

##### DNS caching
- Dopo che un server DNS ha eseguito una mappatura (cioè ha risolto un nome di dominio), memorizza temporaneamente il risultato in cache
- Questo meccanismo riduce il numero di richieste future per lo stesso dominio
- I server DNS locali spesso mantengono in cache le informazioni sui server TLD (come `.com`, `.org`) e su quelli authoritative
- Di conseguenza, i server root vengono contattati raramente, solo quando non c’è nulla in cache e serve iniziare una nuova risoluzione completa

##### DNS: record e messaggi
Il mapping è mantenuto all’interno di server sottoforma di **resource record (RR)**, ognuno di questi mantiene un mapping tra hostname, indirizzo IP, alias, nome canonico e altro

- I server si scambiano questi record attraverso i messaggi DNS
- un messaggio può contenere più RR

**Record DNS**:
-  sono memorizzati all’interno del database
- vengono trasportati dai messaggi, hanno questa struttura:

- `(Name, Value, Type, TTL)` dove:
    - TTL è il tempo residuo di vita
    - Type ha vari valori e a seconda di questi fa cambiare il significato di `name` e `value`

Se Type vale `A`:
- allora il record contiene una mappatura: **Hostname → IP address**
- troveremo:
	- sul campo `name` l’hostname 
	- sul campo `value` l’indirizzo IP
- Es. (relay1.bar.foo.com, 45.37.93.126, A)

Se Type vale `CNAME`:
- mappatura: **Alias ➔ Canonical Name** 
- si tratta di una traduzione fra alias e nome canonico
- troveremo:
	- sul campo `name` l’alias 
	- sul campo `value` nome canonico
- Es. (foo.com, relay1.bar.foo.com, CNAME)

Se Type vale `NS`:
- mappatura: **Domain name ➔ Name Server**
-  dominio come ad esempio `uniroma1.it` e su `value` il nome dell’host del server authoritative di quel dominio come ad esempio `di.uniroma1.it`
- troveremo:
	- sul campo `name` il dominio (come ad esempio `uniroma1.it`)
	- sul campo `value` il nome dell’host del server authoritative di quel dominio (`di.uniroma1.it`) (server authoritative = server di competenza)
- Es. (foo.com, dns.foo.com, NS)

Se Type vale `MX`:
- mappatura: **Alias ➔ mail server canonical name**
- troveremo:
	- `value` è il nome canonico del server di posta associato a `name`
- Es. (foo.com, mail.bar.foo.com, MX)

In generale:

![](https://alem1105.github.io/Quartz/vault/Primo-Anno/Primo-Semestre/Immagini/Pasted-image-20250316110630.png)


[^5]: - **DNS:**  
	    Una query DNS è una richiesta fatta per conoscere l’indirizzo IP associato a un nome di dominio (es. "Qual è l'IP di www.google.com?")
	    
	- **Motori di ricerca (es. Google):**  
	    Anche la frase che scrivi nella barra di ricerca è una query: "migliori ristoranti a Roma"
	    
	- **API Web:**  
	    Un’app può inviare una query HTTP a un server REST per ottenere dati in formato JSON

##### Messaggi DNS
Le query e le risposte hanno lo stesso formato nel protocollo DNS

Abbiamo un’intestazione da 12 byte che comprende:

- Identificazione: 
	- numero di 16 bit (2 byte) per la domanda, la risposta usa lo stesso numero
- Flag che indicano:
    - Domanda o Risposta
    - richiesta di ricorsione
    - ricorsione disponibile
    - risposta di competenza (se il server non è competente per quel nome)
- Contatori che specificano il numero di:
	- domande
	- record di risposta (Answer RR)
	- record autorevoli (Authority RR)
	- record addizionali (Additional RR)

Dopo l’intestazione, il messaggio DNS è composto da **quattro blocchi**, corrispondenti alle quantità specificate nei campi dell’intestazione:
- Domande (Question Section): 
	- campi per il nome di dominio richiesto e il tipo di record cercato (A, MX, NS…)
- Risposte (Answer Section):
	- include i record DNS che rispondono alla query
	- RR per la risposta, se abbiamo replicati ci saranno più RR
	- se ci sono più risposte o duplicati, troveremo più record
- Competenza (Authority Section): 
	- fornisce informazioni sui server authoritative per il dominio richiesto
- Informazioni aggiuntive(Additional Section): 
	- contiene dati extra utili, ad esempio:
		- se la risposta è di tipo MX (mail exchange), qui potrebbe esserci il record A (IP Address) con l’IP del server di posta indicato dal record MX

![[file 58.png]]

##### Inserire record nel database DNS
Se ad esempio abbiamo appena creato una nuova società chiamata `Network Stud` e vogliamo registrare il nostro dominio all’interno di un DNS:
1. Prima cosa dobbiamo contattare un registrar, ovvero delle aziende accreditate dall’ICANN che verificherà la disponibilità del nome e lo inserirà nel database
2. Inseriamo in un nostro server DNS che sarà il server di competenza (ad esempio `dns1.networkstud.it`) e configuriamo i seguenti record:
	- **A Record**: `www.networkstud.it` → `150.160.15.12` (indirizzo del server web)
	- **MX Record**: `networkstud.it` → `mail.networkstud.it` (server di posta)
	- eventualmente, un **record CNAME** per `mail.networkstud.it` → `mail.google.com` (se usiamo Gmail per la posta)
3. A questo punto possiamo fornire al registrar i nomi e gli indirizzi dei nostri server DNS primario e secondario
4. Il registrar provvede a inserire nel **server TLD `.it`** due **resource record (RR)**:
    - un **record NS**: `(networkstud.it, dns1.networkstud.it, NS)`
    - un **record A**: `(dns1.networkstud.it, 212.212.212.1, A)` (IP di esempio)

![[file 59.png]]
Si possono interrogare i server DNS con il comando `nslookup` dal terminale[^6]

[^6]: - `nslookup www.di.uniroma1.it` - per cercare nei server DNS impostati di default gli indirizzi associati a `www.di.uniroma1.ti`
	- `nslookup -type=NS uniroma1.it` - per restituire soltanto i tipi di record `NS` associati a `uniroma1.it`
	- `nslookup -type=NS .` - per restituire i record `NS` del server radice ovvero `.`


## FTP
 Viene usato per trasferire file da / a un host remoto, va effettuata la connessione tramite il comando:
- `ftp NomeHost` e vengono richiesti nome utente e password
- `ftp > get file1.txt` per trasferire un file da un host remoto
- `ftp > put file2.txt` per trasferire un file verso l’host remoto

- FTP usa il modello client / server dove:
	- il server è la macchina che mantiene i dati
	- il client quello che richiede o invia

Quando il client richiede una connessione il server verifica le credenziali e stabilisce una connessione sulla **porta 21**

In realtà vengono stabilite due connessioni:
- **Connessione di controllo**: 
	- si occupa delle informazioni di controllo del trasferimento e 
	- usa regole semplici
	- è quella utilizzata per scambiare i comandi come ad esempio il cambio di directory
	- è una connessione persistente
- **Connessione dati**: 
- si occupa del trasferimento dei file vero e proprio
- questa viene aperta sulla _porta 20_
- la connessione non è persistente:
- viene chiusa alla fine di ogni trasferimento

#### Comandi e Risposte FTP

Ogni comando eseguito corrisponde ad una risposta da parte del server, le risposte sono molto simili a quelle già viste in HTTP.

Alcuni comandi comuni:
- `USER username`
- `PASS password`
- `LIST`: elenca i file nella directory corrente, questa viene inviata in una nuova connessione dati.
- `RETR filename` recupera un file dalla directory corrente
- `STOR filename` memorizza un file nell’host remoto

Le risposte sono composte da un codice e una parte testuale supplementare per informazioni aggiuntive, alcuni codici di ritorno delle risposte:
- `331 username OK, password required`
- `125 data connection already open; transfer starting`
- `425 Can´t open data connection`
- `452 Error writing file`

![[file 60.png]]

# Posta Elettronica
Vediamo i principali componenti utilizzati per la comunicazione mail:
![[file 61.png]]
- User Agent: app/programma usato dall'utente per scrivere, leggere e inviare messaggi di posta elettronica (Gmail)
- Message Transfer Agent: Usato per trasferire il messaggio attraverso Internet (Postfix, Sendmail, Microsoft Exchange Server)
- Message Access Agent: Usato per leggere la mail in arrivo (IMAP o POP3 sono i protocolli più usati, il software può essere lo stesso User Agent (es. Outlook che usa IMAP per leggere la posta)
	(Gmail dal telefono, l’app (UA) usa IMAP (MAA) per recuperare le email dal server di Google)

##### User Agent
- viene attivato da un timer o dall’utente e informa quest’ultimo di nuove mail in arrivo
- in messaggi in uscita o in arrivo vengono memorizzati sul server
- i messaggi da inviare inoltre vengono passati al Mail Transfer Agent
##### comunicazione MTA
- comunicano tramite i _server di posta_ che hanno una mailbox contenente:
	- tutti i messaggi in arrivo per gli utenti
	- _coda di messaggi da trasmettere_ (tentativi ogni $x$ minuti per alcuni giorni)
- sia per inviare messaggi tra i server, sia tra User Agent mittente e destinatario(il suo server di posta) si usa il protocollo **SMTP** (Simple Mail Transfer Protocol) 

### SMTP
- utilizza una connessione TCP client-server sulla **porta 25**
- trasferimento diviso in 3 fasi:
	- Handshaking: Quando i due server devono “riconoscersi”
	- Trasferimento di Messaggi
	- Chiusura

![[file 62.png]]
1. Alice usa il suo agente per comporre il messaggio da inviare alla mail di Roberto
2. L’agente di Alice invia il messaggio al server di posta di Alice, questo viene inserito nella coda dei messaggi
3. Il lato client SMTP apre una connessione TCP con il server di posta di Roberto
4. Il client SMTP invia il messaggio di Alice sulla connessione TCP
5. Il server di posta di Roberto riceve il messaggio e lo pone nella casella di posta di Roberto
6. Roberto invoca il suo agente utente per leggere il messaggio

Dal client mittente fino al server del destinatario si usa il protocollo SMTP poi il destinatario per scaricare la mail sul suo client può usare IMAP o POP

###### Scambio di messaggi nel dettaglio

1. Il client SMTP (server posta in invio) fa stabilire una connessione sulla porta 25 verso il server SMTP ricevente
2. Client e Server effettuano handshaking ovvero il client indica indirizzo email di mittente e destinatario
3. Il client invia il messaggio
4. Il messaggio arriva al server destinatario
5. Se ci sono altri messaggi si usa la stessa connessione, questa infatti è persistente, altrimenti si chiude

![[file 63.png]]

##### Formato dei Messaggi di Posta Elettronica
Abbiamo due componenti principali:
- Intestazione, composta da:
    - To: Indirizzo/i destinatari
    - From: Indirizzo del mittente
    - Cc: Indirizzo di uno o più destinatari a cui si invia per conoscenza (carbon copy)
    - Bcc: È come Cc ma i veri destinatari non sanno che anche questi in Bcc hanno ricevuto il messaggio (blind carbon copy)
    - Subject: Argomento del messaggio
    - Sender: Chi materialmente effettua l’invio, ad esempio il dipendente dell’azienda.
- Corpo: il messaggio vero e proprio composto soltanto da caratteri ASCII

### Protocollo MIME
è un protocollo che aggiunge delle estensioni di messaggi di posta per inviare dei messaggi in formati non ASCII, quindi convertendo i dati prima di inviarli
(intestazioni aggiuntive)

![[file 64.png]]

### Protocolli di Accesso alla Posta
Il client dell’utente finale può usare diversi protocolli per accedere al server della post:
-  SMTP svolge solo azioni di **push**, viene usato per trasferire i messaggi fra i server ma non può essere usato dall’utente
- il client dell’utente finale ha bisogno di eseguire azioni di **pull** per scaricare e leggere i messaggi in arrivo dal server di posta

Possiamo usare diversi protocolli per le azioni di pull:
- POP3 (Post Office Protocol - v.3): Autorizzazione e download
- IMAP (Internet Mail Access Protocol): Permette più funzioni e manipolazione di messaggi sul server
- HTTP: Servizi come gmail, hotmail ecc…


#### POP3
permette al client di aprire una connessione TCP verso il server di posta sulla **porta 110**, poi si procede in 3 fasi:

1. Autorizzazione: l’agente utente si identifica
2. Transazione: l’agente utente recupera i messaggi
3. Aggiornamento: dopo che il client ha finito e chiusa la connessione vengono cancellati i messaggi marcati per la rimozione

Comandi utilizzabili con POP3, possiamo distinguerli in due fasi della comunicazione:

- Fase di Autorizzazione
    - `user` dichiara il nome dell’utente
    - `pass` password
    - Il server può rispondere con `+OK` o `-ERR`
- Fase di Transazione
    - `list` elenca i numeri dei messaggi
    - `retr` ottiene i messaggi in base al numero
    - `dele` cancella
    - `quit`

Con il POP3 quindi, usando la modalità “scarica e cancella” non possiamo rileggere le mail se cambiamo client, queste infatti non saranno più disponibili sul server

L’utente infatti non può nemmeno creare cartelle o altro sul server, va tutto creato in locale sul proprio computer


![[file 65.png|350]]

#### IMAP
- IMAP consente di creare cartelle anche sul server e quindi di organizzare i messaggi
- a differenza di POP, IMAP mantiene lo stato delle sessioni dell'utente
- il protocollo per fare questo associa a una cartella ogni messaggio arrivato dal server e poi fornisce agli utenti dei comandi per:
	- creare cartelle e spostare messaggi da una cartella all’altra
	- effettuare ricerche nelle cartelle del server Mantengono informazioni su:
	- nomi cartelle
	- associazioni messaggi - cartelle
- inoltre vengono forniti comandi per ottenere le componenti dei messaggi:
	- intestazione
	- corpo del messaggio

#### HTTP
- alcuni mail server consentono l’accesso tramite web ovvero tramite HTTP
- in questo caso l’agente utente diventa il browser 
- comunica con il suo mailbox tramite HTTP
- e anche il ricevente usa HTTP per accedere alla mailbox


# Livello Trasporto
- a livello di trasporto abbiamo una connessione logica ovvero gli host eseguono i processi come se fossero direttamente connessi
- i protocolli di trasporto vengono eseguiti nei sistemi terminali e funzionano con un meccanismo di incapsulamento quando si invia un messaggio e decapsulamento quando si riceve
<br>
- a livello di rete abbiamo una comunicazione tra host basata sui servizi del livello di collegamento
- mentre a livello di trasporto una comunicazione tra processi basata sui servizi del livello di rete
![[file 67.png]]

esempio:

1 persona di un condominio invia una lettera a 1 persona di un altro condominio consegnandola/ricevendola a/da un portiere

Possiamo individuare le seguenti figure:

- processi = persone
- messaggi delle applicazioni = lettere nelle buste
- host = condomini
- protocollo di trasporto = portieri dei condomini
- protocollo del livello di rete = servizio postale

Quindi notiamo che _i protocolli di trasporto_ (portieri) _lavorano a livello locale_ e non sono coinvolti al di fuori

![[file 66.png|350]]

## Indirizzamento
I nuovi sistemi operativi sono multiutenti e multiprocesso questo significa che sia a livello client che server avremo diversi processi attivi. Per stabilire una comunicazione tra due processi sarà quindi necessario individuare:
- host locale
- host remoto
- processo locale
- processo remoto

identificazioni:
- l’host verrà identificato tramite un indirizzo IP
- il processo tramite un numero di porta

![[file 68.png]]

- la combinazione di indirizzo IP + porta prende il nome di **socket address**

### Incapsulamento / Decapsulamento
Prima di venire spediti i dati vengono incapsulati, ovvero viene aggiunto un header che verrà poi riconosciuto quando arriva a destinazione
![[file 69.png]]
Questi pacchetti si chiamano:
- segmenti se usiamo TCP
- datagrammi utente se usiamo UDP

Questo processo prende anche il nome di **Multiplexing / Demultiplexing**
![[file 70.png]]

_Multiplexing_:
- il multiplexing a livello mittente raccoglie i dati da vari socket e li incapsula con l’intestazione corrispondente che verrà poi usata per il demultiplexing
- ad esempio se in un host abbiamo due processi uno FTP e uno HTTP l’host dovrà raccogliere i dati da questi socket e passarli al livello di rete

_Demultiplexing_:
- deve consegnare i segmenti ricevuti al socket proprietario
- li riconosce grazie agli header di intestazione di ogni messaggio

funzionamento nello specifico:
- l’host riceve i datagrammi IP, ognuno di questi ha un indirizzo IP di origine e di destinazione
- ogni datagramma trasporta un segmento a livello di trasporto
- ogni segmento ha un numero di porta per origine e uno per destinazione

I pacchetti UDP e TCP sono strutturati nel seguente modo:
![[file 71.png|350]]

- i campi per la porta occupano 16 bit
- possono assumere valori da 0 a 65535
- i primi 1023 sono usati per servizi specifici, vengono chiamati **well known-port number**

in generale funziona così:
![[file 72.png]]

### Socket
- i socket permettono la comunicazione fra processi
- questi appaiono si come terminali o file ma in realtà non sono entità fisiche
- sono delle astrazioni, una struttura dati creata dal programma applicativo

comunicare tra un processo client e uno server significa comunicare tra due socket

sono composti da:
- 32 bit di indirizzo IP
- 16 bit numero di porta

- l’interazione tra client e server è _bidirezionale_, ci serve quindi una coppia di indirizzi socket uno locale e uno remoto ovvero mittente e destinatario
- questi due indirizzi si scambiano di posto a seconda del ruolo ovvero il mittente in una direzione è il destinatario nell’altra

Individuazione degli indirizzi socket:
- lato Client:
	- Il client ha bisogno di socket address locale (client) e remoto (server)

	- socket address locale: viene fornito dal sistema operativo questo infatti conosce l’indirizzo IP del computer sul quale il client è in esecuzione e anche il numero di porta assegnato
	- socket address remoto: il numero di porta varia in base all’applicazione che stiamo usando, l’indirizzo viene fornito dal DNS oppure il socket intero è conosciuto dal programmatore se ad esempio sta testando un’applicazione

- lato Server:
	- anche il server ovviamente ha bisogno di socket address locale (server) e remoto (client)

	- socket address locale: fornito dal sistema operativo, conosce l’indirizzo IP del computer e il numero di porta è noto al server perché assegnato dal progettista
	- socket address remoto: è il socket address locale del client che si connette, questo viene trovato dal server all’interno del pacchetto di richiesta

## Servizi di Trasporto
Usiamo 2 servizi:
- TCP: affidabile ma più lento
- UDP: non affidabile ma più veloce

### TCP
- **orientato alla connessione**: è richiesto un setup fra i processi client e server

funziona come un tubo:
- il trasmettitore spinge i bit da una estremità e il ricevitore li prende dall’altra
- viene quindi mantenuto l’ordine di trasmissione

il TCP offre:
- trasporto affidabile
- controllo di flusso: il mittente evita il sovraccarico del destinatario
- controllo della congestione: se la rete è sovraccaricata si riduce l’invio di dati

non offre:
- sistemi di sicurezza dei dati
- temporizzazione [^7]
- garanzia su ampiezza di banda minima


[^7]: si riferisce alla gestione del tempo e dei ritardi nella trasmissione dei dati tra due punti in una rete

### UDP - User Datagram Protocol
- funziona **senza connessione** ovvero non è richiesto alcun setup fra processi client e server
- abbiamo un trasferimento inaffidabile infatti i dati potrebbero non arrivare nello stesso ordine in cui li inviamo
- non offre alcuni servizi del TCP, ossia:
	- il controllo del flusso
	- controllo della congestione

usato:
- principalmente per dati che appunto non richiedono affidabilità, come ad esempio streaming video o telefonia internet come Skype
- comunemente usato per i broadcast, in quanto consente di inviare dati a più destinatari contemporaneamente senza la necessità di stabilire una connessione diretta con ciascuno di essi

offre i servizi:
- comunicazione tramite socket
- multiplexing e demultiplexing dei pacchetti
- incapsulamento e decapsulamento [^8]

non fornisce:
- controllo errori (è ammesso solo checksum)
- controllo flusso o controllo congestione

[^8]: 1. **Multiplexing e Demultiplexing**: si riferiscono alla gestione dei dati
		- Il multiplexing è il processo di combinare più flussi di dati in un singolo flusso per la trasmissione, mentre 
		- il demultiplexing è il processo inverso, cioè separare quel flusso combinato nei flussi originali
		- questo è utile per inviare più dati attraverso una singola connessione
	
	2. **Incapsulamento e Decapsulamento**: si riferiscono alla preparazione e alla ricezione dei dati
		- l'incapsulamento è il processo di avvolgere i dati in un pacchetto, aggiungendo le informazioni necessarie per la trasmissione (come indirizzi e controlli)
		- il decapsulamento è il processo di estrazione dei dati dal pacchetto ricevuto, rimuovendo le informazioni aggiuntive


- il mittente invia i pacchetti uno dopo l’altro e non si preoccupa dell’effettivo arrivo al destinatario
- ogni pacchetto è indipendente dagli altri
- non c’è coordinazione tra livello di trasporto mittente e destinatario 
- si verificano anche situazioni in cui l'ordine di arrivo è diverso da quello di invio
![[file 73.png]]


##### Rappresentazione tramite FSM
Possiamo rappresentare il comportamento di un protocollo di trasporto come un automa a stati finiti, questo rimane in uno stato finché un evento non lo modifica.

La rappresentazione per UDP è:
![[file 74.png]]

- abbiamo soltanto lo stato `ready`
- quando arriva un pacchetto o si deve inviarne uno, viene eseguita l'azione
- una volta terminata, il sistema ritorna nello stato `ready`

#### Datagrammi UDP
Dato che non c’è flusso di dati, i processi devono inviare richieste di dimensioni sufficientemente piccole per essere inserite in un singolo datagramma

Solo i processi che usano messaggi di dimensioni inferiori a 65507 byte possono utilizzare UDP, questo perché abbiamo:

- 65535 - 8 byte di intestazione UDP e -20 byte di intestazione IP
![[file 75.png]]

struttura datagrammi UDP
![[file 76.png]]

##### Checksum
Serve a rilevare errori nel datagramma trasmesso
ruolo mittente e destinatario:
- mittente
	- divide il messaggio in “parole” da 16 bit
	- il checksum viene inizialmente impostato a 0
	- tutte le parole incluso il checksum vengono sommate usando l’addizione complemento a 1
	- viene fatto il complemento a 1 della somma e quello che otteniamo è il checksum
	- viene inviato il messaggio
- destinatario
	- viene ricevuto il messaggio
	- viene diviso in parole da 16 bit
	- tutte le parole vengono sommate con complemento a 1
	- viene fatto il complemento a 1 della somma ed il risultato è il nuovo checksum
	- se il checksum vale 0 allora il messaggio viene accettato altrimenti si scarta

esempio: inviare la seguente stringa da 32 bit: `11100110011001101101010101010101`
la dividiamo quindi in parole da 16 e le sommiamo:
![[file 77.png]]


- UDP viene utilizzato anche da DNS, le applicazioni DNS inviano una query e aspettano una risposta dal server, se questa non arriva semplicemente fanno una query ad altri server
- quindi la semplicità delle richieste DNS giustifica l’utilizzo di UDP che è anche più veloce
- in generale viene spesso utilizzato per applicazioni di streaming multimediale dato che tollera una limitata perdita di pacchetti:
![[file 78.png]]


### intro a TCP
possiamo vederlo come un flusso di byte che passa in tubo:
![[file 79.png]]

TCP a differenza di UDP ci offre anche:
- un trasporto orientato alla connessione dove serve quindi un setup fra i client
- controllo del flusso
- controllo degli errori
- controllo della congestione



#### Demultiplexing orientato alla connessione
La socket TCP è identificata da 4 parametri:[^9]
- indirizzo IP e porta di origine
- indirizzo IP e porta di destinazione

un host server può supportare più socket TCP contemporanee dato che ogni socket è identificata dai 4 parametri
si avrà una socket differente anche per lo stesso client, ogni richiesta viene trasmessa su una diversa socket

- il server accetta tutte le connessioni sulla stessa socket e poi le sposta su una porta differente
![[file 80.png]]

- essendo orientato alla connessione non si verifica più l’evento che i messaggi arrivano in ordine diverso dall’invio
- nel caso in cui in messaggio arrivi prima questo viene memorizzato a livello di trasporto e trasmesso solo dopo l’arrivo del suo predecessore


[^9]: un **socket TCP** è un **endpoint** per una connessione di rete che utilizza il protocollo **TCP (Transmission Control Protocol)**
	i socket permettono a due programmi (di solito su macchine diverse) di comunicare tra loro



#### Rappresentazione tramite FSM
![[file 81.png]]

- l’apertura della connessione inizia dallo stato `closed` dove i due host si inviano dei pacchetti per stabilire la connessione
- una volta stabilita possono scambiarsi i dati
- quando hanno terminato si inviano un messaggio per chiudere la connessione

#### Controllo di Flusso
Quando un host produce dati che un altro host deve consumare deve esserci equilibrio fra velocità di produzione e di consumo:
- Se produzione > consumo: il consumatore potrebbe sovraccaricarsi e potrebbe eliminare alcuni dati
- Se consumo > produzione: Il consumatore rimane in attesa di dati riducendo l’efficienza del sistema.

Il controllo del flusso però si occupa principalmente della prima problematica ovvero la perdita di dati


Ci occupiamo del controllo del flusso a livello di trasporto, individuiamo 4 entità:
- Processo e trasporto mittente
- Processo e trasporto destinatario

Ci sono 2 casi di controllo di flusso:
![[file 82.png]]

realizzazione del controllo del flusso:
- implementiamo dei buffer ovvero delle zone di memoria che memorizzano i pacchetti
- il consumatore invia dei segnali al produttore:
    - il livello trasporto del mittente segnala al livello applicazione di sospendere l’invio dei messaggi quando il buffer è saturo
	    - quando si svuota si può riprendere l’invio
    - il livello trasporto del destinatario segnala al trasporto del mittente di sospendere l’invio dei messaggi quando il buffer è saturo
	    - quando si svuota si può riprendere

#### Controllo degli Errori
dato che il livello di rete è inaffidabile, l’affidabilità va implementata a livello di trasporto e per farlo abbiamo bisogno di implementare un controllo degli errori che agisce sui pacchetti:
- rilevare e scartare pacchetti corrotti
- tracciare i pacchetti persi e il loro rinvio
- riconoscere pacchetti duplicati
- bufferizzare i pacchetti fuori sequenza finché non arrivano quelli mancanti

*È il livello trasporto del destinatario a gestire il controllo errori e segnalarli al livello trasporto mittente*

realizzazione:
- numeriamo i pacchetti con un campo nell’header, numerazione sequenziale

Poiché il numero di sequenza va inserito nell’intestazione, va specificata la dimensione massima:
- se l’intestazione prevede $m$ bit per il numero di sequenza questi possono assumere valori da $0$ a $2^m−1$
- i numeri di sequenza sono quindi considerati in modulo $2^m$ (ciclici)

Questo numero di sequenza serve al destinatario per:
- capire la sequenza dei pacchetti in arrivo
- se ci sono pacchetti persi
- se ci sono pacchetti duplicati

il mittente come fa a capire che un pacchetto è andato perso?
- il destinatario ad un ogni pacchetto ricevuto invia un **ACK (acknowledgment)** con il numero del pacchetto che ha ricevuto che funziona da notifica di ricezione

#### Integrazione del controllo degli errori e del flusso
- il controllo del flusso richiede 2 buffer uno per il mittente e uno per il destinatario
- il controllo degli errori richiede un numero di sequenza e dei messaggi di ACK

Possiamo combinare i due meccanismi tramite dei buffer numerati nel mittente e nel destinatario:

Mittente
- quando prepara un pacchetto usa come come numero di sequenza il numero $x$ della prima locazione libera nel buffer
- quando invia il pacchetto ne memorizza una copia nella locazione $x$ del buffer
- quando riceve il segnale ACK di un pacchetto libera la posizione di memoria occupata da quel pacchetto

Destinatario
- quando riceve un pacchetto con numero $y$ lo memorizza nella locazione $y$ fino a quando il livello applicazione non è pronto a riceverlo
- quando passa il pacchetto al livello applicazione invia ACK al mittente

Dato che i numeri di sequenza sono calcolati in modulo $2^m$ e quindi ciclici possiamo rappresentarli tramite un cerchio
Il buffer viene rappresentato con un insieme di settori chiamati **sliding windows** che occupano una parte del cerchio
![[file 83.png]]

- finché il destinatario non svuota il suo buffer non vengono inviati nuovi pacchetti in modo da non sovraccaricare la rete

Nella realtà per memorizzare i numeri di sequenza del prossimo pacchetto da inviare e dell’ultimo inviato si usano delle variabili e le sliding windows vengono rappresentate in modo lineare:
![[file 84.png]]

#### Controllo della Congestione
nella commutazione a pacchetto la congestione avviene se il carico della rete ovvero il numero di pacchetti su di essa è superiore alla sua capacità
- con il controllo della congestione usiamo delle tecniche per fare in modo che questa situazione non si verifichi mai

si parla di congestione perché arrivano più pacchetti di quelli che router e switch riescono a gestire:
- quindi si riempiono le loro code portando alla perdita di pacchetti e rallentamenti

## Livello Trasporto - Meccanismi di Trasferimento dati Affidabili

### Stop and Wait
sia mittente che destinatario hanno una finestra scorrevole di un solo pacchetto, quindi:
- il mittente invia un pacchetto e attende il suo ACK prima di inviarne un altro
- quando il pacchetto arriva, il destinatario calcola il suo checksum e se corretto invia un ACK altrimenti lo scarta e non informa il mittente
- il mittente capisce che il pacchetto non è stato ricevuto correttamente tramite un timer

questo significa che il mittente deve tenere una copia del pacchetto fino a che non riceve il suo ACK

questo meccanismo garantisce:
- controllo errori: tramite il **numero di sequenza** dei pacchetti, l’ACK e il timer
- controllo del flusso: non è possibile inviare più di un pacchetto quindi non possono arrivare in ordine sparso

per gestire i duplicati, lo stop and wait utilizza i numeri di sequenza, in questo caso 0 ed 1 sono sufficienti per il meccanismo

quando viene ricevuto un pacchetto si invia un ACK con il numero di sequenza successivo, quindi se ricevo 0 invio ACK 1 e se ricevo 1 invio ACK 0, questo sta ad indicare qualche pacchetto sta aspettando

_FSM che rappresenta il Mittente_
![[file 85.png]]
quindi invia un pacchetto e aspetta o un riscontro o la fine del timer prima di inviarne un altro

_FSM che rappresenta il Destinatario_
![[file 86.png]]
sempre pronto a ricevere pacchetti, non va mai in stato di blocked

#### Efficienza
Prendiamo ad esempio un sistema che ha:
- Rate = `1Mbps`
- Ritardo di andata e ritorno di 1 bit = `20 ms`

Abbiamo un valore di `rate * ritardo = 20'000 bit/ms`

I pacchetti hanno dimensione `1000 bit` quindi significa che l’utilizzo del canale è `1000 / 20000 = 5%` dato che possiamo inviare un solo pacchetto alla volta
abbiamo quindi una rete molto inefficiente

Utilizziamo **protocolli con pipeline** ovvero dove il mittente può inviare più di un pacchetto alla volta
vediamo **Go-Back-N** e **Ripetizione Selettiva**

### Go Back N
in questo meccanismo i numeri di sequenza sono calcolati modulo $2^m$ dove $m$ è la dimensione del numero di sequenza in bit

anche qui inviamo gli ACK con il numero successivo a quello ricevuto, ovvero quello che stiamo aspettando
la differenza è che qui usiamo un **ACK cumulativo**: ovvero tutti i pacchetti fino al numero di sequenza indicato nell’ACK sono stati ricevuti correttamente

quindi se ACK = 7 -> significa che fino a 6 è stato tutti ricevuto e si sta aspettando il pacchetto 7

la nostra finestra di scorrimento per l’invio non è più quindi da un solo slot:
![[file 87.png]]
- `Sf` indica il primo inviato, per il quale stiamo ancora aspettando un ACK
- `Sn` indica il prossimo da inviare appena possibile

1uindi se ad esempio arriva un ACK = 6
![[file 88.png]]

per quanto riguarda la finestra di ricezione invece rimaniamo con un solo slot
- il destinatario è sempre in attesa di un solo pacchetto e qualsiasi arriva fuori sequenza viene scartato

quindi se ad esempio siamo in attesa di 5 e arrivano 6,7 questi verranno scartati e dovranno essere rinviati successivamente

qui entra il gioco il timer:
- viene mantenuto per il più vecchio pacchetto inviato e per il quale non abbiamo ancora ricevuto un ACK
- quando il timer scade “Go Back N” ovvero vengono rispediti tutti i pacchetti in attesa di riscontro

esempio:
 il primo pacchetto inviato e su quale si trova il timer è 3 (`Sf = 3`) e il mittente invia 6 quindi abbiamo `Sn = 7`
 non arrivano ACK e scade il timer quindi vengono rinviati i pacchetti 3,4,5,6

_FSM per rappresentare il mittente_
![[file 89.png]]
_FSM per il destinatario_
![[file 90.png]]


Ma con una finestra di $2^m$ i duplicati vengono gestiti correttamente?[^10]

[^10]: _sending window_, è un concetto fondamentale nei protocolli affidabili come **Go-Back-N** e **Sliding Window**, usati per controllare **quanti pacchetti** un mittente può **inviare senza dover aspettare gli ACK** dal destinatario

![[file 91.png]]
in questo caso la finestra di ricezione viene riempita e tutti i segnali di ACK sono stati persi, cosa succede?
- il destinatario sta di nuovo aspettando un pacchetto 0
	- e scade anche il timer del mittente
- vengono quindi rispediti tutti i pacchetti e ricevuti erroneamente come corretti quando in realtà erano già stati ricevuti

La dimensione corretta per la finestra di invio è 2m−1:
![[file 92.png]]
In questo modo i pacchetti già ricevuti ma per i quali sono andati persi gli ACK vengono scartati quando ricevuti nuovamente (duplicati)

### Ripetizione Selettiva
- nel Go Back N se un pacchetto non viene ricevuto correttamente dobbiamo rinviare tutti quelli presenti nella finestra
- questo è molto efficiente se la rete funziona sempre correttamente ma se ad esempio c’è congestione non lo è

nella ripetizione selettiva vengono rispediti soltanto i pacchetti per i quali non è stato ricevuto un ACK:
- in questo caso la finestra di invio e ricezione hanno la stessa dimensione

![[file 93.png]]

- per implementare questo meccanismo abbiamo bisogno di un timer per ciascun pacchetto in attesa di riscontro
- quando un timer scade viene rispedito soltanto quel pacchetto

_FSM per rappresentare il mittente_
![[file 94.png]]
_FSM per Destinatario_
![[file 95.png]]

le dimensione delle finestre abbiamo detto essere uguali e in questo meccanismo sono calcolate modulo $2^{m-1}$

perché non possiamo usare $2^m-1$?
![[file 96.png]]

- in questo caso scade il timer per 0 e viene rispedito
- il destinatario lo prende come nuovo pacchetto ma in realtà è un duplicato

usando $2^{m-1}$:
![[file 97.png]]



> [!info] Protocolli Bidirezionali: Piggybacking
> i meccanismi che abbiamo visto li abbiamo presentati in modo unidirezionale ovvero i pacchetti vengono spediti in una direzione e gli ACK in un'altra ma in realtà questi viaggiano in entrambe le direzioni, sia pacchetti che ACK
> 
   per migliorare l'efficienza dei protocolli bidirezionali si usa la tecnica **piggybacking** ovvero quando un pacchetto trasporta dati da A a B può trasportare anche gli ACK relativi ai pacchetti ricevuti da B e viceversa

In generale:
![[file 98.png]]

## TCP
il protocollo TCP raggruppa un certo numero di byte da inviare in un segmento al quale poi aggiunge un’intestazione

### Struttura Segmenti
![[file 99.png]]

- numero di sequenza: è il numero del primo byte dei dati trasmessi nel segmento
- Acknowledgment number: è il numero di sequenza del prossimo byte che il client si aspetta di ricevere dal server

Invece le _flag_ colorate in celeste indicano:
- URG: puntatore urgente, questi dati vanno elaborati subito
- ACK: riscontro valido
- PSH: richiesta di push ovvero inviare i dati all’applicazione non appena possibile
- RST: azzeramento connessione
- SYN: sincronizzazione dei numeri di sequenza
- FIN: chiusura della connessione

### Connessione TCP
La connessione TCP crea un percorso virtuale tra mittente e destinatario sopra all’indirizzo IP
	IP, spedisce e basta, ed è privo di connessione
	TCP, stabilisce la connessione


Una connessione TCP è caratterizzata da 3 fasi:
1. apertura connessione
2. trasferimento dati
3. chiusura connessione

#### Apertura della connessione
L’apertura della connessione avviene tramite un meccanismo chiamato **3 way handshake**:
![[file 100.png]]

1. il client invia un segmento con la flag `SYN`
	- il numero di sequenza `seq` viene scelto casualmente dal client, in questo caso 8000
2. il server riceve il messaggio e risponde con un `ACK + SYN`
	- manderà ACK = 8001 dato che ha ricevuto 8000, quindi si aspetta dati a partire da 8001
	- il suo `seq = 15000`anche questo è scelto a caso
3. Il client risponde con un ultimo `ACK`

Adesso si può iniziare a trasferire dati
#### Trasferimento dati: Push
![[file 101.png]]
1. il client invia due segmenti da 1000 byte:
	- i loro numeri di sequenza vanno da 8001 a 9000 e da 9001 a 10000
2. il server risponde con un `ACK = 10001`
	1. e invia il proprio segmento con numero di sequenza da 15001 a 17000
	- il campo `rwnd = 3000` indica quanti byte può ricevere per volta il server, `receive window`
3. Il client invia la sua `receive window` e un `ACK = 17001`

Quando un'applicazione ha dei dati urgenti o vuole che il destinatario li elabori subito, imposta PSH = 1
- il destinatario, una volta ricevuto il segmento con PSH, non aspetta di riempire ulteriormente il buffer TCP: passa subito i dati all’applicazione utente

##### Trasferimento dati: Urgent
i dati che hanno un flag `URG` vengono elaborati subito, indipendentemente dalla loro posizione nel flusso di byte

questi dati urgenti vengono inseriti all’inizio di un nuovo segmento che potrà comunque avere dati non urgenti ma solo dopo quelli urgenti

il campo `URG` viene fornito insieme ad un puntatore, questo puntatore indica dove finiscono i dati urgenti e iniziano quelli normali nel nuovo segmento

#### Chiusura della Connessione
entrambi le parti coinvolte nella connessione possono richiedere la chiusura
di solito questa viene chiesta dal client, oppure dal server per timeout

La procedura è simile a quella per aprire la connessione ma al posto di segnali `SYN` vengono inviati segnali `FIN`:
![[file 102.png]]

Esiste anche un altro tipo di chiusura, ovvero la **half - close**
- non è sincronizzata fra le due parti
- la chiusura infatti avviene solo da un lato mentre dall’altro la connessione rimane aperta
- una delle due parti ha finito di inviare i dati e chiude la connessione
- ma vuole comunque continuare a ricevere dall’altra parte finché anche questa non manderà un `FIN`

![[file 103.png]]

### Controllo degli Errori
Come gestisce TCP i numeri di sequenza e gli ACK:
- numeri di sequenza: numero del primo byte del segmento nel flusso di byte
	- il numero iniziale viene scelto a caso
- ACK: numero di sequenza del prossimo byte atteso dall’altro lato
	- usa inoltre ACK cumulativi
![[file 104.png]]

per la gestione dei segmenti fuori sequenza:
- non ci sono specifiche TCP
- di solito è il destinatario che li mantiene in memoria e poi li “riceve” quando rispettano il giusto ordine

### Affidabilità
- si usa la **checksum** per scartare i segmenti che arrivano corrotti
- usa degli ACK cumulativi e un timer associato al più vecchio pacchetto non riscontrato
- il segmento viene ritrasmesso all’inizio della coda di spedizione[^11] 

Vediamo come si comporta TCP in varie situazioni:

**Regola 2**
- arriva un segmento ordinato che ha il numero di sequenza attesto, allora tutti i dati precedenti sono stati riscontrati
- per rispondere con un ACK si aspetta 500ms, in questo modo se arriva anche il segmento successivo viene inviato un solo ACK ma se entro questi 500ms non viene ricevuto il successivo si invia l’ACK singolo

---

**Regola 3**
- arriva un segmento con numero di sequenza atteso e un altro segmento precedente è in attesa di trasmissione dell’ACK
- si invia un singolo ACK cumulativo per entrambi

---

**Regola 4**
- arriva un segmento non ordinato che ha quindi un numero di sequenza superiore a quello atteso e viene quindi rilevato un buco
- viene subito inviato un ACK duplicato indicando il numero di sequenza del prossimo byte atteso
	- questa procedura prende il nome di **ritrasmissione veloce**

---

**Regola 5**
- arriva un segmento mancante ovvero è già stato ricevuto un suo successivo
- viene inviato subito un ACK

---

**Regola 6**
- arriva un segmento duplicato
- invia un ACK che contiene il numero di sequenza atteso


[^11]: La coda di spedizione (send buffer) è una **memoria nel lato mittente** dove vengono conservati temporaneamente i segmenti trasmessi ma non ancora confermati (ACK)


### Ritrasmissione dei segmenti
quando un segmento viene inviato viene comunque mantenuta una copia in una coda, in attesa di ricevere un riscontro

Se questo riscontro non arriva possono accadere due cose:
- se è il primo segmento all’inizio della coda può scadere il timer e viene quindi ritrasmesso
	- viene anche rinviato il timer
- vengono ricevuti 3 ACK duplicati e quindi avviene una **ritrasmissione veloce** del segmento senza aspettare il timer

_FSM Mittente_
![[file 105.png]]

_FSM Destinatario_
![[file 106.png]]

Usando quindi i timer per gli ACK cumulativi avremo una situazione simile:
![[file 108.png]]

quando il client riceve 4001-5000 si aspetta 5001:
- attende 500ms e non riceve niente e quindi invia ACK di 5001

il server risponde con 5001-6000:
- quindi il client aspetta per 500ms il 6001
- dato che riceve 6001-7000 entro la finestra invia un singolo ACK di 7001

### Segmento Smarrito
![[file 109.png]]
1. il client invia in ordine 501-600 e 601-700
	- il server aspetta 701
2. 701-800 viene spedito ma smarrito
3. il client invia anche 801-900 e il server lo riceve
	- siccome ha ricevuto un pacchetto superiore a quello che si aspettava rinvia un ACK di 701
4. il client lo riceve e rispedisce 701-800
5. a questo punto il server, una volta ricevuto spedisce l’ACK dei prossimi ovvero 901

### Ritrasmissione Rapida
![[file 110.png]]
- viene perso il pacchetto 301-400
- una volta che il client ha ricevuto 3 ACK duplicati per lo stesso pacchetto inizia la ritrasmissione veloce **prima dello scadere del timer** del 301

### Riscontro smarrito senza ritrasmissione
![[file 111.png]]
- viene smarrito l'ACK di 701 ma non viene ritrasmesso dato che il server ha ricevuto dei segmenti successivi quindi può inviare direttamente ACK di 901
	- non sempre accade questo

### Riscontro smarrito con ritrasmissione
![[file 112.png]]
in questo caso l’ACK viene rispedito:
- il server invia ACK 701 e il client non lo riceve
- il client rispedisce quindi il primo pacchetto e il server riceve un duplicato
- dato che è stato ricevuto un duplicato il server rinvia l’ACK con il numero di sequenza atteso


### Riassunto su meccanismi del TCP
- Pipeline
- Numero di sequenza
- ACK cumulativo e delayed
	- conferma tutti i precedenti e posticipato nel caso di segmenti in sequenza
- timeout basato su RTT, ovvero si ha un unico timer di ritrasmissione associato al più vecchio segmento non riscontrato
	- se arriva un ACK intermedio si riavvia il timer del più vecchio non riscontrato
- ritrasmissione
    - singola ovvero solo sul segmento non riscontrato
    - rapida quando si riceve il terzo ACK duplicato prima del timeout del timer

### Controllo del Flusso
L’obiettivo del controllo del flusso per il mittente è quello di non sovraccaricare il buffer del destinatario inviando troppi dati troppo velocemente
- per fare questo il destinatario comunica al mittente lo spazio disponibile attraverso il campo `RWND` dell’header
![[file 113.png]]

La finestra di invio è completamente gestita dal destinatario:
![[file 114.png]]
mano a mano che vengono ricevuti i riscontri e inviati segmenti, la finestra si sposta verso destra
riduciamo quindi la grandezza della finestra in base alla situazione attuale della rete e del destinatario

esempio:
![[file 115.png]]
1. viene inviato il pacchetto 100
2. il server con un ACK comunica la sua finestra grande 800
	- il client quindi adatta la sua finestra di invio a 800
3. il client aggiusta la sua finestra di invio e trasmette un ACK per stabilire la connessione
4. il client invia 200 byte di dati (parte grigia)
	- il server li riceve e fa scorrere la sua finestra di ricezione, 800-200 = 600
5. il server invia ACK contenente anche la nuova finestra da 600
	- Il client la riceve e fa scorrere anche la sua
6. il client invia altri 300 byte
	- il server li riceve e passa a 300 byte di finestra
	- 100 byte di quelli ricevuti vengono passati all’applicazione e quindi li riguadagna la finestra, siamo a 400
7. il server invia la nuova finestra in un ACK
	- il client fa scorrere la finestra togliendo da sinistra i segmenti per i quali ha ricevuto ACK e aggiungendo a destra fino a raggiungere 400
8. il server consuma altri 200 byte
	- la finestra passa a 600
	- invia un ACK al client che allargherà verso destra la finestra di invio per farla combaciare

