- affinché i dispositivi possano comunicare non è sufficiente assicurare i collegamenti tra le reti
- è necessario utilizzare anche la parte software che faccia rispettare i protocolli

> [!info] Protocollo
> - definisce le regole che il mittente e il destinatario, così come i sistemi intermedi coinvolti devono rispettare per poter comunicare
> - alle volte sono necessari più protocolli, per suddividere i compiti tra più layer, si tratta del **layering di protocolli**


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
