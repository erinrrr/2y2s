Internet è composto da:
- **reti di accesso**:
	- ossia il collegamento da un sistema fino all'_edge router_
		- il primo router sul percorso dal sistema di origine a un qualsiasi sistema di destinazione 
	- ci forniscono accesso alla backbone
	- possono essere
		- residenziali: punto-punto (modem dial-up, DSL), modem via cavo (fibra ottica)
		- aziendale: LAN
		- wireless: tramite access point LAN o cellulare

rappresentate in blu nell'immagine che segue
![[dir/dir reti/asset/file 17.png]]
- **backbone Internet**:
	- insieme di router e collegamenti ad alta velocità che formano l'ossatura principale di Internet
	- collegano tra loro gli edge router
	- costituiti da grandi provider (ISP tier 1)
	- interconnesse tra loro

rappresentate in blu:
![[dir/dir reti/asset/file 18.png]]

## Struttura di Internet
- struttura gerarchica, basata su vari livelli di ISP
- al centro troviamo gli _ISP di livello 1_
	- si occupano di comunicazione a livello nazionale / internazionale
	- direttamente connessi tra di loro, comunicano tra di loro come pari
- _ISP di livello 2_:
	- nazionali o distrettuali
	- si può connettere solo ad alcuni ISP di livello 1, in quanto clienti degli ISP di livello 1 che gli forniscono connettività sul resto della rete
	- possono anche connettersi ad altri ISP di livello 2, e quando sono interconnessi vengono detti di pari grado (peer)
- infine troviamo _ISP di livello 3_ e _ISP locali_:
	- last-hop network, ultima rete che i dati attraversano prima di raggiungere il sistema di destinazione, le più vicine ai sistemi terminali
	- sono clienti degli ISP di livello superiore che li collegano all'intera internet
![[dir/dir reti/asset/file 19.png]]

- un pacchetto passa attraverso molte reti prima di arrivare a destinazione
- il problema di scegliere su che percorso mandarlo è detto **routing**

![[dir/dir reti/asset/file 20.png]]


## Capacità e prestazioni
- la velocità della rete o di un collegamento è un concetto che coinvolge vari fattori, nel caso di una rete a commutazione di pacchetto le prestazioni si misurano in termini di:
	- ampiezza di banda
	- bit rate
	- throughput
	- latenza (ritardi)
	- perdita di pacchetti

#### Bandwidth e bit rate
**bandwidth** è un termine che fa riferimento a due concetti diversi ma legati:
- _caratterizzazione del canale o del sistema trasmissivo_:
	- si misura in hertz
	- rappresenta la larghezza dell'intervallo di frequenze utilizzato dal sistema trasmissivo, che consentono di trasmettere senza danneggiare il segnale
	- maggiore è l'ampiezza di banda, maggiore è la quantità di informazioni che può essere veicolata attraverso il mezzo trasmissivo
- _caratterizzazione di un collegamento_:
	- si misura in bit al secondo (bps)
	- è la quantità di bit al secondo che un link garantisce di trasmettere

il **bit rate**:
- dipende sia dalla banda
- che dalla tecnica di trasmissione o formato di modulazione digitale utilizzato
- è proporzionale alla banda in hertz
- fornisce un indicazione della capacità della rete di trasferire dati

#### Throughput
- indica quanto velocemente riusciamo _effettivamente_ a inviare i dati
- numero di bit al secondo che passano attraverso un punto della rete
 - un link può avere un rate di $B$ bps di ma in quel momento possiamo inviare soltanto $T$ bps con $T \le B$

il rate è la _misura potenziale_ della velocità di un link
il throughput è una _misura effettiva_ della velocità di un link


- un pacchetto passa in molti nodi con link che hanno throughput diversi
- quindi la velocità tra nodo iniziale e quello di destinazione è data dal minore throughput dei link
- questo effetto è detto _collo di bottiglia_: collegamento su un percorso punto-punto che vincola un throughput end to end

![[dir/dir reti/asset/file 21.png]]
nel primo caso avremo $R_{S}$​ mentre nel secondo $R_{C}$


- solitamente i dati passano attraverso due reti di accesso e la dorsale
- e viene considerato solo il minimo tra i due link di accesso che collegano la sorgente e la destinazione alla dorsale in quanto la dorsale ha un throughput molto alto
- in generale in un percorso con $n$ link in serie abbiamo che il throughput è il minimo dei vari link $min(T_{1},T_{2},\dots,T_{n})$
- potremmo avere che il link tra due router raccoglie il flusso da varie sorgenti e/o lo distribuisce a varie destinazioni, invece di essere dedicato
- il rate del link tra i due router è condiviso tra i flussi di dati


![[dir/dir reti/asset/file 22.png]]
il throughput end to end è 200 kbps in quanto il link è condiviso

## Latenze e perdite
- è il tempo che impiega un pacchetto ad arrivare completamente a destinazione dal momento in cui il primo bit parte dalla sorgente
- nella commutazione di pacchetto, quando il tasso di arrivo dei pacchetti eccede la capacità del collegamento di trasmetterli si crea un accumulo
- quindi i pacchetti si accodano nei buffer dei router in attesa del proprio turno
	- ne deriva che si genera un ritardo
- se invece non c'è più spazio nei buffer per accogliere altri pacchetti, questi verranno scartati
![[dir/dir reti/asset/file 23.png]]

in totale sono 4 le cause di ritardo per i pacchetti:
1. **processing delay**:
	- tempo per processare il pacchetto:
		- controllo errori sui bit
		- determinazione del canale di uscita
		- trasferimento da porta in input a porta in output
2. **queueing delay**:
	- tempo di attesa in coda (input/output) prima della trasmissione
	- dipende dal livello di congestione del router
	- può variare da pacchetto a pacchetto
![[dir/dir reti/asset/file 26.png]]
3. **transmission delay**
	- tempo richiesto per immettere tutti i bit del pacchetto sul collegamento
	- se il primo viiene trasmesso a tempo $t_{1}$ e l'ultimo bit a tempo $t_{2}$ allora il ritardo di trasmissione è $t_{2}-t_{1}$
	- formula: $\text{Transmission delay} = \dfrac{L}{R}$
		- $L$ = lunghezza del pacchetto (in bit)
		- $R$ = Rate del collegamento (in bps)
![[dir/dir reti/asset/file 24.png]]
4. **propagation delay**:
	- tempo che un bit impiega per propagarsi fisicamente sul collegamento, dal punto $A$ al punto $B$
	- Formula: $\text{Propagation delay} = \dfrac{d}{s}$
		- $d=$ lunghezza del collegamento
		- $s=$ velocità di propagazione del collegamento solitamente quella della luce ($\sim 2\cdot10^8 m/\sec$)
![[dir/dir reti/asset/file 25.png]]


> [!info] Differenza tra ritardo di trasmissione e propagazione
> - trasmissione:
> 	- tempo necessario a un nodo per far uscire il pacchetto quindi
> 	- varia in base al rate del link e alla lunghezza del pacchetto
> - propagazione:
> 	- tempo che impiega un bit per propagarsi da un nodo all'altro sul mezzo trasmissivo una volta fuori dal nodo
> 	- varia in vase alla distanza

quindi il **ritardo di nodo** + dato da:
- $d_{\text{nodal}} = d_{\text{proc}} + d_{\text{queue}} + d_{\text{trans}} + d_{\text{prop}}$
dove:
- $d_{\text{trans}}$​: transmission delay
	- significativo sui collegamenti a bassa velocità
- $d_{\text{prop}}$​: propagation delay
	- da pochi microsecondi a centinaia di millisecondi
- $d_{\text{proc}}$​: processing delay
	- in genere pochi microsecondi o meno
- $d_{\text{queue}}$​: queueing delay
	- dipende dalla congestione della rete in quel momento

### Ritardo di accodamento
- può variare da pacchetto a pacchetto, anche perché non è detto che seguano tutti lo stesso percorso
- su un percorso potrebbe crearsi del traffico in qualsiasi momento
- possiamo fare solo stime, per questo si fa uso di misure statistiche
- dipende:
	- dal tasso di arrivo
	- dal rate
	- dalla lunghezza dei pacchetti

indichiamo con:
- $R$ = Rate di trasmissione in bps
- $L$ = Lunghezza del pacchetto in bit
- $a$ = tasso medio di arrivo dei pacchetti pkt/s

l'intensità del traffico la indichiamo come:
$$
\frac{La}{R} \rightarrow \begin{cases}
\sim 0: \text{ritardo basso} \\
\rightarrow 1: \text{ritardo consistente, vicini alla capacità massima} \\
> 1: \text{più lavoro di quello gestibile, ritardo medio } \infty
\end{cases}
$$
![[dir/dir reti/asset/file 27.png]]

- più l’intensità si avvicina a 1 più rapidamente cresce il ritardo medio di accodamento

##### Perdita di pacchetti (packet loss)
- se un buffer (coda) ha capacità finita, quando il pacchetto trova la coda piena viene scartato
- il pacchetto perso può essere ritrasmesso:
	- dal nodo precedente
	- dal sistema terminale che lo ha generato
	- o non essere trasmesso affatto

![[dir/dir reti/asset/file 28.png]]

##### Ritardi e percorsi in Internet
`traceroute` e `tracert` sono algoritmi diagnostici che forniscono:
- una misura del ritardo dalla sorgente a tutti i router lungo il percorso Internet punto-punto verso la destinazione
- dettagli su tutti i router che attraversiamo

funzionamento:
- vengono invianti gruppi di 3 pacchetti, ogni gruppo con tempo di vita incrementale, che raggiungeranno il router $i$ sul percorso verso la destinazione
- il router $i$ restituirà i pacchetti al mittente
- il mittente calcola l'intervallo tra trasmissione e risposta
- output composto da:
	- numero del router nel percorso
	- nome del router
	- indirizzo del router
	- tempo andata e ritorno 1 pacchetto
	- tempo andata e ritorno 2 pacchetto
	- tempo andata e ritorno 3 pacchetto
il tempo di andata e ritorno (round trip time - RTT) include i 4 delay

![[dir/dir reti/asset/file 29.png]]

- può accadere che il RTT del router $n$ sia maggiore del RTT del router $n+1$ a causa dei ritardi di accodamento
- se la sorgente non riceve risposta da un router intermedio, pone un asterico al posto del tempo di RTT

##### Prodotto rate * ritardo
- massimo numero di bit che possono riempire il collegamento
- ad esempio se abbiamo un link 1 bps e un ritardo di 5 secondi avremo non  più di 5 bit contemporaneamente sul link

![[dir/dir reti/asset/file 30.png]]

- il primo bit entra dopo 1 secondo
- e dopo 5 secondi sarà arrivato alla fine
- in questi 5 secondi sono entrati altri bit tutti a differenza di un secondo per via del bit rate e si saranno mossi tutti ogni secondo

possiamo pensare al link tra due punti come a un tubo
- la sezione trasversale del tubo rappresenta il rate
- la lunghezza rappresenta il ritardo
- quindi il volume del tubo definisce il prodotto rate-ritardo

![[dir/dir reti/asset/file 31.png]]