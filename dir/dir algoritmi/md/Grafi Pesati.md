Abbiamo il seguente problema:
- tre contenitori di capienza 4, 7 e 10 litri, inizialmente i contenitori da 4 e 7 sono pieni e quello da 10 vuota
- possiamo soltanto versare acqua da un contenitori all’altro e fermarci solo quando il contenitore sorgente è vuoto o il destinazione è pieno
esiste una sequenza di operazioni che termina lasciando esattamente $2$ litri d’acqua nel contenitore da 4 o da 7?
- possiamo modellare il problema con un grafo diretto dove i nodi sono i possibili stati di riempimento dei 3 contenitori, avremo quindi delle terne $(a,b,c)$ dove $a$ indica il contenitore da 4, $b$ quello da 7 e $c$ quello da 10
- metteremo degli archi fra due nodi se è possibile passare fra lo stato di uno verso l’altro
Un frammento del grafo sarà quindi:
![[dir/dir algoritmi/asset/file 28.png]]
- dobbiamo capire se uno dei due nodi $(2,x,y)$, $(x,2,y)$ è raggiungibile dal nodo $(4,7,0)$
- possiamo usare una BFS per la ricerca del cammino minimo

consideriamo:
- buona: un'operazione se termina lasciando esattamente $2$ litri in uno dei due contenitori
- parsimoniosa: un'operazione in cui abbiamo spostato il minor numero di litri

potremmo modellare il problema assegnano un costo ad ogni arco, dove il costo sarà uguale ai litri spostati, in parte:
![[dir/dir algoritmi/asset/file 29.png]]

- il cammino minimo che cercheremo ora non sarà più in termini di archi percorsi ma in litri spostati
- un'idea potrebbe essere inserire dei **nodi dummy**, se un arco pesa ad esempio $3$ inseriamo $2$ nodi finti in modo che è come se attraversassimo $3$ archi invece di $1$, soluzione possibile solo con archi a costi interi e piccoli, non è scalabile come soluzione:
![[dir/dir algoritmi/asset/file 30.png]]

## Algoritmo di Dijkstra
- esiste un algoritmo che permette di trovare i cammini minimi quando si lavora su grafi pesati:
	- funziona anche con pesi non interi 
	- non è detto che funzioni con pesi negativi

l’algoritmo parte da un nodo sorgente e ad ogni passo aggiunge all’albero della ricerca l’arco che produce il nuovo cammino più economico per raggiungere un nuovo nodo, a questo nodo assegna il costo che abbiamo pagato per raggiungerlo ![[dir/dir algoritmi/asset/file 31.png]]
###### Prova di correttezza
Pseudocodice:
- $P[0…n−1]$ vettore dei padri inizializzato a $-1$
- $D[0…n−1]$ vettore delle distanze inizializzato a $+\infty$
- nodo $s$ come sorgente quindi $D[s]=0$
- `while` esistono archi `{x,y}` con `P[x] != -1` e `P[y] == -1`: ($x$ visitato e $y$ no)
    - sia `{x,y}` quello per cui è minimo `D[x] + peso(x,y)` allora `D[y], P[y] = D[x] + peso(x,y), x`
- return `P,D`

 ad ogni iterazione del while viene assegnata una nuova distanza ad uno dei nodi del grafo, dimostriamo per induzione sul numero di iterazioni che la distanza assegnata è quella minima:
- caso base: la sorgente ha come distanza 0 dato che banalmente non possono esserci percorsi inferiori (sempre considerando grafi senza pesi negativi)

siano:
- $T_{i}$ l'albero costruito dalla ricerca al passo $i$ 
- $(u,v)$ l'arco aggiunto al passo $i+1$
- bisogna dimostrare che il $D[v]$ è la distanza minima di $v$ da $s$ (sorgente), quindi che un cammino alternativo è sempre superiore o uguale a $D[v]$
![[dir/dir algoritmi/asset/file 32.png]]

- al passo $i+1$ l'algoritmo raggiunge $(u,v)$, mostriamo che $D[v]$ appena ottenuta è la distanza minima considerando un possibile percorso alternativo $C$
- sia $C$ un qualsiasi cammino da $s$ a $v$ alternativo a quello presente nell’albero e $(x,y)$ il primo arco che incontriamo percorrendo $C$ tale che $x$ è nell’albero $T_{i}$e $y$ no


quest'arco deve esistere perché l’algoritmo percorre in ordine: 
- $(s,u)$, $(s,x)$ e secondo l’ipotesi $(u,v)$ 
- quindi se esiste un percorso alternativo sicuramente non è $(x,v)$ altrimenti sarebbe stato scelto, deve esistere quindi un arco che ci porta fuori $T_{i}$ovvero $(x,y)$ e che poi ci porta a $u$ ovvero $(y,v)$

ma:
- per ipotesi induttiva, $costo(C)\ge D[x]+peso(x,y)$ dato che $C$ è composto anche da un altro arco
- se l’algoritmo ha percorso $(u,v)$ significa che $D[x]+p(x,y) \ge D[u]+p(u,v)$
	- da cui segue che $costo(C) \ge D[x]+p(x,y) \ge D[u]+p(u,v) = D[v]$
- abbiamo appena mostrato che il cammino alternativo ha costo maggiore al percorso trovato dall’algoritmo

###### Implementazione
Per rappresentare un grafo pesato avremo dato un arco $(x,y)$ nella lista di adiacenza di $x$ troveremo la coppia $[(y,c)]$ dove $c$ indica il costo
Per l'implementazione useremo il vettore lista che sarà della forma `(definitivo, costo, origine)` dove:
- definitivo: un flag che assume valore $1$ se il costo per raggiungere $x$ è stato definitivamente stabilito ossia se non è possibile ottenere un percorso migliore, (0 indica che il costo per $x$ è in fase di aggiornamento)
- costo: rappresenta il costo corrente minimo noto per raggiungere $x$ dalla sorgente, inizializzato a $+\infty$
- origine: indica il nodo "padre" o "predecessore" lungo il cammino dalla sorgente a $x$, inizializzato a $-1$

all'inizio vi è solo la sorgente dunque la lista è inizializzata a:
$$
lista[x] = \begin{cases}
(1,0,s) \quad \text{se } x=s\\
(0,costo,s) \quad \text{ se } (costo,x) \in G[s] \\
(0, +\infty, -1) \quad \text{altrimenti}
\end{cases}
$$
seguono una serie di iterazioni dove:
- nodo minimo non definitivo: 
	- si scorre la lista di adiacenza di $x$ per individuare il nodo con costo minimo non definitivo (flag = 0), che sarà il candidato per il quale il cammino minimo della sorgente è noto
- verifica di terminazione:
	- se il costo minimo trovato è $+ \infty$ significa che non esistono altri nodi raggiungibili non ancora definitivi, il ciclo si interrompe
- flaggare $x$ come definitivo:
	- il nodo $x$ trovato viene aggiornato impostando il flag a $1$ e impostando il suo costo come definitivo
- aggiorniamo i vicini di $x$:
	- per ogni nodo $y$ adiacente a $x$, se $x$ non è ancora definitivo è $costo_{x}+costo_{(x+y)} <$ costo memorizzato di $y$, aggiorniamo il costo di $y$

```python 
def dijkstra(s, G):
	#restituisce il vettore delle distanze  D e
	#l'albero dei cammini minimi rappresentato tramite il vettore dei padrio P
	n = len(G)
	Lista = [(0, float('inf'), -1)] * n #(non def, costo = inf, sorgente = -1)
	Lista[s] = (1, 0, s)
	for y, costo in G[s]:
		Lista[y] = (0, costo, s)
	while True:
		minimo, x = float('inf'), -1
		for i in range(n): #trovo il nodo non definitvo con costo minimo
			if Lista[i][0] == 0 and Lista[i][1] < minimo:
				minimo, x = Lista[i][1], i
		if minimo == float('inf'):
			break #non ci sono nodi raggiungibili non definitivi
		definitivo, costo_x, origine = Lista[x] #rende x definitivo
		Lista[x] = (1, costo_x, origine)
		for y, costo_arco in G[x]: #aggiornamento vicini di x non definitivi
			if Lista[y][0] == 0 and minimo + costo_arco < Lista[y][1]:
				Lista[y] = (0, minimo + costo_arco, x) #controllo costo
	#estrae i vettori delle distanze e dei padri 
	D,P = [costo for _, costo, _ in lista], [origine for _, _, origine in Lista]
	return D,P
```
costo:
- fuori dal `while` $\theta(n)$
- il `while` lo eseguiamo al più $n-1$ volte:
	- primo `for` eseguito esattamente $n$ volte
	- secondo `for` eseguito al più $n$ volte (tante quanti gli adiacenti)
- dunque il `while` costa $\theta(n^2)$ che è la complessità dell'implementazione
- ottima nei grafi densi dove $m = \theta(n^2)$


se si potesse evitare di scorrere ogni volta il vettore Lista alla ricerca del minimo:
- eviteremmo di pagare $\theta(n)$ ad ogni iterazione del `while`
- realizzabile sostituendo Lista con un heap minimo che ci fornisce il minimo in tempo logaritmico
- risolvendo il problema in $O((n+m)\log n)$
	- se il grafo è denso $\Rightarrow O(n^2\log n)$
```python 
from heapq import heappush, heappop
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
esiste anche una terza implementazione con l'heap di Fibonacci con costo di $O(m+n \log n)$

## Minimo albero di copertura
Il problema del minimo albero di copertura dato un grafo è:
- l'albero al suo interno (grafo connesso aciclico)
- che copre l'intero grafo
- la cui somma dei costi dei suoi archi sia minima

l'algoritmo di _Kruskal_ dato un grafo $G$ risolve questo problema:
- partendo dal grafo $T$ contenente tutti i nodi di $G$ ma nessun suo arco
- si considerano tutti gli archi in ordine crescente
- se l'arco forma un ciclo in $T$ con archi già presi non lo prendiamo
- restituiamo $T$ al termine
rientra nel paradigma della tecnica greedy
![[dir/dir algoritmi/asset/file 33.png|450]]![[dir/dir algoritmi/asset/file 34.png|450]]
##### Prova di correttezza
Dobbiamo dimostrare che al termine dell'algoritmo $T$ è un albero di copertura e che non c'è altro albero che costa meno

mostriamo che è un albero di copertura con costo minore:
- tra tutti gli archi di copertura di costo minimo per $G$ prendiamo quello che differisce meno da $T$, e lo chiamiamo $T^*$
- supponiamo ora per assurdo che $T$ differisca da $T^*$
- considerando gli archi $e_{1},e_{2}\dots$ nello stesso ordine in cui sono stati presi nel corso dell'algoritmo, sia $e$ il primo arco preso che non compare in $T^*$, questo significa che se lo inserissimo in $T^*$ otterremmo un ciclo $C$
- allora il ciclo contiene almeno un arco $e'$ che non appartiene a $T$, altrimenti tutti gli archi di $C$ sarebbero in $T$
- consideriamo ora l'albero $T'$ che si ottiene aggiungendo l'arco $e$ e rimuovendo l'arco $e'$ a $T^*$, il suo costo non può aumentare rispetto a quello di $T^*$ perché $costo(e)\le costo(e')$
- allora $T'$ è un altro albero di copertura ottimo che differisce da $T$ in meno archi di quanto faccia $T^*$ il che contraddice l'ipotesi che $T^*$ differisce da $T$ nel minor numero di archi

##### Implementazione
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
costo:
- sort esterno al for costa $O(m\log m)=O(m\log n^2)=O(m\log n)$
- il for viene iterato $m$ volte, e il codice di connessi costa $O(n)$ quindi $O(m\cdot n)$
- $totale =O(m\cdot n)$ 

## Union e Find
Possiamo migliorare l'algoritmo, ricorrendo alla struttura dati **UNION-FIND**:
- struttura dati per la collezione $C$ delle componenti connesse di un grafo di $n$ nodi
- permette di testare efficientemente se due nodi appartengono o meno alla stessa componente connessa
- così riduciamo la complessità a $O(m\log n)$
- ha le operazioni:
	- `UNION(a,b,C)`: unisce due componenti connesse `a` e `b` in `C` in tempo $O(1)$
	- `FIND(x,C)`: trova in `C` la componente connessa in cui si trova il nodo `x` in $O(\log n)$
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
- il sort costa sempre $O(m\log n)$
- il for lo iteriamo $m$ volte e al suo interno:
    - l’estrazione dell’arco minimo richiede $\theta(1)$
    - il FIND costa $O(\log n)$
    - l'UNION costa $\theta(1)$
- quindi abbiamo che il for costa $O(m \log n)+ O(m\log n)$ per il sort e quindi il totale è $O(m\log n)$

[[Struttura dati per insiemi disgiunti]]

## Cammini con pesi negativi