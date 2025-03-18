#### Esercizio 1
- modificare l'`umask` in modo che, sia venga creata una directory che un file i permessi siano `664`
	- `umask 133`, teoricamente ma non ho capito perché, ma `umask 100` influisce solo sulle dir
- modificarlo per far si che sia `775` per le directory e `664` per i file
	- `umask 002` (`777-2=775`, `666-2=664`)

#### Esercizio 2
- creare un file e guardare il suo inode
- cancellarlo
- creare un altro file
- quale sarà l'inode nel nuovo file?

##### Esercizio 3
- Cosa succede se rimuovo il sorgente di un soft link?
- Cosa succede se rimuovo il sorgente di un hard link?

- in quali casi touch non riesce a creare un file?

#### Esercizio 4
- creare un file.txt con il seguente contenuto:
	"""
	ciao1
	addio2
	via3
	ehila4
	dove vai5
	"""
- copiarlo in un altro file in modo che contenga solo i bytes che vanno dal $10°$ al $20°$, usare sia $1$ che $10$ come valori per `bs`
- copiare come sopra ma facendo in modo che sia duplicato, il contenuto sia presente 2 volte consecutive

