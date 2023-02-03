---
title: Dettagli implementativi sistema di backend
parent: Dettagli implementativi
has_children: false
nav_order: 2
---

# Dettagli implementativi sistema di backend
Durante lo sviluppo della parte backend del sistema, come detto nel precedente capitolo,  si è cercato di utilizzare il più possibile un approccio a micro-servizi e di raffinare sempre di più la soluzione adottata, per poter sfruttare al meglio i vantaggi che questo approccio offre. Nelle prossime sezioni verranno descritte le scelte implementative legate all'adozione di tale scelta progettuale.

## Adapters HTTP e MQTT

All'interno del progetto ogni servizio, espone i propri _adapters_ che possono essere utilizzati dagli altri componenti del sistema per poter comunicare con lui. 

Per la componente backend si è scelto di far comunicare tra loro i servizi mediante il protocollo HTTP, mentre la comunicazione tra il micro-servizio `GreenhouseCommunication` e il sistema di automazione avviene mediante l'utilizzo del protocollo MQTT descritto precedentemente.

Al fine di rendere reattivi i servizi e di gestire entrambi i protocolli di comunicazione, si è scelto di adottare il _framework_ ad eventi e organizzato con architettura event-loop **Vert.x**.

La logica di business di Vert.x è eseguita all’interno di uno o più componenti chiamati **Verticle** (vertice). Ogni `Verticle` viene eseguito in maniera concorrente rispetto agli altri, senza alcuna condivisione di uno stato. Ogni servizio dell'applicazione prevede l'utilizzo di `Verticle` incaricati di gestire il servizio stesso. 

Per quanto riguarda l'inizializzazione, all'avvio di ogni servizio si effettua il _setup_ degli _adapters_, grazie all'utilizzo della programmazione asincrona e ai concetti di `Future` e `Promise`, i quali rendono l'operazione di _setup_ non bloccante.

L'`Adapter` che è stato più utilizzato, espone il servizio mediante l'utilizzo del protocollo di comunicazione HTTP. 

L'avvio dell'`Adapter`, come si può osservare dal <a href="#lst1">listato 1</a>, può essere suddiviso in due fasi: la prima, consiste nell'avvio dell'`HttpServer` incaricato della gestione delle richieste in arrivo e delle rispettive risposte in uscita; la seconda, consiste nella definizione delle rotte attraverso le quali i servizi esterni possono richiedere informazioni o far svolgere operazioni al servizio in oggetto. 

Le rotte sono state ideate seguendo le linee guida delle API _REST_, per cui per ogni risorsa manipolata dal servizio sono state definite rotte differenti, ognuna delle quali si occupa di gestire, mediante l'apposito _handler_, una o più delle operazioni CRUD (Create, Read, Update, Delete) che possono essere effettuate su di essa.

```java
public void start() {
        HttpServer server = vertx.createHttpServer();
        Router router = Router.router(vertx);

        router.route().handler(BodyHandler.create());
        try {
            router.get(BASE_PATH).handler(this::handleGetGreenhouse);
            router.put(BASE_PATH).handler(this::handlePutModality);
            router.post(BASE_PATH).handler(this::handlePostSensorData);
            router.get(MODALITY_PATH).handler(this::handleGetModality);
            router.get(PARAM_PATH).handler(this::handleGetParamValues);

        } catch (Exception ex) {
            log("API setup failed - " + ex.toString());
            return;
        }
        server.requestHandler(router).listen(this.port, this.host);
    }
```
<p align="center" id="lst1">[Listato 1] Esempio di setup di un adapter HTTP</p>

Per quanto riguarda l'`Adapter` che gestisce la comunicazione MQTT, come si può osservare nel <a href="#lst2">listato 2</a>, prevede anch'esso due fasi per l'avvio: la prima di connessione al broker MQTT, e la seconda di definizione dei topic gestiti dal servizio con i rispettivi _handler_.

```java
public void setupAdapter(Promise<Void> startPromise) {
        mqttClient = MqttClient.create(this.getVertx());
        mqttClient.connect(port, host, c ->{
            System.out.println("MQTT adapter connected");
            mqttClient.publishHandler(this::handleNewDataReceived)
                    .subscribe(GREENHOUSE_NEWDATA_TOPIC, qos);
        });
        this.getVertx().eventBus().consumer(BRIGHTNESS_OPERATION_TOPIC, this::handleOperationReceived);
        this.getVertx().eventBus().consumer(SOIL_MOISTURE_OPERATION_TOPIC, this::handleOperationReceived);
        this.getVertx().eventBus().consumer(TEMPERATURE_OPERATION_TOPIC, this::handleOperationReceived);
        this.getVertx().eventBus().consumer(AIR_HUMIDITY_OPERATION_TOPIC, this::handleOperationReceived);
```
<p align="center" id="lst1">[Listato 2] Esempio di setup di un adapter MQTT</p>

Gli _adpters_ di ogni servizio, sono stati progettati per essere il più indipendenti possibili gli uni dagli altri, attraverso l'utilizzo della programmazione asincrona e dell'`EventBus` messi a disposizione dal _framework_ Vert.x. 

Nel caso del servizio `GreenhouseCommunication`, infatti, le operazioni da eseguire sul micro-controllore vengo ricevute prima mediante l'`Adapter` HTTP, le quali vengono elaborate del `Model` incaricato della loro gestione, che si occuperà di inviarle tramite l'`EventBus` messo a disposizione da Vert.x all'`Adapter` MQTT ( <a href="#lst2">listato 2</a>), che come previsto, le comunicherà al sistema di automazione tramite il protocollo MQTT.

## Docker e docker Compose

Al fine di rendere il deployment del sistema di backend più semplice, si è deciso di utilizzare **[Docker](https://www.docker.com/)** per poter caricare ed eseguire i diversi micro-servizi. 

In particolare, si è deciso di inserire ogni micro-servizio all'interno di un apposito container Docker. Un container è una __lightweight Virtual Machine__, che è in grado di contenere un'applicazione e il suo ambiente di esecuzione, vengono considerati __lightweight__, in quanto contengono solo lo stretto necessario per poter eseguire l'applicazione e utilizzano le risorse dell'host sul quale vengono istanziati, consentendoci di avere un ambiente di esecuzione isolato per ogni servizio. 

Nel nostro caso il container di ogni micro-servizio, come si può vedere nel <a href="#lst3">listato 3</a>, contiene al suo interno il JDK 11, necessario per l'esecuzione del jar e il .jar contenente il codice, compreso di dipendenze, del servizio che rappresenta. Nell'esempio, il servizio è quello relativo alla gestione del parametro della luminosità, per cui una volta nominato l'immagine ``brigthness_service``, in modo da poterla poi identificare, viene caricato al suo interno il .jar del servizio e non appena questo accade viene lanciata la sua esecuzione. 

Eventualmente, se necessario per il corretto funzionamento del servizio, un container può esporre alcune porte, ad esempio, aggiungendo al ``Dockerfile`` il comando ``EXPOSE 1234``, il container sarà in grado di esporre all'esterno la porta 1234.

 ```dockerfile
FROM openjdk:11.0.16 AS brightness_service
WORKDIR /
ADD build/libs/brightnessService-0.1.0-all.jar brightnessService-0.1.0-all.jar
CMD java -jar brightnessService-0.1.0-all.jar
 ```
<p align="center" id="lst3">[Listato 3] Dockerfile di un micro-servizio</p>

I diversi container sono in grado di comunicare tra loro tramite una rete costruita ad-hoc; per semplicità di gestione la costruzione dei container e della rete è stata realizzata tramite la scrittura di un apposito file ``docker-compose.yaml``. 

All'interno del file, come si può vedere nell'estratto del  <a href="#lst4">listato 4</a>, ogni servizio viene identificato dallo stesso nome usato per la creazione della propria immagine, poi, viene definito attraverso ``container_name`` il nome da utilizzare per effettuare le richieste HTTP al container; dopodichè si passa alla creazione della rete mediante la specificazione delle dipendenze tra container, ad esempio dal listato è possibile notare come i servi che necessitano di un database dipendano dal servizio mongodb. 

Una volta specificate le dipendenze tra i diversi servizi, si procede con la creazione vera e propria del servizio, specificando quale sotto-porgetto dovrà essere utilizzato come contesto di esecuzione e il `Dockerfile` da utilizzare per la creazione della rispettiva immagine. Infine, vengono mappate le porte del container su quelle dell'host, in modo tale da poter effettuare le richieste al servizio.

```yaml
version: "3.9"
services:
  mongo:
    container_name: mongodb
    image: mongo:latest
    restart: always
    ports:
      - 27017:27017
    volumes:
      - ./docker-entrypoint-initdb.d/mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro

  brightness_service:
    container_name: brightness
    depends_on:
      - mongo
    build:
      context: brightnessService
      dockerfile: Dockerfile
    ports:
      - 8893:8893

  ...

  client_communication_gateway:
    container_name: client_communication
    build:
      context: clientCommunicationGateway
      dockerfile: Dockerfile
    extra_hosts:
      - host.docker.internal:host-gateway
    ports:
      - 8890:8890
      - 1234:1234
      - 1235:1235

  greenhouse_service:
    container_name: greenhouse
    depends_on:
      - mongo
      - temperature_service
      - brightness_service
      - soil_moisture_service
      - humidity_service
      - operation_service
      - client_communication_gateway
    build:
      context: greenhouseService
      dockerfile: Dockerfile
    ports:
      - 8889:8889
```
<p align="center" id="lst5">[Listato 4] Estratto del docker-compose utilizzato</p>