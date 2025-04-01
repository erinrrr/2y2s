# Ricerca
#### DFS matrice binaria
- complessità: $O(n)\times\theta(n)=O(n^2)$
```python 
def DFS(u,G):
	def DFSr(u,G,visitati,D,distanza):
	    visitati[u] = 1
	    D[u] = distanza
	    for v in G[u]:
	        if not visitati[v]:
	           dfsR(v,G,visitati,D,distanza+1)
	           
    n = len(G)
    D = [-1]*n
    distanza=0
    visitati= [0]*n
    DFSr(u,G,visitati,D,distanza)
    return D
```


#### DFS liste di adiacenza
- complessità: $O(n+m)$
	- best case: grafo sparso $O(n)$
	- worst case: grafo completo $O(n^2)$
```python 
def DFS(u, G):
	def DFSr(u, G, visitati): #G grafo, non si passa più la matrice
		visitati[u] = 1
		for v in G[u]:
			if not visitati[v]: #O(1) poichè lista 
				DFSr(v, G, visitati)
				
	n = len(G)
	visitati = [0] * n
	DFSr(u, G, visitati)
	return [x for x in range(n) if visitati[x]]
```

#### BFS
- complessità: $O(n+m)$
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
###### BFS per ottenere il vettore delle distanze
- se abbiamo come valore $-1$ in un nodo allora non lo abbiamo raggiunto
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

#### Trasposizione di grafi
- complessità: $O(n+m)$
```python 
def Trasposto(G):
  GT = [[] for _ in G]
   for i in range (len(G)):
    for v in G[i]:
        GT[v].append(i)
        
  return GT
```

--- 
#### Vettore dei padri
- DFS
- complessità$O(n)$
```python 
def Padri(u, G):

    def DFSr(x, G, P):
        for y in G[x]:
            if P[y] == -1:
                P[y] = x
                DFSr(y, G, P)
                
    n = len(G)
    P = [-1] * n
    P[u] = u
    DFSr(u,G,P)
    return P
```
- BFS
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
---
#### Cammino se si conosce il vettore dei padri
- complessità: $O(n)$
```python 
def Cammino(u, P):
    if P[u] == -1: return []
    path = []
    while P[u] != u:
        path.append(u)
        u = P[u]
    path.append(u)
    path.reverse()
    return path
```
- versione ricorsiva
```python 
def CamminoR(u, P):
    if P[u] == -1: return []
    if P[u] == u: return [u]
    return CamminoR(P[u], P) + [u]
```
---
#### Colorazione di grafi
- bi-colorazione  del grafo se bi-colorabile altrimenti produce una lista vuota
- complessità: $O=n+m$ (quella di una semplice visita) $= O(m)$ in quanto è connesso
```python 
#O(m) poichè in un grafo connesso m >= n-1
def Colora1(G):

    def DFSr(x, G, Colore, c):
        Colore[x] = c
        for y in G[x]:
            if Colore[y] == -1:
                if not DFSr(y, G, Colore, 1-c):
                    return False
            elif Colore[y] == Colore[x]:
                    return False
        return True
        
    Colore = [-1] * len(G)
    if DFSr(0, G, Colore, 0):
        return Colore
        
    return []
```
---
#### Componenti connesse
- restituisce il vettore C delle componenti connesse del grafo G
- complessità: $O(n+m)$
```python 
def Componenti(G):

    def DFSr(x, G, C, c):
        C[x] = c
        for y in G[x]:
            if C[y] == 0:
                DFSr(y, G, C, c)
                
    C = [0] * len(G)
    c = 0
    
    for x in range(len(G)):
        if C[x] == 0:
            c += 1
            DFSr(x, G, C, c)
            
    return C
```
- calcola le componenti fortemente connesse di un nodo $x$
- complessità: $O(n+m)$
```python 
def ComponenteFC(x, G):
  visitati1 = DFS(x, G)
  G1 = Trasposto(G)
  visitati2 = DFS(x, G1)
  componente = []
  
  for i in range((len(G))):
    if visitati1[i] == visitati2[i] == 1:
        componente.append()
        
    return componente
```
- vettore delle componenti fortemente conne3sse
- complessità: $O(n^2)$
	- worst case: $\theta(n^3)$
```python 
def compFC(G):
	FC = [0] * len(G)
	c = 0
	
	for i in range(len(G)):
		if FC[i] == 0:
			E = ComponenteFC(i, G)
			c += 1
			for x in E:
				FC[x] = c
	return FC
```
---
#### Ordinamento topologico
- restituisce un sort topologico ST di G se esiste altrimenti lista vuota
- complessità: $O(n+m)$
```python 
def sortTop(G):
    n = len(G)
    gradoEnt = [0] * n

	for i in range(n):
        for j in G[i]:
            gradoEnt[j] += 1
    sorgenti = [ i for i in range(len(G)) if gradoEnt[i] == 0 ]
    ST = []
    
    while sorgenti:
        u=sorgenti.pop()
        ST.append(u)
	    for v in G[u]:
            gradoEnt[v] -= 1
            if gradoEnt[v] == 0:
                sorgenti.append(v)
                
    if len(ST)==len(G): return ST
    return []
```
- sort topologico di un $DAG$
- complessità $O(n+m)$
```python 
def sortTop1(G):
 
	def DFSr(u, G, visitati, lista):
		visitati[u] = 1
		for v in G[u]:
			if visitati[v] == 0:
				DFSr(v, G, visitati, lista)
		lista.append(u)
 
	visitati = [0] * len(G)
	lista = []
	for u in range(len(G)):
		if visitati[u] == 0:
			DFSr(u, G, visitati, lista)
	lista.reverse()
	return lista
```
---
#### Verificare la presenza di un ciclo in un grafo
- verifica se vi sono cicli in un grafo che sia diretto o non diretto, verificando se vi sono archi all'indietro
- complessità: $O(n+m)$
```python 
def cicloD(G):

	def DFSr(u, G, visitati):
		visitati[u] = 1
		for v in G[u]:
			if visitati[v] == 1:
				return True
			if visitati[v] == 0:
				if DFSr(v, G, visitati):
					return True
		visitati[u] = 2
		return False

	visitati = [0] * len(G)
	for u in range(len(G)):
		if visitati[u] == 0:
			if DFSr(u, G, visitati):
				return True
	return False
```
--- 
#### Ricerca di ponti
- ricerca ponti in un grafo connesso non orientato rappresentato come lista di adiacenza
- complessità $O(n+m)$
```python 
## O(n+m)
def trova_ponti(G):
    """ Trova tutti i ponti in un grafo connesso non orientato G.
    G è rappresentato come una lista di adiacenza.
    Restituisce una lista di coppie (u, v) che sono ponti.
    """
    def dfs(x, padre, altezza, ponti):
        if padre == -1:
            altezza[x]=0
        else:
            altezza[x] = altezza[padre] + 1
        min_raggiungibile = altezza[x]
        for y in G[x]:
            if altezza[y] == -1:
                b = dfs(y, x, altezza, ponti)
                if b > altezza[x]:
                    # l'altezza di x è minore di quella ritornata da y,
                    # quindi (x, y) è un ponte
                    ponti.append((x, y))
                min_raggiungibile = min(min_raggiungibile, b)
            elif y != padre:
                # y già visitato e (x,y) è un arco all'indietro
                min_raggiungibile = min(min_raggiungibile, altezza[y])
        return min_raggiungibile
    
    altezza = [-1] * len(G)
    ponti = []
    dfs(0, -1, altezza, ponti) #inizia la DFS dal nodo 0
    return ponti
```

--- 
## Algoritmi famosi
- Dijkstra: implementato con heap minimo
- complessità: $O((n+m)\log n)$
	- worst case: grafo è denso $O(n^2\log n)$
```python 
def dijkstra1(s, G):
 
    n = len(G)
    D = [float('inf')] * n  #distanze inizialmente infinite
    P = [-1] * n  #predecessori inizializzati a -1
    D[s] = 0 #la sorgente dista 0 da se stessa
    P[s] = s #la sorgente è la radice dell'albero delle distanze
    H = []  #min-heap
    
    for y, costo in G[s]: #inizializziamo l'heap con i vicini della sorgente
        heappush(H, (costo, s, y))  #(costo, x=s, y)
    
    while H:
        #estrai il nodo con distanza minore
        costo, x, y = heappop(H)
        if P[y] == -1:
            P[y] = x #inserisci y nel vettore dei padri come figlio di x
            D[y] = costo #setta la distanza minima di y
            for v, peso in G[y]: #esplora i vicini di y
                if P[v] == -1: #se v non è ancora stato aggiunto all'albero
                    heappush(H, (D[y] + peso, y, v))
    return D, P
```
- Kruskal: risolve il problema del minimo albero di copertura dato un grafo
- complessità: $O(m\cdot n)$
```python
def kruskal(G):
	E = [(c, u, v) for u in range(len(G)) for v,c in G[u] if u<v]
	E.sort()
	T = [[] for _ in G]
	for c, u, v in E:
		if not connessi(u, v, T):
			T[u].append(v)
			T[v].append(u)
	return T
 
def connessi(u, v, T):
	def DFSr(a, b, T, visitati):
		visitati[a] = 1
		for z in T[a]:
			if z == b:
				return True
			if not visitati[z]:
				if DFSr(z, b, T, visitati):
					return True
		return False
 
	visitati = [0] * len(T)
	return DFSr(u, v, T, visitati)
```
- kruskal implementato con UNION-FIND
- complessità:$O(m\log n)$
```python 
def kruskal1(G):
	E = [(c,u,v) for u in range(len(G)) for v,c in G[u] if u<v]
	E.sort()
	T = [[] for _ in G]
	C = Crea(T)
	for c, u, v in E:
		cu = FIND(u, C)
		cv = FIND(v, C)
		if cu != cv:
			T[u].append(v)
			T[v].append(u)
			Union(cu, cv, C)
	return T
```
dove:
```python 
def Crea(G):
	C = [(i, 1) for i in range(len(G))]
	return C
 
 
def Find(u, C):
	while u != C[u]:
		u = C[u]
	return u
 
 
def Union(a, b, C):
	tota, totb = C[a][1], C[b][1]
	if tota >= totb:
		C[a] = (a, tota + totb)
		C[b] = (a, totb)
	else:
		C[b] = (b, tota + totb)
		C[b] = (a, totb)
```
