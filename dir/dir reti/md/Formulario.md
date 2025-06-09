### Delay
- **Transmission delay**: $R_{t} = \dfrac{L}{R}$
	- $L$ = lunghezza del pacchetto (in bit)
	- $R$ = rate del collegamento (in bps)
- **intensità del traffico**: $\dfrac{La}{R}$
	- come transmission delay, ma si usa quando si hanno $a$ pacchetti, si considera il traffico di più pacchetti contemporaneamente
	- quanto carico metti sul link considerando a pacchetti contemporaneamente
	- se tende a 1: il link è molto carico
	- $a$ = la frequenza, quante richieste o pacchetti arrivano in un secondo (req/sec)
	- sulla LAN, $L$ = quanto è grande una richiesta (Mb/req)
- **Propagation delay**: $R_{p} = \dfrac{d}{s}$
	- $d=$ lunghezza del collegamento
	- $s=$ velocità di propagazione del collegamento ($\sim 2\cdot10^8 m/\sec$)
- **tempo di risposta**: $2RTT + \text{tempo di trasmissione}$
- **total delay**: $\text{LAN delay}+\text{access delay}+\text{Internet delay}$
	- indica il tempo di risposta totale
	- $\text{Internet delay}$ = $2sec$
- **prodotto banda-ritardo (Bandwidth-Delay Product)**: [^1]
	- $BDP = R \cdot d_{prop}$ 
		- se ci interessa il ritardo di propagazione
	- $BDP = R \cdot RTT$
		- se ci interessa la quantità di dati "in volo" durante un’intera andata e ritorno (RTT)
	- $R$ = capacità del link (bps)
	- $d_{prop}$ = propagation delay
	- misura la quantità massima di dati che può "risiedere" sul collegamento in un dato momento (cioè "in volo")
    - si interpreta come: quanti bit possono essere presenti nel collegamento mentre un bit viaggia dalla sorgente alla destinazione
    - utile per:
	    - valutare quanto buffer può essere necessario
	    - capire quanta finestra di trasmissione serve per mantenere pieno il canale (in TCP)
[^2]
- Il ritardo è dato quindi da: $d_{nodal}​=d_{proc}​+d_{queue}​+d_{trans}​+d_{prop}​$
	- $d_{trans}$​ - Ritardo di trasmissione che è significativo sui collegamenti a bassa velocità
	- $d_{prop}​$ - Ritardo di propagazione che va da pochi microsecondi a centinaia di millisecondi
	- $d_{proc}$​ - Ritardo di elaborazione, in genere pochi microsecondi o meno
	- $d_{queue}$​ - Ritardo di accodamento che dipende dalla congestione della rete in quel momento

## RR format
- **RR format**: (name, value, type, TTL)
	- type A:
		- map: Hostname ➔ IP address
		- name = hostname
		- value = ip Address
	- type CNAME:
		- map: Alias ➔ Canonical Name
		- name = alias
		- value = Canonic NAME
	- type NS:
		- map: Domain name ➔ Name Server
		- name = dominio
		- value = Nome del Server authoritative di quel dominio
	- type MX:
		- map: Alias ➔ mail server canonical name
		- name = _name_
		- value = nome canonico del server di posta associato a _name_


## Link

- **link usage**: 
	- $\text{Utilizzo} = \dfrac{\text{Throughput effettivo}}{\text{Capacità del link}} \times 100\%$
		- indica quanto di un collegamento viene effettivamente usato rispetto alla sua capacità totale
	- $\text{Throughput per flusso} = \dfrac{R}{N}$
		- $R$ = capacità totale del link condiviso
		- $N$ = numero di flussi attivi
		- quando un collegamento è condiviso tra più flussi, il throughput per ogni flusso si calcola dividendo la capacità per il numero di flussi attivi
	- [[dir/dir reti/esercizi/esercizi#Esercizio 9|esempio]]

[^1]: ![[dir/dir reti/asset/file 36.png]]
[^2]: il **rate** è una misura potenziale mentre il **throughput** è una misura effettiva

