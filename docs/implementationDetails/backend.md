---
title: Dettagli implementativi sistema di backend
parent: Dettagli implementativi
has_children: false
nav_order: 2
---
//TODO sistemare i numeri dei listati ! 
# Dettagli implementativi sistema di backend


## Docker e docker Compose
Al fine di rendere il deployment del sistema di backend più semplice, si è deciso di utilizzare **[Docker](https://www.docker.com/)**. In particolare, si è deciso di inserire ogni micro-servizio all'interno di un apposito container Docker. Un container è una __lightweight Virtual Machine__, che è in grado di contenere un'applicazione e il suo ambiente di esecuzione, vengono considerati __lightweight__, in quanto contengono solo lo stretto necessario per poter eseguire l'applicazione e utilizzano le risorse dell'host sul quale vengono istanziati, questo ci consente di avere un ambiente di esecuzione isolato per ogni servizio. 

 ```dockerfile
FROM openjdk:11.0.16 AS brightness_service
WORKDIR /
ADD build/libs/brightnessService-0.1.0-all.jar brightnessService-0.1.0-all.jar
CMD java -jar brightnessService-0.1.0-all.jar
 ```
<p align="center" id="lst4">[Listato 4] Dockerfile di un micro-servizio</p>

Nel nostro caso il container di ogni micro-servizio, come si può vedere nel <a href="#lst4">listato 4</a>, contiene al suo interno il JDK 11, necessario per l'esecuzione del jar, e il jar contenente il codice, compreso di dipendenze, del servizio che rappresenta. Nell'esempio il servizio è quello relativo alla gestione del parametro della luminosità, per cui una volta nominato l'immagine ``brigthness_service``, in modo da poterla poi identificare, viene caricato al suo interno il jar del servizio e non appena questo accade viene lanciata la sua esecuzione. Eventualmente, se necessario per il corretto funzionamento del servizio, un container può esporre alcune porte, aggiungendo al ``Dockerfile`` mostratro ``EXPOSE 1234``, dove 1234 è la porta che si desidera rendere accessibile dall'esterno.

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
<p align="center" id="lst5">[Listato 5] Estratto del docker-compose utilizzato</p>

I diversi container sono in grado di comunicare tra loro tramite una rete costruita ad-hoc; per semplicità di gestione la costruzione dei container e della rete è stata realizzata tramite la scrittura di un apposito file ``docker-compose.yaml``. All'interno del file, come si può vedere in nel piccolo estratto del  <a href="#lst5">listato 5</a> ogni servizio viene identificato dallo stesso nome usato per la creazione della propria immagine, poi, viene definito attraverso ``container_name`` il nome da utilizzare per effettuare le richieste HTTP a quel container; dopo di chè si passa alla creazione della rete, mediante la specificazione delle dipendenze tra container, ad esempio è possibile notare come i servi i quali necessitano di un database dipendano dal servizio mongodb. Una volta specificate quindi le dipendenze tra i servizi si procede con la creazione vera e propria del servizio, specificando quale sotto-porgetto dovrà essere utilizzato come contesto di esecuzione e il dockerfile da utilizzare per la creazione della rispettiva immagine. Infine vengono mappate le porte del container su quelle dell'host, in modo tale da poter effettuare le richieste al servizio.