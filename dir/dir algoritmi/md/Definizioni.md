*%% lezione 27/02/2025 %%*
# Algoritmo efficiente/inefficiente/ottimo
- un algoritmo si dice efficiente se è di ordine **polinomiale**, nella dimensione n dell'input:
	- $O(n^c)$ per una qualche costante $c$
- inefficiente se la sua complessità è di ordine **super-polinomiale**:
	- *esponenziale*: funzione di ordine $\theta(c^n)=2^{\theta(n)}$
	- *super-esponenziale*: funzione che cresce più velocemente di un esponenziale 
	esempi: $2^{\theta(n^2)}\ \text{oppure}\ 2^{\theta(n\log n)}$[^4]
	- *sub-esponenziale*: funzione che cresce più lentamente di un esponenziale $2^{o(n)}$
	esempi: $2^{\theta(\log^2 n)}=n^{\theta(\log n)},\ 2^{\theta(n)}, \ 2^{\theta(n^c)}\ c<1$
- ottimo se la complessità asintotica coincide con il limite inferiore del problema che risolve
	- In altre parole, se un algoritmo ha una complessità $O(g(n))$ (limitazione superiore) e il problema ha un limite inferiore $\Omega(f(n))$, allora l'algoritmo è detto **ottimo** se $f(n) = g(n)$. Ciò implica che l'algoritmo ha la miglior complessità possibile in termini di ordine di grandezza, poiché nessun altro algoritmo può risolvere il problema in modo asintoticamente più efficiente
	- complessità dell'algoritmo $\ne$ complessità del problema

#### Modi per calcolare le limitazioni inferiori 
- Dimensione dei dati:
	se un problema ha in input $n$ dati e richiede di esaminarli tutti, allora la limitazione inferiore della complessità è $\Omega(n)$
	(ricerca del max in una lista, basta scorrerla una volta), però non funziona con l'ordinamento in quanto non basta solo scorrere i dati ma vanno anche confrontati tra di loro
- Eventi contabili:
	se un problema richiede che un certo evento sia ripetuto almeno m volte abbiamo una limitazione inferiore pari a $\Omega(m)$
	(sorting basati su confronto, $n$ elem, numeri di confronti minimi da fare = $n\log n$ $\Rightarrow \Omega(n\log n)$)
	(se aggiungiamo dei vincoli il lower bound varia, magari usando algoritmi che non si basano sul confronto)[^1]
#### Problema intrattabile[^2]
- un problema si dice intrattabile se non può avere algoritmi efficienti
(esempio: stampare tutte le permutazioni di n elementi, lower bound: $2^{\Omega(n\log n)}$)

[^1]: Lower bound più forte prodotto di due matrici quadrate $n \times n$ è $\Omega(n^2)$

[^2]: Tesi di Church-Turing estesi: i modelli di calcolo realistici sono tra loro polinomialemente correlati. Il concetto di trattabilità è indipendente dalla macchina

[^3]: SCC : Strongly Connected Components


-----
# Grafi
[[Grafi]]
$G(V,E)$
- $|V|=n \ (\text{nodi})$
- $|E|= m\ (\text{archi})$, (arco = coppie  di nodi)

- grafi diretti: direzionali  $0 \le m \le n(n-1) = O(n^2)$
- grafi non diretti: i collegamenti sono bilaterali $0 \le m \le \frac{n(n-1)}{2} = O(n^2)$
<br>
- grafo sparso se $m= O(n)$
- grafo denso se $m = \Omega(n^2)$
	- un grafo si dice completo se ha tutti gli archi $m = \theta(n^2)$
	- si dice torneo se tra ogni coppia di nodi c'è esattamente un arco
- un grafo può essere né sparso né denso
- grafo vuoto: grafo senza archi
- ciclo dispari: grafo ciclico di dispari nodi
- ciclo negativo: ciclo diretto in un grafo la cui somma dei pesi degli archi che lo compongono è negativa

##### tipi di grafi
- _grafo planare_: grafi che possono essere disegnati senza che gli archi si intersechino
- _grafo parzialmente orientato_: contiene sia archi orientati che archi non orientati
- **grafo cactus**: grafo connesso non orientato in cui ogni arco appartiene al massimo ad un ciclo
- **grafo connesso**: in un grafo _non orientato_, esiste un cammino tra ogni coppia di nodi
- **grafo fortemente connesso (SCC)**: in un grafo _orientato_, esiste un **cammino diretto** tra ogni coppia di nodi in entrambe le direzioni
- **grafo debolmente connesso**: in un grafo _orientato_, ignorando la direzione degli archi diventa un **grafo connesso**
- **grafo pesato**: grafo in cui a ogni arco è associato un peso
- **minimo albero di copertura**: è un albero che include tutti i nodi del grafo con il **minimo peso** totale degli archi, connessi tra loro senza cicli

### tipi di archi e di nodi
- **archi in avanti** ovvero le frecce che da un antenato puntano ad un discendente
- **archi all’indietro** ovvero le frecce che da un discendente vanno ad un antenato
- **archi di attraversamento** ovvero quelle frecce che ci portano da un sottoalbero ad un altro
- **ponte**: arco la cui eliminazione disconnette il grafo
- arco di ritorno??
<br>
- **punto di articolazione**: vertice la cui rimozione è in grado di sconnettere il grafo
- **cappi**: nodi che nascono e muoiono su loro stessi
- **nodi dummy**: archi fasulli, ad esempio se un arco pesa 3 inseriamo due nodi finti in modo che è come se attraversassimo 3 archi invece che 1


##### raggiungibilità e connettività
- raggiungibilità: ho un nodo $u$, quali nodi posso raggiungere
	- può essere unidirezionale, $u$ può essere raggiunto da $v$, ma non necessariamente il contrario
	- in un grafo diretto un nodo $u$ è raggiungibile se esiste un percorso diretto dal nodo $v$
- connettività: si riferisce alla possibilità di raggiungere qualsiasi nodo partendo da qualsiasi altro nodo
	- in un grafo non orientato, il grafo è **connesso** se esiste un percorso tra ogni coppia di nodi
		- si può verificare con DFS o BFS in $O(n+m)$
	- in un grafo diretto: 
		- il grafo fortemente connesso $(SCC)$[^3]: se per ogni coppia di nodi $u,v$ esiste
			- un cammino diretto da $u$ a $v$
			- un cammino diretto da $v$ a $u$
		- se la connettività esiste solo ignorando la direzione degli archi, il grafo è **debolmente connesso**
<br>
- La **raggiungibilità** riguarda singole coppie di nodi
- La **connettività** riguarda l’intero grafo e la presenza di percorsi tra tutti i nodi

>Una _componente connessa_ di un grafo è un sottografo composto da un insieme di nodi connessi con dei cammini
	 Un grafo si dice connesso se ha una sola componente connessa

>Una _componente fortemente connessa_ di un grafo diretto è un sottografo composto da un insieme massimale di nodi connessi da cammini
	 Un grafo diretto si dice fortemente connesso se ha una sola componente
<br>
- nodo sorgente: nodo in cui non entrano archi

In un grafo diretto sommando le liste ottengo tutti i nodi raggiunti dagli archi nel grafo

- **Albero**: grafo particolare che ha sempre $m = n-1$ archi (quindi è sparso)
- Albero **DFS**: albero formato durante una visita DFS composto da nodi e archi effettivamente traversati
- Albero **BFS**: albero formato durante una visita BFS, detto *albero dei cammini minimi*
- albero radicato: albero in cui abbiamo definito una radice
- albero bilanciato: albero in cui l'altezza dei sottoalberi di ciascun nodo differiscano al massimo di una quantità predefinita (solitamente 1)
	- utile per mantenere basso il costo delle operazioni di ricerca, inserimento e cancellazione, garantendo complessità $O(\log n)$
### Chi preferire? 
- Si preferisce una matrice quando
	abbiamo un numero di archi vicino a $n^2$ ovvero un grafo denso e se dobbiamo controllare frequentemente l’esistenza di archi o in generale vogliamo accessi costanti
- Lista di adiacenza
	se abbiamo un grafo sparso, vogliamo risparmiare memoria e dobbiamo iterare sui nodi adiacenti di un nodo

###### Visite nei grafi
- **DFS**: Depth-first search (visita in profondità)
	- $O(n+m)$
- **BFS**: Breadth-first search (visita in ampiezza)
	- $O(n+m)$
	- $O(n^2)$ se coda implementata tramite una lista

#### Vettore dei padri
Un albero DFS può essere memorizzato tramite il vettore dei padri:
il _vettore dei padri_ $P$ di un albero DFS di un grafo di $n$ nodi ha $n$ componenti:
- se $i$ è nodo dell'albero allora $P[i]$ contiene il padre del nodo $i$
- se $i$ non è nodo (non è stato esplorato) allora $P[i]$ contiene il valore $-1$

**Cammino**: funzione che restituisce il cammino dalla radice dell'albero DFS fino a $u$ utilizzando il vettore dei padri $P$ 
il cammino minimo è quello che attraversa il minor numero di archi
<br>
_Teorema dei 4 colori_: un grafo planare richiede al più 4 colori per essere colorato

#### Vettore delle distanze
- _distanza minima_: è il numero minimo di archi che bisogna attraversare per raggiungere il nodo $b$ a partire dal nodo $a$
	- inizializzata a $+\infty$
- il vettore delle distanze $D$ a partire da un nodo $x$, contiene in $D[y]$ la distanza di $y$ da $x$

###### Grafo trasposto:
dato un grafo diretto $G$ il grafo trasposto di $G$, $G^T$ ha gli stessi nodi di $G$ ma archi con direzione opposta

##### Ordinamento topologico: 
l'**ordinamento topologico** di un **grafo diretto aciclico (DAG)** è una disposizione lineare dei suoi nodi tale che:
- per ogni arco $u \to v$, il nodo $u$ compare _prima_ del nodo $v$ nella sequenza
- ha sempre un nodo sorgente
<br>
### Algoritmi
un algoritmo è:
- **corretto**: funziona per tutti gli input del problema
- **euristicamente corretto**: non garantisce sempre la soluzione ottima, ma spesso ne trova una buona
- **sbagliato**: non restituisce la risposta corretta per almeno un input
- [[Algoritmi noti]]
--- 

- **UNION-FIND**: struttura dati per la collezione $C$ delle componenti connesse di un grafo di $n$ nodi, testa efficientemente appartengono o meno alla stessa componente connessa

[^4]: credo che sia circa $2^{\Omega(n)}$, ma senza l'uguale: chat mi conferma che è così e si scrive $2^{\omega(n)}$, $\omega$ indica crescita strettamente maggiore

---
# ottimizzazione e approssimazione
_copertura tramite nodi_: dato un grado non diretto $G$, essa è un sottoinsieme $S$ dei suoi nodi tale che tutti gli archi di $G$ hanno almeno un estremo in $S$

- **algoritmi di approssimazione**
- **euristiche**