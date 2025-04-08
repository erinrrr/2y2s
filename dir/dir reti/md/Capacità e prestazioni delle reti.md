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