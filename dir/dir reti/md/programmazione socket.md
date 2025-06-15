copy-paste appunti alessio :P
### Programmazione Socket TCP

- Socket - Un ingresso tra il processo di un’applicazione e il protocollo di trasporto end - to - end (UDP o TCP)
- Servizio TCP - Trasferimento affidabile di byte da un processo all’altro

Si parla di flusso di byte ovvero una sequenza di byte che fluisce verso o da un processo:

- Flusso d’ingresso se viene dall’esterno (socket) e fluisce nel processo
- Flusso di uscita se dal processo va verso l’esterno

Per permettere la connessione:

- Il server deve essere sempre in esecuzione e aver creato un socket che “accoglie” il client
- Il client contatta il server creando un socket TCP, specificando il suo indirizzo IP e il numero di porta del processo che sta usando.
- Creato il socket è stata stabilita una connessione con il server.

Una volta che il server ha accettato il client, deve creare un nuovo socket e spostare li la connessione, in questo modo può accettare più connessione.

Quindi per aprire la connessione abbiamo:

![](https://alem1105.github.io/Quartz/vault/Primo-Anno/Primo-Semestre/Immagini/Pasted-image-20250407112103.png)

Una volta stabilita, si libera il socket di “benvenuto” e se ne crea uno nuovo:

![](https://alem1105.github.io/Quartz/vault/Primo-Anno/Primo-Semestre/Immagini/Pasted-image-20250407112128.png)

La comunicazione avviene quindi in questo modo:

![](https://alem1105.github.io/Quartz/vault/Primo-Anno/Primo-Semestre/Immagini/Pasted-image-20250407112239.png)

> Esempio Applicazione
> 
> _Esempio di applicazione_
> 
> 1. Il client legge una riga dall’input dell’utente e la invia al server tramite >la socket
> 2. Il server legge la riga dalla socket
> 3. Il server converte la riga in maiuscolo e la invia al client
> 4. Il client legge la riga nella socket e la visualizza

---

In Java tramite il package `java.net` abbiamo a disposizione interfacce e classi per l’implementazione di applicazioni di rete:

- `Socket` `ServerSocket` per connessioni TCP
- `DatagramSocket` per connessioni UDP
- `URL` per HTTP
- `InetAddress` per rappresentare indirizzi
- `URLConnection` per rappresentare connessioni ad un URL

_Scriviamo un applicazione per comunicare TCP in Java_

- Scriviamo il Client:

```
importa java.io.*;
importa java.net.*;
 
class TCPClient {
	public static void main(String argv[]) throws Exception {
		String sentence;
		String modifiedSentence;
 
		// Crea Flusso di ingresso
		BufferedReader inFromUser = new BufferedReader(new InputStreamReader(System.in));
		// Crea un socket client connesso al server
		Socket clientSocket = new Socket("hostname", 6789);
		// Crea un flusso di uscita collegato al socket
		DataOutputStream outToServer = new DataOutputStream(clientSocket.getOutputStream());
		// Crea un flusso di ingresso collegato al socket
		BufferedReader inFromServer = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
		
		System.out.print("Inserisci una frase: ");
		sentence = inFromUser.readLine();
 
		// Invia la frase inserita al server
		outToServer.writeBytes(sentence + '\n');
		// Legge la risposta dal server
		modifiedSentence = inFromServer.readLine();
		System.out.println("FROM SERVER: " + modifiedSentence);
		// Chiude la connessione
		clientSocket.close();
	}
}
```

- Scriviamo il Server:

```
import java.io.*;
import java.net.*;
class TCPServer {
public static void main(String argv[]) throws Exception
{
	String clientSentence;
	String capitalizedSentence;
	// Crea Socket di benvenuto
	ServerSocket welcomeSocket = new ServerSocket(6789);
	while(true) {
	// Attende sul benvenuto un client
		Socket connectionSocket = welcomeSocket.accept();
	// Crea flusso di ingresso collegato al socket
		BufferedReader inFromClient =
			new BufferedReader(new
			InputStreamReader(connectionSocket.getInputStream()));
	// Crea flusso di uscita collegato al socket
		DataOutputStream outToClient =
		new DataOutputStream(connectionSocket.getOutputStream());
	// Legge la riga dal socket
		clientSentence = inFromClient.readLine();
		capitalizedSentence = clientSentence.toUpperCase() + '\n';
	// Scrive la riga sul socket
		outToClient.writeBytes(capitalizedSentence);
	}
}
}
```

### Programmazione Socket UDP

In UDP non c’è connessione tra client e server, quindi:

- Non c’è handshaking
- Il mittente allega esplicitamente a ogni pacchetto l’indirizzo IP e la porta di destinazione
- Il server deve estrarre l’indirizzo IP e la porta del mittente del pacchetto ricevuto

Inoltre in questo tipo di connessione i pacchetti potrebbero arrivare in ordine diverso rispetto a quello di invio. Trasferiamo “datagrammi” in modo inaffidabile.

La comunicazione avviene in questo modo:

![](https://alem1105.github.io/Quartz/vault/Primo-Anno/Primo-Semestre/Immagini/Pasted-image-20250407121448.png)

- Scriviamo il client in Java

```
import java.io.*;
import java.net.*;
 
class UDPClient {
	public static void main(String args[]) throws Exception
	{
// Crea flusso di ingresso
	BufferedReader inFromUser =
		new BufferedReader(new InputStreamReader(System.in));
// Crea socket client
	DatagramSocket clientSocket = new DatagramSocket();
// Traduce il nome dell'host in indirizzo IP con DNS
	InetAddress IPAddress = InetAddress.getByName("hostname");
	
	byte[] sendData = new byte[1024];
	byte[] receiveData = new byte[1024];
	String sentence = inFromUser.readLine();
	sendData = sentence.getBytes();
// Crea il datagramma con dati, lunghezza, IP, porta
	DatagramPacket sendPacket =
		new DatagramPacket(sendData, sendData.length, IPAddress, 9876);
// Invia il datagramma al server
	clientSocket.send(sendPacket);
	DatagramPacket receivePacket =
		new DatagramPacket(receiveData, receiveData.length);
// Legge il datagramma ricevuto dal server
	clientSocket.receive(receivePacket);
	
	String modifiedSentence =
		new String(receivePacket.getData());
	System.out.println("FROM SERVER:" + modifiedSentence);
	clientSocket.close();
	}
}
```

- Scriviamo il server

```
import java.io.*;
import java.net.*;
class UDPServer {
	public static void main(String args[]) throws Exception
	{
	// Crea socket per datagramma sulla porta 9876
		DatagramSocket serverSocket = new DatagramSocket(9876);
		byte[] receiveData = new byte[1024];
		byte[] sendData = new byte[1024];
		while(true)
		{
		// Crea spazio per i datagrammi
			DatagramPacket receivePacket =
			new DatagramPacket(receiveData, receiveData.length);
		// Riceve i datagrammi
			serverSocket.receive(receivePacket);
			String sentence = new String(receivePacket.getData());
		// Ottiene l' indirizzo IP e il numero di porta dal mittente
			InetAddress IPAddress = receivePacket.getAddress();
			int port = receivePacket.getPort();
			String capitalizedSentence = sentence.toUpperCase();
			sendData = capitalizedSentence.getBytes();
		// Crea il datagramma da inviare al client
			DatagramPacket sendPacket =
			new DatagramPacket(sendData, sendData.length, IPAddress,port);
		// Scrive il datagramma sulla socket
			serverSocket.send(sendPacket);
		} // Fine while
	}
}
```