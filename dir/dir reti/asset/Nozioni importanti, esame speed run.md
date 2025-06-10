## cose che palesemente non ricorderò
- il **throughput** ci indica quanto velocemente riusciamo a inviare i dati. non é un’indicazione basata sulla tecnologia utilizzata come il bitrate o larghezza di banda, è quanto riusciamo ad inviare _realmente_ in quel momento

- differenza trasmissione-propagazione:
	- la trasmissione è il tempo necessario a un nodo per far uscire il pacchetto quindi varia in base al rate del link e alla lunghezza del pacchetto.
	- la propagazione invece indica il tempo che impiega un bit per percorrere il percorso una volta fuori dal nodo, varia quindi in base alla distanza

- DNS
	- Organizziamo i server in 3 livelli:
	
		1. Root
		2. Top-level Domain (TLD)
			1. Authoritative
Infine troviamo i server locali delle applicazioni che sono quelli che fanno le richieste.

![[file 54.png]]

>Un **proxy riceve le richieste dal tuo computer, le inoltra a Internet**, riceve le risposte e te le rimanda indietro[^4]
>- se chiedi una pagina web, il tuo computer → **chiede al proxy** → il proxy va su Internet → prende la pagina → te la restituisce

server authoritative = server di competenza

Una **query** è una richiesta che un client (come un programma o un utente) invia a un server o a un database per ottenere dati o una risposta [^5]

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


### FTP
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

### Posta Elettronica
Vediamo i principali componenti utilizzati per la comunicazione mail:
![[file 61.png]]
- User Agent: app/programma usato dall'utente per scrivere, leggere e inviare messaggi di posta elettronica (Gmail)
- Message Transfer Agent: Usato per trasferire il messaggio attraverso Internet (Postfix, Sendmail, Microsoft Exchange Server)
- Message Access Agent: Usato per leggere la mail in arrivo (IMAP o POP3 sono i protocolli più usati, il software può essere lo stesso User Agent (es. Outlook che usa IMAP per leggere la posta)
	(Gmail dal telefono, l’app (UA) usa IMAP (MAA) per recuperare le email dal server di Google)

##### User Agent
- viene attivato da un timer o dall’utente e informa quest’ultimo di nuove mail in arrivo
- in messaggi in uscita o in arrivo vengono memorizzati sul server e quelli da inviare passano per il Mail Transfer Agent
- 