- In un grafo diretto un pozzo è un nodo senza archi uscenti, con un massimo di n pozzi
- mentre un pozzo universale è un pozzo verso cui tutti gli altri nodi hanno un arco, se c'è è unico
![[dir algoritmi/asset/file 8.png]]
$\Omega(n)$ per controllare se c'è un pozzo in quanto devo scorrere tutti i nodi (dimensione dei dati)

- per trovare un pozzo: 
	bisogna trovare (se c'è) la riga della matrice che contiene solo valori 0, quindi non ha archi uscenti
- per trovare il pozzo universale
	bisogna controllare che il nodo sia un pozzo,
	dobbiamo controllare se la colonna di quel nodo abbia tutti valori 1, (tranne su se stesso), ovvero che tutti puntino verso di lui
	 così: ![[dir algoritmi/asset/file 9.png]]

a livello algoritmico per trovarlo potremmo usare un algoritmo che controlla l'intera matrice e quindi ci verrà a costare $O(n^2)$

Ma esiste un algoritmo che verifica se un grado diretto ha un pozzo universale in $\theta(n)$:
- prendiamo i nodi $i,j$ e controlliamo i loro valori:
$$
M[i][j]=\begin{cases}
1 \quad \text{i non é pozzo} \\
0 \quad \text{j non é pozzo universale}
\end{cases}
$$

se la cella vale $1$ allora $i$ ha un nodo verso $j$, avendo un arco uscente non può essere pozzo universale, se troviamo uno $0$ allora $j$ non è un pozzo universale dato che non tutti i nodi hanno un arco verso di lui, però può essere ancora un pozzo.

Utilizziamo un **approccio con due indici**: $i$ e $j$, inizializzati a $i = 0$, $j = 1$ (suppongo $i\ne j$)

| $i$ | $j$ | controllo $M[i][j]$ | Azione                                   |
| --- | --- | ------------------- | ---------------------------------------- |
| 0   | 1   | $M[0][1] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 0   | 2   | $M[0][2] = 1$       | $i$ non può essere pozzo → $i = 2$[^1]   |
| 2   | 3   | $M[2][3] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 4   | $M[2][4] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 5   | $M[2][5] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 6   | $M[2][6] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
| 2   | 7   | $M[2][7] = 0$       | $j$ non può essere pozzo → Avanziamo $j$ |
```python
def pozzoU2(M):
    '''restituisce True se il grafo M ha pozzo universale, False Altrimenti'''
    L=[x for x in range(len(M))]
    while len(L)>1:
        a=L.pop()
        b=L.pop()
        if M[a][b]:
            L.append(b)
        else:
            L.append(a)
    x=L.pop()
    for j in range(len(M)):
        if M[x][j]: return False
    for i in range(len(M)):
        if i!=x and M[i][x]==0: return False
    return True
```

[^1]: (poiché $i=1$ è stato scartato prima)
