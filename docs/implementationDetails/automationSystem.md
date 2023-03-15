---
title: Dettagli implementativi sistema di automazione
parent: Dettagli implementativi
has_children: false
nav_order: 1
---

# Dettagli implementativi sistema di automazione
Il Sistema di automazione, come detto in precedenza è costituito da due parti principali, una componente Arduino che racchiude la scheda Arduino Uno, i sensori e gli attuatori presenti nella serra e una componente ESP, che invece comprende la scheda NodeMCU, utilizzata per inviare i dati in rete.

## Collegamento ad Arduino Cloud
Quando si crea una nuova "_Thing_" su Arduino Cloud e vi si associa a questa uno _skatch_, in automatico la piattaforma inserisce all'interno del progetto due file:

- `thingProperties.h`, contenente le diverse varibili Cloud dichiarate per la "_Thing_" e che si occupa dell'instaurazione della connessione al Cloud;
- `secret.h`, il quale contiene le diverse informazioni relative alla connessione di rete e alla chiave del dispositivo, necessarie per la connessione.

L'instaurazione della connessione effettiva alla piattaforma Cloud, avviene all'intenro del programma principale del progetto individuato dal file `.ino`. 

In particolare, all'interno del metodo `setup` del programma, come possibile vedere dal <a href="#lst1">listato 1</a>, avviene l'inizializzazione del Serial Monitor e delle proprietà utilizzando il metodo `initProperties`, la connessione ad Arduino Cloud viene inizializzata attraverso il metodo `ArduinoCloud.begin`, il quale utilizza funzioni che sono presenti all'interno delle librerie `ArduinoIoTCloud` e `Arduino_ConnectionHandler`, incluse all'interno del file `thingProperties.h`. Infine, gli altri due metodi richiamati `setDebugMessageLevel` e `ArduinoCloud.printDebugInfo`, vengono utilizzati per funzionalità di debug, essi infatti, si occupano di stampare le infromazioni relative allo stato della rete e alla connessione ad Arduino Cloud.

```c++
void setup() {
  // Initialize serial and wait for port to open:
  Serial.begin(9600);
  // This delay gives the chance to wait for a Serial Monitor without blocking if none is found
  delay(1500);

  // Defined in thingProperties.h
  initProperties();

  //...

  // Connect to Arduino IoT Cloud
  ArduinoCloud.begin(ArduinoIoTPreferredConnection);

  /*
     The following function allows you to obtain more information
     related to the state of network and IoT Cloud connection and errors
     the higher number the more granular information you’ll get.
     The default is 0 (only errors).
     Maximum is 4
  */
  setDebugMessageLevel(2);
  ArduinoCloud.printDebugInfo();
}
```
<p align="center" id="lst2">[Listato 1] Connessione al Cloud</p>

All'interno del metodo loop (<a href="#lst2">listato 1</a>), invece, avviene la chiamata al metodo `ArduinoCloud.update`, il quale si occupa di mantenere aggiornati tutti i dviersi parametri del Cloud, in relazione alle modifiche e aggiornamenti che possono avvenire.

```c++
void loop() {

  ArduinoCloud.update();
  //...
}
```
<p align="center" id="lst2">[Listato 2] Aggiornamento valori Cloud</p>




## Comunicazione tramite il protocollo MQTT
Per far comunicare fra loro il sistema di automazione con il resto del sistema di backend, in particolare con il micro-servizio ``GreenhouseCommunication``,  si è deciso di adottare il protocollo MQTT. 

Nello specifico MQTT, è un protocollo per lo scambio di messaggi di tipo publish/subscribe, pensato per poter inviare e ricevere i dati in modo accurato nonostante i ritardi della rete e la larghezza di banda ridotta. In questo tipo di protocollo si distinguono due ruoli principali che i processi possono ricoprire, il ruolo di _publisher_: cioè di colui che pubblica i messaggi relativamente a un certo topic, e quello di _subscriber_: che invece rappresenta colui che è interessato a ricevere i messaggi per un determinato argomento. 

MQTT è un protocollo asincrono: il _publisher_ pubblica i messaggi indipendentemente dal fatto che vi siano dei _subscribers_ interessati a riceverli e iscritti all'argomento. A regolare l'interazione fra _publisher_ e _subscriber_ viene utilizzato un _Message-Broker_, il quale si occupa di raccogliere i messaggi pubblicati dai _publishers_ e di inoltrarli ai _subscribers_, interessati a riceverli, come possibile vedere nella seguente <a href="fig1">figura 1</a>.

<div align="center">
<img src="img/architettura-MQTT.jpg" alt="Architettura MQTT" width="80%" id="fig1">
<p align="center">[Fig 1] Architettura MQTT</p>
</div>

Per il progetto, la componente ESP del sistema e il micro-servizio ``GreenhouseCommunication`` sono entrambi sia _publisher_ che _subscriber_; nello specifico:

- **ESP**, si occupa di pubblicare i dati relativi ai sensori tramite il topic dataSG ed è interessata a ricevere i messaggi relativi alle operazioni che il sistema deve compiere, quindi effettuerà la sottoscrizione ai topic: ``LUMINOSITY``, ``VENTILATION``, ``TEMPERATURE`` e ``IRRIGATION``;
- **GreenhouseCommunication**, è interessato a ricevere i messaggi relativi al topic dataSG, contenenti le rilevazioni effettuate dai sensori e in più si occupa di comunicare le operazioni da effettuare, tramite la pubblicazione dei messaggi relativi ai seguenti topic: ``LUMINOSITY``, ``VENTILATION``, ``TEMPERATURE`` e ``IRRIGATION``.

## Comunicazione Seriale

Le due componenti del sistema di automazione, comunicano fra loro attraverso la comunicazione seriale.

La comunicazione seriale, consente lo scambio di messaggi fra due dispositivi tramite un unico bus seriale, il quale è costituito da solo due collegamenti, uno per poter inviare i dati e l'altro per poterli ricevere. Di conseguenza, un **device** che supporta la comunicazione seriale dovrebbe avere due serial pin a disposizione: `RX` per poter ricevere  i dati e `TX` per poterli inviare. Per la comunicazione seriale il pin `RX` di un dispositivo deve essere collegato al pin `TX` dell'altro e vice-versa, come possibile vedere in <a href="#fig2">figura 2</a>.

<div align="center">
<img src="img/serial_communication.png" alt="Comunicazione Seriale" width="50%"id="fig2">
<p align="center">[Fig 2] Comunicazione seriale</p>
</div>

Un altro aspetto importante da tenere in considerazione per garantire la corretta comunicazione fra i dispositivi è il _baud-rate_, il quale rappresenta la velocità con cui i dati sono inviati lungo il collegamento seriale, ed entrambi i dispositivi devono comunicare con lo stesso _boud-rate_ per poter ricevere correttamente i dati.

Nel nostro caso, si è deciso di utilizzare un _baud-rate_ di 9600 bps e i pin `TX` e `RX` di Arduino sono stati connessi ai pin `RX` e `TX` dell'ESP e vengono utilizzati per far sì che Arduino possa comunicare i dati rilevati dai sensori all'ESP, che si occuperà di inoltrarli ai micro-servizi incaricati della loro gestione, e all'ESP di poter inviare le operazioni da compiere ad Arduino, le quali possono essere state stabilite dal sistema di gestione della serra o richieste dall'utente stesso.

Nel seguente listato è rappresentato una parte principale del programma dell'ESP che mostra la connessione tramite seriale ad Arduino e l'attesa della ricezione di messaggi per il loro successivo inoltro.

```c++

//...
MsgServiceArduino *msgARD;
Connection *conn;
//...

void setup()
{
    //...
    Serial.begin(9600);
    msgARD = new MsgServiceArduino(RX, TX);
    msgARD->init();
    conn = new Esp8266(SSIDNAME, PWD, MQTT_SERVER, msgARD);
    conn->connecting();

    state = RECEIVE;

}

void loop()
{
    //...
    conn->processIncomingMessages();
    if (msgARD->isMsgAvailable()){
        Msg *message = msgARD->receiveMsg();
        String m = message->getContent();

        //invia dati con mqtt
        conn->sendData("dataSG", m);
        delete message;
    }
    delay(1000);
}
```
<p align="center" id="lst3">[Listato 3] Esempio di connessione e ricezione di un messaggio tramite seriale</p>
