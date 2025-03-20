*%% lezione 03/03/2025 %%*
###### Nodo raggiungibile
Dato un grafo G ed un suo nodo $u$ vogliamo sapere quali nodi del grafo sono raggiungibili da $u$

controllare i vicini alla peggio ci costa $O(n)$
in una matrice pago esattamente $\theta(n)$
mentre in una lista di adiacenza $\theta(1)$ basta fare $len(G[u])$


![[dir/dir algoritmi/asset/file 10.png|dir algoritmi/asset/file 10.png]]
- Raggiungibili da $(0,G)={0,1,2,3,4,5,6}$
- Raggiungibili da $(6,G)={2,4,6}$
- Raggiungibili da $(5,G)={5}$

### DFS: Depth-first search
Visita in profondità su un grafo rappresentato tramite matrice binaria
```python 
def DFS(u, M): 
	def DFSr(u, M, visitati):
		visitati[u] = 1
		for i in range(len(M)): #vicini di u, controllo l'intera riga θ(n)
			if M[u][i] == 1 and not visitati[i]:
				DFSr(i, M, visitati) #ricorsione

	n = len(M)
	visitati = [0] * n #vettore di booleani (0,1), O(n)
	DFSr(u, M, visitati)
	return [x for x in range(n) if visitati[x]] #riporta i visitati, O(n)
```
- $Visitati[i]=1$  se e solo se $i$ è raggiungibile da $u$
- Complessità: $O(n)\times\theta(n)=O(n^2)$

DFS tramite lista di adiacenza:
```python 
def DFS(u, G):
	def DFSr(u, G, visitati): #G grafo, non si passa più la matrice
		visitati[u] = 1
		for v in G[u]:
			if not visitati[v]: #O(1) poichè lista 
				DFSr(v, G, visitati) #chiamata ricorsiva
				
	n = len(G)
	visitati = [0] * n
	DFSr(u, G, visitati)
	return [x for x in range(n) if visitati[x]]
```
- $Visitati[i]=1$  se e solo se $i$ è raggiungibile da $u$
- Complessità: $O(n+m)$
	in un grafo non diretto avremmo $O(n+2m)$ che asintoticamente non tange
- complessità spaziale: $O(n)$
- se il grafo è sparso, con $m$ dell'ordine di $n$ allora $O(n+m)=O(n+n)=O(2n)=O(n)$

- worst case: grafo completo poiché $O(n^2)$


partendo da $x$ sapendo che arrivo a $7$ tra $x$ e $7$ non ci possono essere $0$ (che indicherebbe che non ho ancora visitato il nodo, se si sarà $1$ alla prossima iterazione)

accorgimenti:
- occhio a non creare un vettore visitati che sia una variabile globale in quanto non è parte delle regola di buona progettazione, modularità e mantenimento del codice
- durante la visita non bisogna cadere nei loop
- in $C$ passare il vettore costerebbe un sacco a livello di spazio, sia nel caso in cui non utilizzassimo i puntatori, sia perché non sono mutabili
- aggiungere un elemento in coda ad una lista cosa $O(1)$
	mentre inserire in un insieme python non costa precisamente $O(1)$ in quanto andrebbe controllato (politica no duplicati)
- per ogni algoritmo ricorsivo ne esiste il corrispondente iterativo, che simula lo stack con la pila[^1]


[^1]: ```python 
	def DFS_iterativo(u.G):
		#esegue visita dei nodi di G raggiungibili a partire dal nodo u
		visitati = [0] * len(G)
		pila = [u] #inizializza la pila con il nodo di partenza
		while pila:
			u = pila.pop()
			if not visitati[u]:
				visitati[u] = 1
				#aggiungiamo i vicini non visitati
				for v in G[u]:
					if not visitati[v]:
						pila.append(v)
		return [x for x in range(len(G)) if visitati[x]]
		#complessita O(n+m)
		#complessità di spazio O(n)
	```

--- 

###### Distanza
Dati due nodi $a$ e $b$, la _distanza minima_ nel grafo $G$ di $a$ da $b$ è il numero minimo di archi che bisogna attraversare per raggiungere $b$ a partire da $a$
- per convenzione la poniamo a $+\infty$ nel caso in cui $b$ non fosse raggiungibile
- equivale alla profondità di $a$ nell'albero BFS
- l’albero BFS è anche detto *albero dei cammini minimi*

Il vettore delle distanze $D$ a partire da un nodo $x$, contiene in $D[y]$ la distanza di $y$ da $x$

### BFS: Breadth-First Search
La BFS, visita in ampiezza esplora i nodi del grafo partendo da quelli a distanza $1$ dalla sorgente, poi quelli a distanza 2 e così via, l'algoritmo guarda tutti i nodi a livello $k$ prima di passare al livello $k+1$

- manteniamo in una coda i nodi visitati i cui adiacenti non sono ancora stati esplorati
- ad ogni passo preleviamo dalla coda ed esaminiamo i suoi adiacenti
- se scopriamo un nuovo nodo lo visitiamo e lo aggiungiamo alla coda
- a fine algoritmo avremo che `visitati[u]`$= 1$ soltanto se $u$ è raggiungibile da $x$
```python 
def BFS(x, G):
	visitati = [0] * len(G)
	visitati[x] = 1
	coda = [x] #coda contenente il nodo di partenza
	while coda:
		u = coda.pop(0) #estraiamo un nodo O(n)
		for y in G[u]: #visitiamo tutti gli adiacenti e li mettiamo in coda O(m)
			if visitati[y] == 0:
				visitati[y] = 1
				coda.append(y) #messo in coda solo se non visitato
	return visitati
```

costo:
- un nodo finisce in coda al più una volta, `while` costa $O(n)$
- le liste di adiacenza dei nodi verranno scorse al più una volta quindi il costo totale dei `for` sarà $O(m)$
	- se le operazioni sulla coda costano $1$ abbiamo: $O(n+m)$
- ma abbiamo implementato la coda come una lista, inseriamo in coda e cancelliamo in testa:
	- inserimento costa $1$
	- estrazione (`pop(0)`) costa in base a quanti elementi ci sono $O(n)$ 
	- quindi $\Rightarrow  O(n^2)$
- possiamo fare delle modifiche e ottenere un costo di $O(n+m)$
	- effettuo solo cancellazioni logiche e non effettive: invece di eliminare gli elementi usiamo un puntatore che incrementiamo di volta in volta[^2]:
	```python
		def BFS(x, G):
			visitati = [0] * len(G)
			visitati[x] = 1
			coda = [x]
			i = 0
			while len(coda) > i:
				u = coda[i]
				i += 1
				for y in G[u]:
					if visitati[y] == 0:
						visitati[y] = 1
						coda.append(y)
	return visitati
	```
- per ottenere il vettore dei padri:
```python 
def BFSpadri(x, G):
	P = [-1] * len(G)
	P[x] = x
	coda = [x]
	i = 0
	while len(coda) > i:
		u = coda[i]
		i += 1
		for y in G[u]:
			if P[y] == -1:
				P[y] = u
				coda.append(y)
	return P
```
- per ottenere il vettore delle distanze $D$:
	- se in un nodo abbiamo come valore $−1$ allora significa che non abbiamo raggiunto quel nodo
```python 
def BFSdistanza(x, G):
	D = [-1] * len(G)
	D[x] = 0
	coda = [x]
	i = 0
	while len(coda) > i:
		u = coda[i]
		i += 1
		for y in G[u]:
			if D[y] == -1:
				D[y] = D[u] + 1
				coda.append(y)
	return D
```

[^2]: in python è possibile farlo anche tramite la struttura _deque_
