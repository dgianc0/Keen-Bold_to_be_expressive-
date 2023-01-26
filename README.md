# Keen - bold to be expressive
Il coraggio di manifestare i propri sentimenti. 

# Contesto
Sono passati 4 anni da quando è scoppiato il virus del Covid-19. 
Pian piano il mondo si sta riprendendo la propria libertà e la propria autonomia che gli erano stati privati durante gli anni della pandemia. 
Ma gli effetti negativi del lockdown persistono ancora tutt'oggi, come il distanziamento sociale che ha acuito la diffidenza e la sfiducia 
verso gli altri. Molti hanno perso la propria capacità di esprimersi o rapportarsi con gli altri.

# Soluzione 
Keen propone un software in grado di riconoscere le espressioni facciali con una connessione neurale profonda. 
Il progetto mira a dare una maggior consapevolezza delle nostre emozioni e di quelle degli altri.

# Caratteristiche
Utilizza OpenCV, una libreria utilizzata per il processing delle immagini, e la libreria FER ( Facial Expression Recognition ) che è utilizzata per rilevare le emozioni dell'utente e
visualizzare i punteggi emotivi. 

1. Processing delle immagini

Definisce quindi un percorso per una cartella contenente immagini e crea una lista vuota per archiviare quelle immagini. Il programma quindi ottiene un elenco di tutti i file nella cartella e controlla se ogni file è un file immagine (JPG, PNG, BMP). Se il file è un'immagine, viene aperto utilizzando OpenCV e aggiunto alla lista delle immagini dello screensaver. 
Lo script seleziona quindi un'immagine casuale dalla lista e la visualizza come screensaver. 

2. Libreria FER

Lo script cattura inoltre i fotogrammi video e utilizza la libreria FER per rilevare le emozioni nel fotogramma. Se viene premuto il tasto spazio, lo script attiva o disattiva lo screensaver e registra le emozioni facciali nel fotogramma. Se si preme il tasto 'q', il codice terminerà, se si preme il tasto spazio, il codice attiverà o disattiverà lo screensaver e registrerà le emozioni facciali nel fotogramma.

3. Ciclo "While"

Il codice utilizza un ciclo infinito "while" per catturare continuamente i frame video dalla fotocamera usando il cv2.VideoCapture()function. 
All'interno del ciclo while, controlla l'input dell'utente in particolare per il tasto 'q' ( per uscire dal programma ) e il tasto della barra spaziatrice ( per attivare o disattivare lo screen saver e iniziare a registrare le emozioni facciali nella cornice ). 

3.1. Screensaver

Se il flag dello screensaver è impostato su true (che significa che lo screensaver sta attualmente registrando), lo script controlla se il contatore ha raggiunto i 5 secondi. 
In caso affermativo, seleziona un'immagine casuale dall'elenco screen_saver_images e la visualizza utilizzando la funzione cv2.imshow(). 
Se il contatore non ha raggiunto i 5 secondi, lo script incrementa il contatore e rimane inattivo per 1 secondo. 

3.2 Rivelatore di emozioni

Se il flag record è impostato su true (che significa che il video sta attualmente registrando), lo script utilizza la funzione "detect_emotions" dell'oggetto "detector" per rilevare le emozioni nel frame corrente. 
Dopo che le emozioni sono state rilevate, vengono estratte le coordinate della bounding box della persona e i punteggi per le diverse emozioni (angry, disgusted, fear, happy, sad, surprise e neutral).

Il codice crea poi delle stringhe per ogni emozione con il punteggio formattato come "emozione: punteggio" e le scrive sull'immagine utilizzando la funzione cv2.putText(). 
Inoltre, utilizza cv2.rectangle() per disegnare una barra di lunghezza proporzionale al punteggio dell'emozione accanto al testo scritto. 
Infine, il codice utilizza cv2.addWeighted() per sovrapporre un'immagine nera trasparente sulla regione sottostante la bounding box, creando un effetto di sfocatura.

3.3 Screenshot

Utilizza la libreria OpenCV (cv2) per visualizzare il frame corrente della videocamera e disegnare un rettangolo intorno all'area rilevata come un volto. Quando l'utente preme il tasto 'r', viene letta la variabile "indice" da un file di testo chiamato "indice.txt". Il nome del file dello screenshot viene quindi impostato come "screenshot + indice + .jpg" e viene salvato nella cartella specificata da "file_path". Il valore dell'indice viene quindi incrementato di 1 e scritto nuovamente nel file "indice.txt". Viene quindi visualizzato un messaggio "Screenshot salvato" sullo schermo. Infine, viene mostrato l'ultimo frame della videocamera con il rettangolo intorno al volto e la scritta "Screenshot saved".

4. Fine

Lo script continua quindi a eseguire il ciclo, catturando ed elaborando i frame, finché l'utente non preme il tasto "q" per uscire.
