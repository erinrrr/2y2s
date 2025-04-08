## Composizione della rete
- una rete è composta da dispositivi in grado di scambiarsi informazione quali _sistemi terminali_ e _dispositivi di interconnessione_
- i sistemi terminali possono essere di due tipi:
	- **host**:
		- solitamente di proprietà degli utenti
		- dedicata ad eseguire applicazioni
		- ad esempio computer desktop, portatile, cellulare $\dots$
	- **server**:
		- tipicamente un computer ad alte prestazioni
		- destinato a eseguire programmi che forniscono servizio a diverse applicazioni utente
		- esempi di applicazioni utente: posta elettronica, web $\dots$
		- sono gestiti da amministratori di sistema
		- gestiscono e condividono risorse di rete, come stampanti
- dispositivi di interconnessione:
	- rigenerano/modificano il segnale che ricevono e si distinguono in:
		- **router**: dispositivi che collegano una rete ad altre reti
		- **switch**: collegano più sistemi terminali a livello locale
		- **modem**: trasformano i segnali che ricevono in varie codifiche di dati
			- ad esempio ricevono il segnale telefonico dall’esterno e lo convertono in un segnale digitale con il quale usare anche internet

![[dir/dir reti/asset/file.png]]

### Collegamenti
- i dispositivi di rete vengono collegati utilizzati mezzi trasmissivi cablati o wireless, detti **link**
	- collegamenti cablati:
		- rame
		- fibra ottica
	- collegamenti wireless:
		- onde elettromagnetiche
		- satellite

mezzi trasmissivi _cablati_:
- **bit**: 
	- viaggia da un sistema terminale ad un altro
	- passa per una serie di coppie trasmittente-ricevente
- **mezzo fisico**:
	- ciò che si trova tra il trasmittente e il ricevente
	- serve a trasportare i segnali
- **mezzi guidati**: 
	- _filo di rame_
	- _twister pair (TP)_ anche detto doppino intrecciato
		- due fili di rame distinti come il cavo telefonico
			- oppure
		- intreccio di quattro coppie di fili come l'Ethernet
	- _cavo coassiale_
		- due conduttori din rame concentrici
		- bidirezionale
		- usato per cablatura di reti locali ad alta velocità
		- resistente alle interferenze
		- soppiantato dalla fibra ottica
	- _fibra ottica_
		- mezzo sottile e flessibile che conduce impulsi di luce (ogni impulso rappresenta un bit)
		- alta frequenza trasmissiva: velocità punto-punto da 10 a 100 Gbps
		- basso tasso di errore: distanze maggiori tra i ripetitori, immunità all’interferenza elettromagnetica
		- mezzo prevalente delle dorsali internet
	- essendo tutti questi cablati il segnale è guidato da una destinazione all'altra

mezzi trasmissivi _wireless_:
-  mezzi a onda libera:
	- i segnali si propagano nell'atmosfera o nello spazio esterno
	- trasportano segnali nello spettro elettromagnetico
	- non richiedono l'installazione fisica di cavi
	- bidirezionali
	- effetti derivati dall'ambiente di propagazione
		- riflessione
		- ostruzione da parte di ostacoli
		- interferenza
- tipi di _canali radio_:
	- microonde terrestri: canali fino a 45 Mbps
	- LAN: come ad esempio WiFi-IEEE802.11; 11 Mbps, 54 Mbps
	- wide-area: reti cellulari come 3/4/5G; circa 1Mbps
	- satellitari: canali fino a 45 Mbps ma con ritardi punto a punto di 270ms, geostazionari/a bassa quota


## Classificazione delle reti
| Scale    | Type                                   | Example                   |
| -------- | -------------------------------------- | ------------------------- |
| Vicinity | PAN (Personal Area Network)            | Bluetooth (e.g., headset) |
| Building | LAN (Local Area Network)               | WiFi, Ethernet            |
| City     | MAN (Metropolitan Area Network)        | Cable, DSL                |
| Country  | WAN (Wide Area Network)                | Large ISP                 |
| Planet   | The Internet (network of all networks) | The Internet!             |

#### LAN
- **rete privata**
- collega i sistemi terminali in un singolo ufficio (azienda, università)
- ogni sistema terminale nella LAN ha un indirizzo che lo identifica univocamente nella rete
- non specifica un minimo o un massimo numero di dispositivi
- nata con lo scopo di condividere risorse tra sistemi terminali che ne facevano parte, oggi si connette ad altre LAN o WAN per consentire comunicazione su larga scala

esistono due tipi di LAN:
- _LAN a cavo condiviso (broadcast)_:
	- il pacchetto inviato da un dispositivo, viene ricevuto da tutti gli altri
	- solo il destinatario elabora il pacchetto, mentre gli altri lo ignorano
	- può trasmettere un dispositivo alla volta per evitare delle collisioni
![[dir/dir reti/asset/file 1.png]]
- _Lan con switch di interconnessione_
	- i dispositivi non sono collegati tra di loro, ma ad uno **switch** che:
		- riconosce l'indirizzo di destinazione, e invia il pacchetto solo al destinatario senza inviarlo agli altri dispositivi
		- riduce il traffici nella LAN, consentendo a più coppie di dispositivi di comunicare contemporaneamente tra di loro (sorgenti e destinazioni diverse)
![[dir/dir reti/asset/file 2.png]]


#### WAN
- può servire una città. una regione o una nazione, è una **rete geografica**
- interconnette dispositivi di connessione quali:
	- switch
	- router
	- modem
- gestita da un operatore di telecomunicazioni, **ISP** (Internet Service Provider), che fornisce i suoi servizi alle organizzazioni che ne fanno uso

esistono due tipi di WAN:
- _WAN punto-punto_
	- collega due dispositivi terminali tramite un mezzo trasmissivo (cavo o wireless)
![[dir/dir reti/asset/file 3.png]]
- _WAN a commutazione_
	- rete con più di due punti di terminazione
	- usata nelle dorsali di Internet
![[dir/dir reti/asset/file 4.png]]

### Internetwork e rete eterogenea
- composta da due LAN e una WAN punto-punto
- le LAN e le WAN sono in genere connesse fra di loro per formare una internetwork (internet)

> [!tip]+ internet vs Internet
> - internet in generale può significare un insieme di reti interconnesse 
> - Internet è l'internet più famosa composta da migliaia di reti interconesse

esempio - un’azienda vuole mettere in comunicazione due uffici in diverse città, in ciascun ufficio esiste una LAN e per mettere in comunicazione queste due l’azienda acquista una WAN punto-punto da un ISP realizzando quindi una internet privata
![[dir/dir reti/asset/file 5.png]]
- i router instradano i pacchetti da una LAN all'altra

la rete eterogenea è una rete più complessa composta da:
- quattro WAN
- tre LAN
![[dir/dir reti/asset/file 6.png]]


> [!info]- WAN: la rete GARR
> - la rete GARR interconnette ad altissima velocità università, biblioteche, centri di ricerca $\dots$
> - l'infrastruttura è in fibra ottica  e si estende su circa 15.000km tra collegamenti di dorsale e di accessi
> - la capacità delle singole tratte della dorsale arriva a 100Gbps
> - nei collegamenti di accesso la capacità può arrivare a 40Gbps
> ![[dir/dir reti/asset/file 7.png]]


## Commutazione (switching)
- una internet (internetwork) è una combinazione di link e dispositivi capaci di scambiarsi informazioni
- i sistemi terminali appartenenti alla rete comunicano tra di loro per mezzo di dispositivi come **switch**, **router** che si trovano nel percorso tra i sistemi sorgente e destinazione
- esistono due tipi di reti basate su switch
	- reti a commutazione di circuito - **circuit-switched network**
	- reti a commutazione di pacchetto - **packet-switched network**

#### Circuited-switched network
- tra due dispositivi è sempre disponibile un collegamento dedicato detto _circuito_
- nonostante esistano più percorsi possibili viene usato lo stesso per tutta la trasmissione
- durante la comunicazione, vengono riservate risorse che rimangono occupate fino alla fine
- comunicazioni diverse tra gli stessi dispositivi possono usare circuiti stabiliti su percorsi diversi

![[dir/dir reti/asset/file 8.png]]

esempio: una telefonata sulla rete fissa ➔ quando chiami qualcuno, viene creato un collegamento diretto che rimane per tutta la chiamata	

![[dir/dir reti/asset/file 9.png]]
_efficienza_:
- se 4 persone da un lato comunicano cone le 4 persone dall'altro lato:
	- capacità completamente utilizzata
- se ima persona da un lato è collegata con una dall'altro lato:
	- solo $\frac{1}{4}$ della capacità utilizzata
	- inefficiente in quanto sottoutilizzata

**risorse di rete**:
- le risorse di rete come la bandwidth, sono suddivise in pezzi per non farle sovrapporre
- ciascun pezzo viene allocato ai vari collegamenti
- le risorse se non utilizzate rimangono in attive (non c'è condivisione)


suddivisione della banda:
- divisione di frequenza: **FDM** (Frequency Division Multiplexing)
- divisione di tempo: **TDM** (Time Division Multiplexing)

![[dir/dir reti/asset/file 10.png]]

#### Packet-switched network (store and forward)
- la comunicazione fra i due lati avviene trasmettendo blocchi di dati, i **pacchetti**, i messaggi vengono suddivisi in blocchi di informazioni con una lunghezza massima
- non si ha una comunicazione continua
- non viene riservata alcuna risorsa per la comunicazione
- gli switch memorizzano e inoltrano i pacchetti provenienti dai vari dispositivi
- non c'è un percorso specifico per il trasferimento di dati tra due dispositivi
- i pacchetti possono seguire anche percorsi diversi, e arrivare in un tempo diverso, bisogna quindi numerarli per ricostruire il messaggio una volta arrivato

![[dir/dir reti/asset/file 12.png]]

esempio: il sistema postale
![[dir/dir reti/asset/file 11.png]]
_efficienza_:
- se solo due dispositivi, uno per lato:
	- comunicano tra di loro senza alcun tempo di attesa per i pacchetti
- più dispositivi comunicano tra di loro, e la banda non è tale da inviare tutti i pacchetti che arrivano, il router deve memorizzarli in una coda:
	- i pacchetti possono avere qualche ritardo
	- ma più utenti possono usare la rete

più precisamente:
- la coda nei router serve a memorizzare i pacchetti in caso di traffico sulla linea, in questo modo il router può aspettare il momento adatto per inviare i pacchetti

## Internet
- l'internet che noi conosciamo è composta da migliaia di reti interconnesse
- è una rete a commutazione di pacchetto

rappresentazione concettuale:
![[dir/dir reti/asset/file 13.png]]

- le dorsali sono "lo scheletro" della rete alla quale si collegano i sottonodi


> [!info]- ARPANET
> una delle prime reti è ARPANET (Advanced Research Projects Agency Network)
> - nata per condividere materiale di ricerca fra alcune università
> - prima rete packet-switched ad implementare lo stack TCP/IP
>  ![[dir/dir reti/asset/file 14.png]]

### Accesso a Internet
- qualsiasi utente può far parte della internetwork di Internet
- tuttavia l'utente deve fisicamente collegarsi a un ISP
	- solitamente mediante una WAN punto-punto
- il collegamento che connette l'utente al primo router di Internet è detto _rete di accesso_

l'accesso può avvenire in diversi modi:
- accesso via rete telefonica
	- servizio dial-up (via modem)
	- servizio DSL (Digital Subscriber Line)
- accesso tramite reti wireless
	- Wi-Fi
	- cellulare
- collegamento diretto
	- aziende di grandi dimensioni possono diventare delle ISP locali affittando reti WAN dagli operatori

##### Accesso via rete telefonica
- è possibile accedere ad internet modificando la linea telefonica fra la sede del dispositivo che si vuole collegare e la centrale telefonica con una WAN punto-punto.

**servizio dial-up**:
- dobbiamo inserire sulla linea telefonica un modem che converte i dati digitali in analogici e viceversa
- servizio molto lento, non consente di parlare e navigare contemporaneamente
![[dir/dir reti/asset/file 15.png]]

**servizio DSL**
- tecnologia che supporta la comunicazione digitale ad alta velocità sulla linea telefonica
- divide il collegamento tra abitazione e ISP in tre bande di frequenza non sovrapposte
- veloce e ha la possibilità di usare il telefono mentre si naviga
![[dir/dir reti/asset/file 16.png]]

##### Accesso Wireless
- Wi-Fi
	- ci colleghiamo all’access point locale
	- il quale è collegato tramite Ethernet alla rete
	- raggio d'azione: pochi metri
	- `[ PC / Smartphone ] --(WiFi)--> [ Access Point (locale)] --(Ethernet)---> [ Router / Internet ]`
- Cellulare
	- si basa sugli access point della compagnia telefonica
		- più precisamente si connette a una Base Station della compagnia
	- raggio d'azione: decine di km
	- `[ Smartphone ] --(segnale radio)--> [(AP) Base Station ] --> [ Rete Operatore ] --> [ Internet ]`

##### Accesso tramite Ethernet
- nelle reti aziendali e universitarie per l'accesso a Internet si usa principalmente l'Ethernet
- col proprio terminale ci si collega direttamente allo switch tramite cavo
- lo switch è collegato ad un router istituzionale che a sua volta è connesso ai router della dorsale
- `[ PC ] --(cavo Ethernet)--> [ Switch ] --> [ Router ] --> [ Internet ]`
