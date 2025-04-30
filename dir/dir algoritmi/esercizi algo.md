### Esercizio slide 13 algoritmi greedy
 - abbiamo $n$ file di dimensioni $d_{0},d_{1},\dots d_{n-1}$, vorremmo memorizzarli su un disco di capacità $k$
 - la somma delle dimensioni di questi file eccede la capacità del disco
 - vogliamo selezionare un sottoinsieme degli $n$ file che abbia:
	 - cardinalità massima
	 - e che possa essere memorizzato sul disco
 - descrivere un algoritmo greedy che risolve il problema in tempo $\theta(n\log n)$
 - provarne la correttezza

esempio:
- $D= [5,6,3,5,4,7,3]$
- $k = 11$
- la risposta sarà la tripla di file in posizione $2,4,6$ quindi ${3,4,3}$ che occupano spazio 10

possibile soluzione:
- consideriamo i file per dimensione crescente e se c'è spazio per memorizzare il file su disco allora lo facciamo
codice:
```python
def file(D,k):
	n = len(D)
	lista = [((D[i]), i) for i in range (n)] # per salvare la posizione
	lista.sort()
	spazio, sol = 0, []

	for d, i in lista:
		if spazio + d <= k:
			sol.append(i)
			spazio += d
		else:
			return sol
```

la complessità è $\theta(n\log n)$

**prova**:
- assumiamo per assurdo che la soluzione $sol$ prodotta dal greedy non sia ottima
- devono quindi esistere insiemi con più file di $sol$ che rispettano la capacità del disco
- sia $sol^*$ l'insieme con più elementi in comune con $sol$, notiamo che:
	- esiste un file $a$ che appartiene a $sol^*$ e non a $sol$ e che occupa più spazio di qualunque file in $sol$ (tutti gli elem in $sol$ occupano meno spazio di quelli non presenti in $sol$)
	- esiste un file $b$ che appartiene a $sol$ e non a $sol^*$, dato che $sol \not\subset sol^*$ (l'aggiunta di un qualunque elemento in $sol$ porterebbe a superare la capacità del disco)
- posso dunque eliminare da $sol^*$ $a$ e inserire $b$ ottenendo un nuovo insieme di file che rispetta ancora le capacità del disco ed ha un elemento in più in comune con $sol$ contraddicendo le nostra ipotesi ossia
	- che la soluzione $sol^*$ è quella con più elementi in comune con $sol$