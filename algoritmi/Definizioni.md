*%% lezione 27/02/2025 %%*
## Algoritmo efficiente/inefficiente/ottimo
- un algoritmo si dice efficiente se è di ordine **polinomiale**, nella dimensione n dell'input:
	- $O(n^c)$ per una qualche costante $c$
- inefficiente se la sua complessità è di ordine **super-polinomiale**:
	- *esponenziale*: funzione di ordine $\theta(c^n)=2^{\theta(n)}$
	- *super-esponenziale*: funzione che cresce più velocemente di un esponenziale 
	esempi: $2^{\theta(n^2)}\ \text{oppure}\ 2^{\theta(n\log n)},$ (credo che sia circa $2^{\Omega(n)}$, ma senza l'uguale)
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

-----
*%% lezione 28/02/2025 - 03/03/2025 %%*

# Grafi (to fix)
[[Grafi]]
$G(V,E), |V|=n \ (\text{nodi}), |E|= m\ (\text{archi})$; (arco = coppie  di nodi)

- cappi: nodi che nascono e muoiono su loro stessi
- grafi diretti: direzionali
- grafi non diretti: i collegamenti sono bilaterali

- grafo sparso se $m= O(n)$
- grafo denso se $m = \Omega(n^2)$
	- un grafo si dice completo se ha tutti gli archi
	- si dice torneo se tra ogni coppia di nodi c'è esattamente un arco
- un grafo può essere né sparso né denso

- grafo planare: grafi che possono essere disegnati senza che gli archi si intersechino

##### differenza tra raggiungibilità e connettività
- raggiungibilità: ho un nodo $u$, quali nodi posso raggiungere
	- può essere unidirezionale, $u$ può essere raggiunto da $v$, ma non necessariamente il contrario
	- in un grafo diretto un nodo $u$ è raggiungibile se esiste un percorso diretto dal nodo $v$
- connettività: si riferisce alla possibilità di raggiungere qualsiasi nodo partendo da qualsiasi altro nodo
	- in un grafo non orientato, il grafo è connesso se esiste un percorso tra ogni coppia di nodi
	- in un grafo diretto: 
		- il grafo fortemente connesso $(SCC)$[^3]: se per ogni coppia di nodi $u,v$ esiste
			- un cammino diretto da $u$ a $v$
			- un cammino diretto da $v$ a $u$
		- se la connettività esiste solo ignorando la direzione degli archi, il grafo è **debolmente connesso**.

- La **raggiungibilità** riguarda singole coppie di nodi.
- La **connettività** riguarda l’intero grafo e la presenza di percorsi tra tutti i nodi.

--

In un grafo diretto sommando le liste ottengo tutti i nodi raggiunti dagli archi nel grafo

**Albero**: grafo particolare che ha sempre $m = n-1$ archi (quindi è sparso)
### Chi preferire? 
- Si preferisce una matrice quando
	abbiamo un numero di archi vicino a $n^2$ ovvero un grafo denso e se dobbiamo controllare frequentemente l’esistenza di archi o in generale vogliamo accessi costanti
- Lista di adiacenza
	se abbiamo un grafo sparso, vogliamo risparmiare memoria e dobbiamo iterare sui nodi adiacenti di un nodo

## Visite nei grafi
- DFS: Depth-first search (visita in profondità)
- BFS: Breadth-first search (visita in ampiezza)- [ ] 17:07 - 17:37 Pomodoro #1


[^3]: SCC : Strongly Connected Components
