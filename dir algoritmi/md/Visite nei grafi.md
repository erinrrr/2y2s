*%% lezione 03/03/2025 %%*
Dato un grafo G ed un suo nodo $u$ vogliamo sapere quali nodi del grafo sono raggiungibili da $u$

controllare i vicini alla peggio ci costa $O(n)$
in una matrice pago esattamente $\theta(n)$
mentre in una lista di adiacenza $\theta(1)$ *basta fare $len(G[u])$*


![[Pasted image 20250305164021.png]]
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


[^1]: 
```python 
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