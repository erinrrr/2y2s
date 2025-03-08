L'**ordinamento topologico** di un **grafo diretto aciclico (DAG)** è una disposizione lineare dei suoi nodi tale che, per ogni arco $(u,v)$, il nodo $u$ compare **prima** del nodo $v$ nella sequenza.
![[Pasted image 20250308164350.png]]
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
                
    if len(ST) == len(G): return ST #controlliamo che lordinamento contenga tutti i nodi
    return []
```

- costo = $O(n+m)+O(n)+O(m)=O(n+m)$