# Keen - bold to be expressive 

![Group 4 (3)](https://user-images.githubusercontent.com/117916839/217205456-e2861a56-be53-45af-9abc-d13c752f72bb.png)

# Contesto
Sono passati 4 anni da quando è scoppiato il virus del Covid-19. Pian piano il mondo si sta riprendendo la propria libertà e la propria autonomia che gli erano stati privati durante gli anni della pandemia.                                                                           
Ma gli effetti negativi del lockdown persistono ancora tutt'oggi, come il distanziamento sociale che ha acuito la diffidenza e la sfiducia 
verso gli altri. Molti hanno perso la propria capacità di esprimersi o rapportarsi con gli altri.

# Soluzione 
Keen propone un software in grado di riconoscere le espressioni facciali con una connessione neurale profonda. 
Il progetto mira a dare una maggior consapevolezza delle nostre emozioni e di quelle degli altri. 

![screenshot68 copia](https://user-images.githubusercontent.com/117916839/217205978-c7d22ec0-a5f1-48e2-ab47-4f9d90c6c9d6.jpg)

# Installazione
Attualmente FER supporta solo Python 3.6 in poi. Può essere installato tramite pip:
``` 
$ pip install fer
```
Questa implementazione richiede OpenCV>=3.2 e Tensorflow>=1.7.0 installati nel sistema, con binding per Python3.

Possono essere installati tramite pip (se versione pip >= 9.0.1):
```
$ pip install tensorflow>=1.7 opencv-contrib-python==3.3.0.9
```
o compilato direttamente dai sorgenti ( [OpenCV3](https://github.com/justinshenk/fer#:~:text=direttamente%20dai%20sorgenti%20(-,OpenCV3,-%2C%20Tensorflow%20).) , [Tensorflow](https://www.tensorflow.org/install/install_sources)).

Si noti che è possibile utilizzare una versione tensorflow-gpu se nel sistema è disponibile un dispositivo GPU, che velocizzerà i risultati. Può essere installato con pip:
```
$ pip install tensorflow-gpu\>=1.7.0
```
Per estrarre video che includono audio, i pacchetti ffmpeg e moviepy devono essere installati con pip:
```
$ pip install ffmpeg moviepy
```

# Utilizzo

(Le istruzioni si susseguono uno dopo l'altro)

Utilizza OpenCV, una libreria utilizzata per il processing delle immagini, e la libreria FER ( Facial Expression Recognition ) che è utilizzata per rilevare le emozioni dell'utente e
visualizzare i punteggi emotivi.                                                              
# Importo delle librerie
Inoltre , importa anche le librerie "os" per l'interazione con il sistema operativo, "random" per generare numeri casuali, "time" per gestire i tempi di esecuzione e "numpy" per lavorare con array multidimensionali.

```
# import the opencv library
from fer import FER
import cv2
import os
import random
import time
import numpy as np
```

1. **Processing delle immagini**

Definisce quindi un percorso per una cartella contenente immagini e crea una lista vuota per archiviare quelle immagini. Il programma quindi ottiene un elenco di tutti i file nella cartella e controlla se ogni file è un file immagine (JPG, PNG, BMP). Se il file è un'immagine, viene aperto utilizzando OpenCV e aggiunto alla lista delle immagini dello screensaver. 
Lo script seleziona quindi un'immagine casuale dalla lista e la visualizza come screensaver.
```
# Define the path to the folder containing the images
file_path = '/Users/gianc00/Documents/keen/Screensavers'

# Create an empty list to store the images
screen_saver_images = []

# Get a list of all files in the folder
files = os.listdir(file_path)
print(files)

# Iterate through the list of files
for file in files:
   # Get the file extension
   extension = os.path.splitext(file)[1]
   # Check if the file is an image file (JPG, PNG, BMP)
   if extension in [".jpg"]:
      # Open the image using OpenCV
      img = cv2.imread(os.path.join(file_path, file))
      screen_saver_images.append(img)

screen_saver_image = cv2.imread(os.path.join(file_path,random.choice(files)))
```
2. **Libreria FER**                                                                            
Descrizione generale                                                                          
Lo script cattura inoltre i fotogrammi video e utilizza la libreria FER per rilevare le emozioni nel fotogramma. Se viene premuto il tasto spazio, lo script attiva o disattiva lo screensaver e registra le emozioni facciali nel fotogramma. Se si preme il tasto 'q', il codice terminerà, se si preme il tasto spazio, il codice attiverà o disattiverà lo screensaver e registrerà le emozioni facciali nel fotogramma.

2.1. **Ciclo "While"**

Il codice utilizza un ciclo infinito "while" per catturare continuamente i frame video dalla fotocamera usando il cv2.VideoCapture(0) dove 0 indica l'utilizzo della webcam predefinita.

Viene poi creata una variabile "screen_saver" che verifica se lo screensaver deve essere attivato. Viene controllato l'input dell'utente per la pressione dei tasti 'q' per uscire dal programma e lo spazio per attivare o disattivare lo screensaver e iniziare a registrare le emozioni del viso nella frame.                                     

2.1.1. **Se il flag dello screensaver è true**

Se il flag dello screensaver è impostato su true (che significa che lo screensaver sta attualmente registrando), lo script controlla se il contatore ha raggiunto i 5 secondi. 
In caso affermativo, seleziona un'immagine casuale dall'elenco screen_saver_images e la visualizza utilizzando la funzione cv2.imshow(). 
Se il contatore non ha raggiunto i 5 secondi, lo script incrementa il contatore e rimane inattivo per 1 secondo. 

```
# define a video capture object
vid = cv2.VideoCapture(0)
i=0

# Create a flag to check if the screen saver should be activated
screen_saver = True

# Create a counter to keep track of the time passed
counter = 0

#caricodetector
detector = FER()

font=cv2.FONT_HERSHEY_SIMPLEX

record = False
on=0

while(True):
	
   # Capture the video frame
   # by frame

   ret, frame = vid.read()
    k = cv2.waitKey(1)
	
    # Display the resulting frame
    #cv2.imshow("frame",frame)

    if k & 0xFF == ord('q'):
	break

    if k%256 == 32:
	on=on+1
	if on%2 == 1:
	   record = True
           screen_saver = False
	   counter = 0 # Reset the counter
        else:
            record = False
            screen_saver = True


     if screen_saver:
         if counter == 5:
	      # Display a random image from the screen saver images list
	      screen_saver_image = cv2.imread(os.path.join(file_path,random.choice(files)))
              counter = 0 # Reset the counter
	 else:
	      counter += 1 # Increment the counter
	      time.sleep(1) # Sleep for 1 second
		
	 cv2.imshow('frame', screen_saver_image)
```
2.1.2. **Se il flag record è true**

Se il flag record è impostato su true (che significa che il video sta attualmente registrando), lo script utilizza la funzione "detect_emotions" dell'oggetto "detector" per rilevare le emozioni nel frame corrente. 
Dopo che le emozioni sono state rilevate, vengono estratte le coordinate della bounding box della persona e i punteggi per le diverse emozioni (angry, disgusted, fear, happy, sad, surprise e neutral).
```
if record:
     dati = detector.detect_emotions(frame)
     if len(dati) > 0:
         xB = dati[0]['box'][0]
         yB = dati[0]['box'][1]
	 wB = dati[0]['box'][2]
	 hB = dati[0]['box'][3]
			
         angryScore = dati[0]['emotions']['angry']
	 disgustScore = dati[0]['emotions']['disgust']
	 fearScore = dati[0]['emotions']['fear']
	 happyScore = dati[0]['emotions']['happy']
	 sadScore = dati[0]['emotions']['sad']
	 surpriseScore = dati[0]['emotions']['surprise']
         neutralScore = dati[0]['emotions']['neutral']
```
Il codice crea poi delle stringhe per ogni emozione con il punteggio formattato come "emozione: punteggio" e le scrive sull'immagine utilizzando la funzione cv2.putText(). 
Inoltre, utilizza cv2.rectangle() per disegnare una barra di lunghezza proporzionale al punteggio dell'emozione accanto al testo scritto. 
Infine, il codice utilizza cv2.addWeighted() per sovrapporre un'immagine nera trasparente sulla regione sottostante la bounding box, creando un effetto di sfocatura.
```
angryScore = dati[0]['emotions']['angry']
disgustScore = dati[0]['emotions']['disgust']
fearScore = dati[0]['emotions']['fear']
happyScore = dati[0]['emotions']['happy']
sadScore = dati[0]['emotions']['sad']
surpriseScore = dati[0]['emotions']['surprise']
neutralScore = dati[0]['emotions']['neutral']

angryString = "angry: {0:.2f}".format(angryScore)
disgustString = "disgust: {0:.2f}".format(disgustScore)
fearString = "fear:{0:.2f}".format(fearScore)
happyString = "happy:{0:.2f}".format(happyScore)
sadString = "sad:{0:.2f}".format(sadScore)
surpriseString = "surprise:{0:.2f}".format(surpriseScore)
neutralString = "neutral:{0:.2f}".format(neutralScore)
			
x1 = xB+wB-10
x2 = xB+wB+250
y1 = yB-40
y2 = yB+400

if y1 < 1:
y1 = 1
# crop undercolor region of input
sub = frame[y1:y2, x1:x2]

# create black image same size
black = np.zeros_like(sub)

# blend the two
blend = cv2.addWeighted(sub, 0.75, black, 0.50, 0)
			
frame[y1:y2, x1:x2] = blend


# write result to disk
# cv2.imwrite(frame, blend )

cv2.putText(frame,angryString,(xB+wB+5,yB),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+10),(round((xB+wB+10+150*float(angryScore))),yB+14),(255,255,255),3)
			
cv2.putText(frame,disgustString,(xB+wB+5,yB+60),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+70),(round((xB+wB+10+150*float(disgustScore))),yB+74),(255,255,255),3)

cv2.putText(frame,fearString,(xB+wB+5,yB+120),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+130),(round((xB+wB+10+150*float(fearScore))),yB+134),(255,255,255),3)

cv2.putText(frame,happyString,(xB+wB+5,yB+180),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+190),(round((xB+wB+10+150*float(happyScore))),yB+194),(255,255,255),3)

cv2.putText(frame,sadString,(xB+wB+5,yB+240),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+250),(round((xB+wB+10+150*float(sadScore))),yB+254),(255,255,255),3)

cv2.putText(frame,surpriseString,(xB+wB+5,yB+300),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+310),(round((xB+wB+10+150*float(surpriseScore))),yB+314),(255,255,255),3)

cv2.putText(frame,neutralString,(xB+wB+5,yB+360),font,1,(255,255,255),2,cv2.LINE_AA)
cv2.rectangle(frame,(xB+wB+5,yB+370),(round((xB+wB+10+150*float(neutralScore))),yB+374),(255,255,255),3)

cv2.rectangle(frame,(xB,yB),(xB+wB,yB+hB),(0,255,0),5)
```
2.1.3 **Screenshot**

Utilizza la libreria OpenCV (cv2) per visualizzare il frame corrente della videocamera e disegnare un rettangolo intorno all'area rilevata come un volto. Quando l'utente preme il tasto 'r', viene letta la variabile "indice" da un file di testo chiamato "indice.txt". Il nome del file dello screenshot viene quindi impostato come "screenshot + indice + .jpg" e viene salvato nella cartella specificata da "file_path". Il valore dell'indice viene quindi incrementato di 1 e scritto nuovamente nel file "indice.txt". Viene quindi visualizzato un messaggio "Screenshot salvato" sullo schermo. Infine, viene mostrato l'ultimo frame della videocamera con il rettangolo intorno al volto e la scritta "Screenshot saved".
```
	# Check if the user pressed the 'r' key
	if k == ord('r'):
	index_file = open("index.txt", "r")
	index = index_file.read()
	index_file.close()
	print(index)
	# Save the screenshot 

	file_name = "screenshot"+index+".jpg"

	j = int(index)+1
	index_file = open("index.txt", "w")
	index_file.write(str(j))
	index_file.close()

	cv2.imwrite(os.path.join(file_path, file_name), frame)

	# Display the "Screenshot saved" message
	cv2.putText(frame, "Screenshot saved",(int(frame.shape[1]/2), int(frame.shape[0]/2)),font, 1,(255,255,255), 2,cv2.LINE_AA)

cv2.imshow('frame', frame)
```
2.2. **Fine**

Lo script continua quindi a eseguire il ciclo, catturando ed elaborando i frame, finché l'utente non preme il tasto "q" per uscire.
```
# After the loop release the cap object
vid.release()

# Destroy all the windows
cv2.destroyAllWindows()
```

# Credits
[Justin Shenk's Github](https://github.com/justinshenk/fer)
[Chat Gbt](https://openai.com/blog/chatgpt/)
Fablab Torino
Paolo Cavagnolo
Elena Falomo
