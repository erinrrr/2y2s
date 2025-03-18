L'**ordinamento topologico** di un **grafo diretto aciclico (DAG)** è una disposizione lineare dei suoi nodi tale che, per ogni arco $(u,v)$, il nodo $u$ compare **prima** del nodo $v$ nella sequenza.
![[dir algoritmi/asset/file 16.png]]
il fatto di essere un DAG è condizione **necessaria** affinché $G$ possa avere un ordinamento topologico, poiché la presenza di un ciclo non ammette propedeuticità.

- ad essere precisi la condizione di essere DAG è sufficiente perché l'ordinamento topologico esista

>proprietà di un DAG: avere sempre un nodo sorgente

grazie a questa proprietà posso costruire l'ordinamento topologico dei nodi:
- inizio la sequenza dei nodi con una sorgente
- cancello dal grafo la sorgente e le frecce che partono da lui ed ottengo un nuovo DAG
- Itero questo ragionamento finché ordino linearmente tutti i nodi

```python 
def sortTop(G):
	#restituisce un sort topologico ST do G se esiste altrimenti lista vuota
    n = len(G)
    gradoEnt = [0] * n #θ(n)
    for i in range(n): #creaiamo un array che assegna a indx[nodo] il valore di archi entratnti O(m)
        for j in G[i]:
            gradoEnt[j] += 1
    sorgenti = [ i for i in range(len(G)) if gradoEnt[i] == 0 ] #prendiamo i nodi che hanno 0 nell'array (nessun arco entrante) O(n)
    ST = []
    
    while sorgenti: #ordina il tutto O(m)
        u = sorgenti.pop()
        ST.append(u)
        for v in G[u]:
            gradoEnt[v] -= 1
            if gradoEnt[v] == 0:
                sorgenti.append(v)
                
    if len(ST) == len(G): return ST #controlliamo che l'ordinamento contenga tutti i nodi
    return []
```

- costo = $O(n+m)+O(n)+O(m)=O(n+m)$

Possiamo anche usare un algoritmo alternativo che:
- effettua una visita DFS del DAG a partire dal nodo 0
- dopo aver visitato tutti i successori inserisce i nodi in una lista
- restituisce il reverse della lista

![[dir algoritmi/asset/file 17.png]]
In questo caso se effettuiamo una visita sul nodo 0 otteniamo:
- $[2,5,6,0,4,3]$ si parte dal nodo 0 e una volta terminato, continuiamo ad effettuare visite sui nodi rimanente ovvero 3 e 4, di cui verrà restituito il reverse quindi $[3,4,0,6,5,2]$

prova di correttezza, siano $x$ e $y$ due nodi adiacenti possono verificarsi due casi:
- l'arco $(x,y)$ viene attraversato durante la visita, in questo caso banalmente la visita di $y$ finisce prima di quella di $x$, e quindi $y$ finirà nella lista prima di $x$
- l'arco $(x,y)$ **non** viene attraversato durante la visita di $x$ dunque $y$ è stato già visitato e la sua visita è terminata altrimenti vi sarebbe un cammino da $x$ a $y$ e quindi anche un ciclo, ma per definizione il DAG è aciclico, quindi anche in questo caso $y$ finisce nella lista prima di $x$

```python 
def sortTop1(G):
	#sort topologico del DAG G
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
Che ha una complessità di $O(n+m)+O(n)=O(n+m)$