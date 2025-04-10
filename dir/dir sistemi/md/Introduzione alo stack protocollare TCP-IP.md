- affinché i dispositivi possano comunicare non è sufficiente assicurare i collegamenti tra le reti
- è necessario utilizzare anche la parte software che faccia rispettare i protocolli

> [!info] Protocollo
> - definisce le regole che il mittente e il destinatario, così come i sistemi intermedi coinvolti devono rispettare per poter comunicare
> - alle volte sono necessari più protocolli, per suddividere i compiti tra più layer, si tratta del **layering di protocolli**
>   
>   il server è un programma che usa il protocollo




esempio: due amiche vogliono parlarsi per posta e assicurarsi che nessun altro possa leggere i loro messaggi, dovranno usare anche dei corrieri per spedire le lettere
![[dir/dir sistemi/asset/file 18.png]]
ad ogni livello sembrerà di parlare soltanto con il livello simmetrico dall’altro lato, diciamo quindi che non conoscono tutta la struttura che esiste oltre a loro


## Strutturazione a Livelli
- si basa sul suddividere un compito complesso in più compiti semplici
- **modularizzazione**, indipendenza dei livelli:
	- un modulo (livello) può essere considerato come:
		- una black box con opportuni input e output
		- di cui non ci interessa _come_ i dati vengono trasformati internamente
		- ci interessa solo che l'input venga correttamente convertito nell'output
- se due macchine forniscono lo stesso output dato il medesimo input, allora possono essere considerate _equivalenti_
- separazione tra servizi e implementazione:
	- un livello usa servizi dal livello inferiore e offre servizi al livello superiore
	- indipendentemente dall'implementazione

### Principi della strutturazione a Livelli
se richiesta una comunicazione bidirezionale:
- ogni livello deve essere capace di effettuare il compito in entrambe le direzioni
- gli oggetti in input/output devono essere identici:
	- ciò che esce da un livello su un lato deve corrispondere a ciò che entra nel livello corrispondente sull’altro lato


> [!NOTE] collegamento logico
>- livelli direttamente collegati tra loro:
>	- ogni livello di una macchina comunica con il pari livello dell’altra macchina 
>- il protocollo implementato a ciascun livello specifica:
>	- una comunicazione diretta fra i pari livelli delle due parti
>
> ![[dir/dir sistemi/asset/file 19.png]]
> 
> ![[dir/dir sistemi/asset/file 22.png]]


## Stack Protocollare TCP - IP
- gerarchia di protocolli costituita da moduli interagenti
- ogni livello fornisce funzionalità specifiche_
	1. applicazione (lvl 5, il più vicino all'utente)
	2. trasporto
	3. rete
	4. collegamento
	5. fisico (lvl 1)

**applicazione**:
- dove si trovano le applicazioni di rete
- protocolli principali: HTTP, SMTP, DNS, FTP
- i pacchetti sono detti _messaggi_
**trasporto**:
- trasferimento dei messaggi dell’applicazione tra il client e il server
- protocolli principali:
	- TCP: 
		- più sicuro, ma meno veloce
		- i pacchetti sono detti _segmenti_
	- UDP:
		- meno sicuro, ma più veloce
		- i pacchetti sono detti _datagrammi utente_
**rete**:
- si occupa dell'instradamento dei segmenti dall'origine alla destinazione
- principalmente il protocollo IP e altri di instradamento
- i pacchetti sono detti _datagrammi_
**link**:
- si occupa del trasmettere datagrammi da un nodo a quello successivo sul percorso
- lungo un percorso completo, un datagramma può attraversare protocolli di collegamento diversi
- protocolli principali: Ethernet, WiFi, PPP
- i pacchetti sono detti _frame_
**fisico**:
- trasferimento dei singoli bit

- i livello fisico e link: prevalentemente hardware
- i livelli trasporto e applicazione: software
- il livello rete: neutro

##### Comunicazione in una internet
- i dispositivi che necessitano di comunicare implementano tutto lo stack
- i router non hanno bisogno dei livelli più alti dato che devono solo gestire la parte meno vicina all'utente
- quindi grazie al layering tali sistemi implementano solo i livelli necessari
	- svantaggi: a volte necessario scambio informazioni tra livelli non adiacenti non rispettando principio della stratificazione

![[dir/dir sistemi/asset/file 20.png]]

 
> [!tip]- Gerarchia dei protocolli (ricapitolo + osservazione)
> - la rete è organizzata come una pila di layer l'uno sull'altro
> - ogni layer offre servizi ai layer superiori, nascondendo i dettagli dell'implementazione
> - il layer $N$ di un computer è in comunicazione con il layer $N$ di un altro computer
> 	- le regole e le convenzioni usate in questa comunicazione sono note come protocolli dello strato $N$
> 	- i dati non sono trasferiti direttamente dal layer $N$ di un computer al layer $N$ di un altro computer
> - le entità che formano i layer sono detti peer
> - i peer comunicano usando il protocollo


> [!info] servizi vs protocolli
> **servizio**:
> - insieme di primitive che un layer offre a quello superiore
> - definisce quali operazioni il layer è in grado di offrire
> - è correlato all'interfaccia tra due layer dove:
> 	- quello inferiore è il provider del servizio
> 	- quello superiore è l'utente
> 
> **protocollo**:
> - insieme di regole che controllano il formato e il significato dei pacchetti
> - in generale i messaggi scambiati tra le entità peer all'interno di un layer
> 
> ![[dir/dir sistemi/asset/file 21.png]]
> 
> ![[dir/dir sistemi/asset/file 23.png]]



## Incapsulamento e decapsulamento
**incapsulamento**:
- prende il pacchetto dal livello superiore gli aggiunge un header
- _header_ contiene informazione necessari per la comunicazione peer-to-peer
- passaggi:
	- messaggio: dati grezzi dell'applicazione (senza header)
	- segmento: header trasporto + messaggio
	- datagramma: header rete + segmento
	- frame: header collegamento + datagramma
**decapsulamento**:
- ad ogni livello il destinatario:
	- rimuove il rispettivo header
	- capisce le informazioni
	- passa su al livello superiore

il router:
- decapsula il frame ricevuto da un link
- legge le informazioni del datagramma (livello rete)
- Incapsula di nuovo in un nuovo frame per l'altro link

aggiungendo gli header permettiamo ad ogni livello di riconoscere su quali pacchetti devono agire, ad esempio:
- se il livello di trasporto aggiunge come header $4$
- gli altri livelli di trasporto sanno che dovranno leggere soltanto i pacchetti con header $4$
![[dir/dir sistemi/asset/file 24.png]]

## Multiplexing e Demultiplexing
- lo stack protocollare TCP/IP prevede l'uso di più protocolli nello stesso livello è necessario eseguire:
	- il _multiplexing_ alla sorgente
		- protocollo che può incapsulare uno alla volta i pacchetti ottenuti da più protocolli a livello superiore
	- il _demultiplexing_ alla destinazione
		- protocollo che può decapsulare e consegnare i pacchetti a più protocolli a livello superiore
- è necessario un campo nel proprio header per identificare a quale protocollo appartengano i pacchetti incapsulati
- il modello TCP/IP prevede una comunicazione logica tra coppie di layer
- è quindi necessario avere un indirizzo sorgente e un indirizzo destinazione ad ogni livello

esempio di demultiplexing:
 - i protocolli di trasporto ricevono pacchetti dall'applicazione
	 - TCP può incapsulare HTTP, FTP
	 - UDP può incapsulare DNS, SNMP
 - ogni pacchetto ha un header di trasporto che:
	 - specifica a quale protocollo applicativo appartiene il pacchetto
- quando il pacchetto arriva, il livello di trasporto legge l'header, capisce quindi a chi deve consegnarlo
- in sostanza smista i pacchetti ricevuti verso il giusto protocollo applicativo

![[dir/dir sistemi/asset/file 25.png]]




#### Indirizzamento nel modello TCP-IP
- il modello TCP/IP prevede una comunicazione logica tra coppie di livelli, quindi
- è necessario avere un indirizzo sorgente e un indirizzo destinazione a ogni livello
![[dir/dir sistemi/asset/file 26.png]]


> [!info] Layering - vantaggi e svantaggi
> vantaggi:
> - la modularità che ci garantisce:
> 	- facilità di design
> 	- possibilità di modificare dei moduli in modo trasparente agli altri moduli
> svantaggi:
> - a volte necessario scambiare informazioni fra livelli non adiacenti
> 	- quindi non rispetta il principio della stratificazione


> [!tip]- Il modello OSI
> - presentato dalla ISO (International Organization for Standardization) ed è presentato come alternativa al TCP-IP
> - ha 7 livelli
> - è stato creato per permettere la comunicazione fra qualsiasi tipo di dispositivo
> - framework stratificato per il progetto di sistemi di rete che consentono la comunicazione fra qualsiasi tipo di dispositivo
> 
> ![[dir/dir sistemi/asset/file 27.png]]
> 
> nonostante ciò
> - molte infrastrutture usavano già il modello TCP-IP e fare un cambio avrebbe comportato troppi costi
> - per alcuni livelli è stata fornita poca documentazione e quindi non sono mai stati creati software appositi
> - non furono mai state dimostrate delle prestazioni così migliori da convincere le aziende a sostituire il TCP-IP

## Modello TCP/IP
a livello di applicazione
- troviamo i protocolli che gestiscono le applicazioni di rete
- questi protocolli permettono la comunicazione tra programmi applicativi
- troviamo anche:
	- principi delle applicazioni di rete
	- Web e HTTP
	- FTP (protocollo per il trasferimento file)
	- posta elettronica
		- SMTP, POP3, IMAP
	- DNS
	- applicazioni P2P
	- programmazione delle socket con TCP
	- programmazione delle socket con UDP
- offre servizio agli utenti finali
- la comunicazione è fornita per mezzo di un _connessione logica_:
	- i livelli applicazione nei due lati della comunicazione agiscono come se esistesse un collegamento diretto attraverso il quale poter inviare e ricevere messaggi
	- i due utenti possono immaginare che esista un canale logico bidirezionale
	- la comunicazione reale avviene attraverso più layer e più dispositivi e vari canali fisici

il livello applicazione è l’unico che fornisce servizi agli utenti di Internet, la sua flessibilità consente di aggiungere nuovi protocolli facilmente, infatti ora ve ne sono molti, e possiamo distinguerli in:
- **protocolli standard**:
	- esistono diversi protocolli di livello applicazione che sono standardizzati e documentati dagli enti responsabili della gestione di Internet
	- ogni protocollo standard è costituito da una coppia di programmi che interagiscono con l'utente ed il livello di trasporto per fornire uno specifico servizio
	- ad esempio: applicazioni web con il protocollo di comunicazione HTTP
- **protocolli non standard**
	- vi è la possibilità di creare applicazioni non standard scrivendo due programmi che forniscono servizi agli utenti facendo uso dei servizi di trasporto
	- non è necessario chiedere autorizzazioni dato che ogni azienda può sviluppare il proprio protocollo di livello applicazione per far comunicare i propri uffici sparsi nel mondo

## Creare un'applicazione di rete
dobbiamo scrivere programmi che:
- girano su sistemi terminali diversi
- comunicano attraverso la rete
per software in grado di funzionare su più macchine
- non occorre predisporre programmi per i dispositivi del nucleo della rete
- i programmi applicativi sono indipendenti dalla tecnologia che c'è sotto

dobbiamo chiederci:
- che tipo di architettura si vuole creare:
	- client-server
	- peer-to-peer
- come comunicano i processi dell'applicazione?
- che tipo di servizi di rete richiede l'applicazione:
	- necessitiamo di più banda
	- o necessitiamo di più sicurezza
- i due programmi devono essere in grado di richiedere ed offrire servizi o ciascuno ha un solo compito?

paradigma **client-server**:
- sono due entità totalmente differenti:
	- non è possibile eseguire un client come programma server e viceversa
- client:
	- richiede il servizio
	- possono esserci più client che richiedono servizi
	- in esecuzione solo quando è necessario il servizio
- server:
	- fornitore di servizi
	- numero limitato di processi server, pronti a offrire uno specifico servizio
	- sempre in esecuzione, in attesa di richieste dal client
- svantaggi:
	- carico di comunicazione concentrato sul server, deve essere molto potente
	- necessità di server farm per creare un potente server virtuale, che portano a:
		- costi di gestione elevati per offrire il servizio
- applicazioni tipiche:
	- WWW
	- posta elettronica
	- FTP
	- SSH