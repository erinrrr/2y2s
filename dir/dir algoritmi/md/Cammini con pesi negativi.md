dato un grafo $G$ e un suo nono $s$ vogliamo determinare le distanze minime da questo nodo anche se sono presenti archi con pesi negativi

> [!info]- cammino con ciclo negativo
> se in un cammino tra i nodi $s$ e $t$ è presente un nodo che appartiene ad un ciclo negativo, allora non esiste il cammino di costo minimo tra $s$ e $t$ 
> ![[file 36.png]]
> se per il ciclo $W$ si ha $costo(W) < 0$, ripassando più volte attraverso il ciclo $W$ possiamo abbassare arbitrariamente il costo del cammino da $s$ a $t$

la formulazione del problema di prima non era del tutto corretta:
- dato un grafo diretto e pesato $G$ in cui i pesi degli archi possono essere anche negativi, e fissato un suo nodo $s$ vogliamo determinare il costo minimo dei cammini che conducono da $s$ a tutti gli altri del grafo
- se non esiste un cammino verso un determinato nodo il costo sarà considerato infinito

abbiamo già affrontato questo problema nel caso in cui i pesi del grafo $G$ siano tutti non negativi utilizzando l'algoritmo di Dijkstra, ma sappiamo che può produrre risultati errati

# Algoritmo di Bellman-Ford
- è un algoritmo più adatto per risolvere questo tipo di problemi
- basato sulla tecnica della programmazione dinamica
- risolve questo problema in tempo $O(n^2+m\cdot n)$

_proprietà_:
>se il grafo $G$ non contiene cicli negativi allora per ogni nodo $t$ raggiungibile dalla sorgente $s$ esiste un cammino di costo minimo che attraversa al più $n-1$ archi

_dimostrazione_:
- se un cammin avesse più di $n-1$ archi, allora almeno un nodo verrebbe ripetuto formando quindi un ciclo
- poiché il grafo non ha cicli negativi, rimuovere eventuali cicli dal cammino non aumenta il suo costo complessivo
- dunque esiste sempre un cammino ottimale di lunghezza $n-1$
- questo garantisce che il costo minimo può essere calcolato considerando solo cammini di questa lunghezza

![[file 37.png]]


data questa proprietà, consideriamo sottoproblemi che si ottengono limitando la lunghezza dei cammini, definiamo quindi la tabella $T$ di dimensione $n\times n$, dove:
- $T[i][j]=$ costo di un cammino minimo dal nodo $s$ al nodo $j$ di lunghezza al più $i$

- la soluzione al nostro problema sarà data dall’ultima riga ($T[n-1][t]$)
- si capisce già che:
	- $T[0][t]$ (la prima riga) avrà tutti $-\infty$ ad eccezione della sorgente ($T[0][s]$) che avrà 0
	- $T[i][s] = 0$ per ogni $i>0$ (la sorgente inoltre avrà sempre costo $0$)
- i resti valori, quelli delle celle $T[i][j]$ con $j\ne s$ della riga $i>0$, si calcolano in funzione delle celle già calcolate della riga $i-1$
- si distinguono due casi:
	- il cammino di lunghezza al più $i$ da $s$ a $j$ abbia lunghezza inferiore a $i$
	- il caso in cui la lunghezza sia esattamente $i$

nel primo caso si ha:
- $T[i][j] = T[i-1][j]$

nel secondo caso:
- deve esistere un cammino minimo di lunghezza al più $i-1$ ad un nodo $x$ e poi un arco che da $x$ mi porta a $j$
- $T[i][j] = min_{(x,j) \in E}\big(T[i-1][x]+\cos to(x,j)\big)$
	- con $E$ si intende l'insieme degli archi
- dobbiamo però controllare quale è il minimo tra questo valore e il valore di $T[i-1][j]$, quindi:
	- $T[i][j]=min\Big(T[i-1][j],min_{(x,j) \in E}\big(T[i-1][x]+\cos to(x,j)\big)\Big)$
- generalmente:
$$
T[i][j] = 
\begin{cases}
0 &\ \text{ se } j = s \\
+\infty &\ \text{ se } i = 0 \\
\min\Big( T[i - 1][j],\ \min_{(x,j) \in E} \big( T[i - 1][x] + costo(x, j) \big) \Big) & \text{altrimenti}
\end{cases}
$$

_nota_ per un implementazione efficiente:
- nel calcolo della formula è necessario più volte conoscere gli archi entranti nel generico nodo $j$:
	- conviene quindi precalcolare il grafo trasposto $GT$ di $G$
	- così da avere in $GT[j]$ tutti i nodi $x$ tali che in $G$ esiste un arco da $x$ a $j$

#### Implementazione

```python
def CostoCammini(G, s):
	n = len(G)
	inf = float('inf')
	T = [[inf] * n for _ in range(n)]
	T[0][s] = 0
	GT = Trasposto(G)
	for i in range(1, n):
		for j in range(n):
			T[i][j] = T[i - 1][j]
			if j != s:
				for x, costo in GT[j]:
					T[i][j]=min(T[i][j], T[i-1][x] + costo)
	return T[n-1]
 
def Trasposto(G):
	n = len(G)
	GT = [[] for _ in G]
	for i in range(n):
		for j, costo in G[i]:
			GT[j].append((i, costo))
	return GT
```

**costo**:
- l'inizializzazione della tabella $T$ costa $\theta(n^2)$
- la costruzione del grafo trasposto $GT$ costa $O(n+m)$
- per i tre for annidati abbiamo, il limite superiore è $O(n^{3})$:
	- il for esterno costa $\theta(n)$
	- i due for interni però hanno costo totale $\theta(m)$, il tempo di scorrere le liste di adiacenza del grafo $GT$ che hanno lunghezza $m$
	- ne deriva che i tre for costano $\theta(n\cdot m)$
- _costo totale_: $O(n^2+mn)$

#### Implementazione per trovare i cammini
- per trovare anche i cammini con la tabella $T$ bisogna calcolare anche l'albero $P$dei cammini minimi
- bisogna mantenere per ogni nodo $j$ il suo predecessore cioè il nodo $u$ che precede $j$ nel cammino
- il valore di $P[j]$ andrà aggiornato ogni volta che il valore di $T[i][j]$ cambia (ossia diminuisce in quanto troviamo un cammino migliore)

```python
def CostoCammini(G, s):
	n = len(G)
	inf = float('inf')
	T = [[inf] * n for _ in range(n)]
	P = [-1] * n #inizializzazione del vettore dei padri
	T[0][s] = 0
	P[s] = s #s è la radice dell'albero dei cammini
	GT = Trasposto(G)
	for k in range(1, n):
		for j in range(n):
			if j == s:
				T[k][j] = 0
			else:
				for x, costo in GT[j]:
					if T[k-1][x] + costo < T[k][j]:
						T[k][j]=T[j-1][x] + costo
						#il cammino attuale minimo che da s arriva a j
						#ci arriva tramite x
						P[i] = x
	return T[n-1], P #return distanze e vettore dei padri
```

al termine dell'algoritmo avremo:
- $T[i][j] = +\infty$ allora:
	- $j$ non è raggiungibile a partire da $s$
	- $P[j]$ conterrà il valore $-1$
- $T[i][j]\ne + \infty$ allora:
	- $j$ è raggiungibile da $s$
	- $P[j]$ conterrà il nodo che precede $j$ nel cammino minimo da $s$ a $j$


_ottimizzazioni_:
- il contenuto di una cella della riga $k$ dipende dal contenuto delle cella alla riga $k-1$:
	- se $T[k] = T[k-1]$ anche le seguenti righe non varieranno, allora possiamo terminare l'algoritmo (non modifica l'asintotica, ma nel pratico conta)
	- non serve memorizzare l'intera tavella $T$, bastano le ultime due righe, potremmo modificarlo in mood da utilizzare $O(n)$ invece di $O(n^2)$

nota 2:
- a priori non sappiamo se il grafo su cui applichiamo l'algoritmo ha cicli negativi:
	- i cammini minimi calcolati potrebbero essere i minimi con lunghezza al più $n-1$ e non i minimi effettivi
- potremmo scoprire se il grafo contiene cicli negativi raggiungibili da $s$:
	- calcoliamo una riga in più (la riga $n$) con il costo dei cammini minimi di lunghezza al più $n$
	- le righe $n$ e $n-1$ della tabella saranno identiche $\iff$ nel grafo non ci sono cicli negativi raggiungibili da $s$
- questo controllo non modifica l'asintotica dell'algoritmo