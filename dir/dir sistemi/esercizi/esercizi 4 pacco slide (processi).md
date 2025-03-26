#### Esercizio 1
- eseguire i 3 comandi seguenti e interromperli con `ctrl + z`:
	- `sleep 60s`
	- `ls -R`
	- `sleep 10s`
- come faccio a riportare in esecuzione in background
- perché continuo a vedere l'output di `ls` dopo averlo riportato in esecuzione

#### Esercizio 2
- fare in modo che `ps` abbia lo stesso output di `top -b -n1`

#### Esercizio 3
- lanciare un processo in foreground
- usando un'altra shell simulare
	- `ctrl+z` 
	- `ctrl+c`
	- `bg` 
- per sapere il PID di un processo in esecuzione su un'altra shell, si può usare il nome del comando, vedere l'opzione `-C` di `ps`

#### Esercizio 4
- verificare di avere almeno 1 GB di spazio, e creare con `dd` un file di 1 GB (tutto di zeri)
- dopo averlo lanciato, mandare al processo di dd il segnale `USR1`
- cosa succede? 
- come lo possiamo interpretare?