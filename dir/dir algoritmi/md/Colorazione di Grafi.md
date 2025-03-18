*%% lezione 06/03/2025 - 07/03/2025%%*

Dato un grafo connesso $G$ e un intero $k$ vogliamo sapere se è possibile colorare i nodi del grafo in modo che nodi adiacenti abbiano sempre colori distinti

###### Teorema dei 4 colori: un grafo planare richiede al più 4 colori per essere colorato

In generale un grafo colorato con:
- $1$ solo colore se non ci sono archi 
- $2$ colori se e solo se non contiene un ciclo dispari
- 4 colori se grafo planare (sempre)
- può richiedere anche $\theta(n)$ colori, caso di grafo completo con $n$ nodi e $n>4$
- non è noto nessun algoritmo che determini se un grafo planare è $3$-colorabile


> [!NOTE] Round Robin
> il prof ha citato l'algoritmo di round robin per quanto riguarda la colorazione, to check!


### Algoritmo per bi-colorare grafi connessi senza cicli dispari
- tutti i nodi sono inizialmente colorati con -$1$
- colora il nodo $0$ con $0$
- effettua una DFS a partire dal nodo $0$, e a ciascun nodo $x$ assegna il colore $0$ o $1$ affinché sia diverso dal nodo padre

prova di correttezza, siano $x$ e $y$ due nodi adiacenti possono verificarsi due casi:
- l'arco $(x,y)$ viene attraversato nella visita, e quindi hanno colori distinti
- l'arco $(x,y)$ non viene attraversato, allora sia $x$ il nodo visitato per primo, esiste un cammino che ci porta da $x$ a $y$ che forma un ciclo con l'arco $(x,y)$, il ciclo è pari per ipotesi, quindi il cammino è dispari 

codice:
```python 
def Colora(G):
	#restituisce una bicolorazione dei nodi su grafi connessi bicolorabili
	def DFSr(x, G, Colore, c):
		Colore[x] = c
		for y in G[x]:
			if Colore[y] == -1:
				DFSr(y, G, Colore, 1-c) #1-c serve a cambiare la parità del colore
	Colore = [-1] * len(G) #lista dei nodi colorati tutti a -1 di default
	DFSr(0, G, Colore, 0)
	return Colore
```

il codice seguente produce una bi-colorazione se il grafo $G$ è bi-colorabile altrimenti produce una lista vuota:
```python 
def Colora1(G):
	#return una bicolorazione di G se il grafo è bicolorabile altrimenti una lista vuota 
	def DFSr(x, G, Colore, c):
		Colore[x] = c
		for y in G[x]: #itera sui vicini
			if Colore[y] == -1: #se non è stato ancora colorato avvio la ricorsione
				if not DFSr(y, G, Colore, 1-c): 
					return False #propaga il risultato nello stack
				elif Colore[y] == Colore[x]: #controlla se il vicino di x (già colorato) abbia lo stesso colore di x
					return False
			return True
 
	Colore = [-1] * len(G)
	if DFSr(0, G, Colore, 0):
		return Colore
	return []
```


la complessità dell'algoritmo per testare se un grafo è bi-colorabile è quella di una semplice visita del grafo connesso quindi $O(n+m)=O(m)$ in quanto è un grafo connesso $m\geq n-1$

### Componente connessa
Una componente connessa di un grafo è un sottografo composto da un insieme di nodi connessi con dei cammini. Un grafo **si dice connesso se ha una sola componente connessa**.
Esempio di grafo con più componenti connesse:
![[dir/dir algoritmi/asset/file 11.png|dir algoritmi/asset/file 11.png]]

Il vettore $C$ delle componenti connesse di un grafo $G$ è il vettore che ha tanti elementi quanti sono i nodi del grafo e $C[u]=C[v] \iff u$ e $v$ sono nella stessa componente connessa
![[dir/dir algoritmi/asset/file 12.png|dir algoritmi/asset/file 12.png]]
$C=[2,1,2,3,4,5,2,1,1,1,2,3,5,1,5,1,2,5,3]$

```python 
def Componenti(G):
	 #restituisce il vettore C delle componenti connesse del grafo G
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

Questo algoritmo scorre tutti i nodi nella liste di adiacenza, inizia con un valore $c=1$ per il primo nodo e tutti quelli collegati a lui prenderanno il valore $1$, quando non ci sarà più nessun nodo collegato a lui che ha valore $0$ allora si passa al successivo nodo di un’altra componente (se esiste) aumentando il valore di c di $1$, se coloro tutto in una sola visita allora il grafo è connesso.

- $1°$ visita $O(n_{1}+m_{1})$
- $2°$ visita $O(n_{2}+m_{2})$
- $\dots$ $k° \Rightarrow O(n_{k}+m_{k}) \Rightarrow \ generalizzando \  O(n+m)$
### Componente fortemente connessa
Una componente fortemente connessa di un grafo diretto è un sottografo composto da un insieme massimale di nodi connessi da cammini (c'è una sorta di ciclicità, il path tra le comp fc è chiuso tra di loro)
Un grafo diretto si dice fortemente connesso se ha una sola componente
![[dir/dir algoritmi/asset/file 13.png|dir algoritmi/asset/file 13.png]]

Il vettore $C$ delle componenti fortemente connesse di un grafo $G$ è il vettore che ha tanti elementi quanti sono i nodi del grafo e $C[u]=C[v] \iff u$ e $v$ sono nella stessa componente fortemente connessa
$C=[1,1,2,1,1,3,1,1,4,5,4,4]$

Non possiamo usare lo stesso algoritmo delle componenti connesse in quanto essendo in un grafo diretto pur avendo un cammino da $x$ a $y$ non è detto il contrario, il nuovo algoritmo sarà:

- calcolare l'insieme $A$ dei nodi di $G$ raggiungibili da $u$ 
	visita DFS quindi $O(n+m)$
- calcolare l'insieme $B$ dei nodi di $G$ che portano a $u$
	ci calcoliamo il [[Definizioni#Grafo trasposto|grafo trasposto]] in $O(n+m)$
	visita DFS in $G^T$, $O(n+m)$		
- restituire l'intersezione di questi due insiemi
	$O(n)$
	
$\Rightarrow$ l'algoritmo per calcolare le componenti fortemente connesse di un nodo $x$ ha complessità $O(n+m)$ ed è il seguente:
```python 
def ComponenteFC(x, G):
	visitati1 = DFS(x ,G)
	G1 = Trasposto(G)
	visitati2 = DFS(x, G1)
	componente = []
	for i in range(len(G)):
		if visitati1[i] == visitati2[i] == 1:
			componente.append(i)
	return componente
```

ove volessimo il **vettore delle componenti fortemente connesse** possiamo usare l’algoritmo `ComponenteFC` come subroutine e ottenere:
```python 
def compFC(G):
	FC = [0] * len(G)
	c = 0
	for i in range(len(G)): #θ(n)
		if FC[i] == 0:
			E = ComponenteFC(i, G) #O(n+m)
			c += 1
			for x in E:
				FC[x] = c
	return FC
```
che ha complessità $\theta(n)\cdot O(n+m)=O(n^2+nm)=O(n^3)$
al caso pessimo invece è $\theta(n^3)$[^1]

Per il calcolo del vettore CF delle componenti fortemente connesse sono noti diversi algoritmi non banali che lavorano in tempo (ad esempio l’algoritmo di Tarjan o quello di Kosaraju)

[^1]: esempio il grafo diretto G avente un arco da $u$ a $v$ per ogni coppia di nodi $u, v$ con $u<v$.
	Nota che questo grafo ha $\theta(n^2)$ archi e $n$ componenti fortemente connesse (ciascuna composta da un singolo nodo).![[dir/dir algoritmi/asset/file 15.png|dir algoritmi/asset/file 15.png]]![[dir/dir algoritmi/asset/file 22.png|dir algoritmi/asset/file 22.png]]