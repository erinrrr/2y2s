1. creare una directory:
	- `mkdir nome_directory`

2. copiare un file in una directory:
	- `cp file.txt nome_directory/`

3. listare il contenuto di una directory, in modo da visualizzare le informazioni sui file: 
	- `ls -l`

4. listare i processi in esecuzione, e quelli proprietari: 
	- `ps`
	- `ps -u`

5. descrizione comando `ps -p $$ -h`:
	- `ps` → Mostra informazioni sui processi
	- `-p $$` → Mostra solo il processo con **PID del processo attuale** (cioè la shell in esecuzione)
	- `-h` → Rimuove l'intestazione della tabella di output

6. creare due utenti
	- `useradd utente1`
	- `useradd utente2`

7. eseguire il comando `sudo apt-get update` as sudoer
	- - The `apt-get update` command is used to **refresh the package lists** from the repositories. This ensures that the system knows about the latest available versions of software and dependencies.
	- updating the package list **requires administrative (root) privileges**.

