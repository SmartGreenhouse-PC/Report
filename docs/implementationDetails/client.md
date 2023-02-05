---
title: Dettagli implementativi clients
parent: Dettagli implementativi
has_children: false
nav_order: 3
---

# Dettagli implementativi clients
Nella seguente sezione verranno discussi i dettagli implementativi delle due diverse tipologie di clients realizzate: Client Desktop e Client Mobile, di questi alcuni risultano essere comuni e verranno descritti nel seguente paragrafo, mentre altri che risultano essere specifici per la diversa tipologia di Client considerato, verranno descritti in un apposita sottosezione a loro dedicata.

## Socket
Per poter effettuare l'aggiornamento delle pagine dell'applicazione in _real-time_, a seguito della ricezione di un nuovo dato, senza dover inviare continue richieste al Server, ma ricevendo direttamente da lui i dati aggiornati, sia per il Client Desktop che per il Client Mobile si è deciso di utilizzare il meccanismo delle _socket_. 

Questo meccanismo prevede all'interno del backend dell'applicazione la presenza di una _server socket_ incaricata di accettare le connessioni dei clients e inviare a loro un messaggio non appena avviene un determinato evento, che nel nostro caso è rappresentato dalla rilevazione dei nuovi parametri e dall'esecuzione di una nuova operazione. Il _client socket_ è invece posizionato all'interno di ciascun Client del sistema. Come si può vedere nel <a href="#lst1"> listato 1 </a>, che mostra l'utilizzo delle _socket_ nel Client Desktop, il _client socket_ è incaricato di ricevere il messaggio e, una volta elaborato, aggiornare la View. 

Nello specifico, è possibile osservare come avviene l'aggiornamento in real-time del valore di un determinato parametro. In questo caso, a seguito della connessione al Server, il Client si mette in attesa dell'arrivo dei messaggi da parte della _socket server_. Non appena il messaggio viene ricevuto dal Client, mediante il ``textMessageHandler``, viene convertito in formato JSON e da questo vengono poi estratte tutte le informazioni necessarie per l'aggiornamento: per prima cosa si verifica che il messaggio di aggiornamento sia relativo alla serra che l'applicazione sta monitorando, poi individuato il parametro per il quale viene notificato il nuovo valore, vengono estratti e di conseguenza aggiornati: il valore corrente, ossia quello nuovo rilevato, la data in cui questo è stato rilevato ed infine, lo storico delle rilevazioni associate ad asso. Una volta effettuato l'aggiornamento di quello che è sostanzialmente il Model, si procede, a richiamare l'aggiornamento della View passandogli i nuovi valori.

```java
HttpClient socketClient = vertx.createHttpClient();
socketClient.webSocket(SOCKET_PORT,
        HOST,
        "/",
        wsC -> {
            WebSocket ctx = wsC.result();
            if (ctx != null) {
                socket = ctx;
                ctx.textMessageHandler(msg -> {
                    JsonObject json = new JsonObject(msg);
                    if(json.getValue("greenhouseId").equals(this.id)) {
                        if (json.getValue("parameterName").equals(parameterType.getName())) {
                            DateFormat formatter = new SimpleDateFormat("dd/MM/yyyy - HH:mm:ss");
                            this.parameter.getCurrentValue()
                            .setValue(
                                Double.valueOf(json.getValue("value").toString())
                            );
                            try {
                                this.parameter.getCurrentValue()
                                .setDate(formatter.parse(json.getString("date")));
                            } catch (ParseException e) {
                                throw new RuntimeException(e);
                            }
                            var newHistory = this.parameter.getHistory();
                            newHistory.remove(0);
                            ParameterValue newParamValue = new ParameterValueImpl(this.id, 
                            this.parameter.getCurrentValue().getDate(), 
                            this.parameter.getCurrentValue().getValue());
                            newHistory.add(newParamValue);
                            this.parameter.setHistory(newHistory);
                            this.view.updateValues(json.getValue("value").toString() + " " + unit, 
                            this.status(), 
                            this.parameter.getHistoryAsMap());
                        }
                    }
                });
            }

        });
```
<p align="center" id="lst1">[Listato 1] Esempio client socket</p>

## Dettagli implementativi del Client Desktop
Come per le altre parti del sistema, anche per il Client Desktop si è deciso di seguire un approccio il più modulare possibile, cosicché l'eventuale aggiunta o rimozione di funzionalità sia il più semplice e meno impattante possibile.

### JavaFx
Il Client realizzato, come detto precedentemente, si compone di 3 schermate: la homepage, incaricata di mostrare lo stato della serra; quella di dettaglio di un parametro, nel quale è possibile visualizzare uno _snapshot_ dello storico delle rilevazioni di uno specifico parametro; ed infine, quella di riepilogo delle operazioni svolte, che mostra anche in questo caso uno snapshot delle operazioni effettuate sia in modalità manuale che automatica, potendole filtrare per parametro o per data.

Per l’implementazione dell’interfaccia grafica si è scelto di utilizzare la libreria JavaFX, la quale permette di gestire le parti statiche dell'applicazione attraverso la creazione di layout in formato FXML

FXML è un formato XML che permette di comporre applicazioni JavaFX, separando il codice per la gestione degli elementi dalla parte di visualizzazione dei layout. Inoltre, l’utilizzo di SceneBuilder ha facilitato la creazione delle pagine attraverso il suo ambiente grafico, fornendo una renderizzazione visiva e intuitiva.

La logica di caricamento del file FXML viene racchiusa nella classe ``ApplicationViewImpl``: tutti i componenti delle ``subView`` hanno un riferimento a tale classe, specificando il file FXML associato, e possono ottenere in automatico il layout caricato.

Di fatto, il componente View rappresenta il Controller associato al layout. Il Controller può ottenere il riferimento agli elementi dell’interfaccia attraverso gli id specificati nell’FXML e mediante il caricatore, ossia FXMLLoader che cercherà di istanziarli e di renderli accessibili. Tale Controller ha il compito di inizializzare gli elementi dell’interfaccia utente e di gestirne il loro comportamento dinamico.

## Dettagli implementativi del Client Mobile
In questa sezione vengono illustrati gli aspetti rilevanti per la realizzazione dell’applicazione Mobile, descrivendo le scelte e i dettagli implementativi.

### Gestione e condivisione dei dati fra Fragment
Come detto in precedenza Android gestisce i cicli di vita delle ``Activity`` e dei ``Fragment`` in base alle azioni dell'utente oppure ad eventi fuori dal controllo dell'applicazione e quindi il sistema operativo può decidere di distruggere o ricreare il ``Controller UI``.

Ma se il sistema distrugge il controller dell'interfaccia utente, tutti i dati visualizzati nell'interfaccia utente verranno persi ed è necessario che la nuova ``Activity`` recuperi nuovamente tutti i dati.

Ovviamente per dati semplici l'``Activity`` può effettuare il loro recupero grazie al metodo ``onSavedInstanceState()``, ma per dati di grandi dimensioni e dati che devono essere caricati, viene consigliato di gestire quest'aspetto con un nuovo componente ``ViewModel``.

_Architecture Components_ fornisce la classe di supporto **ViewModel** per il controller dell'interfaccia. Gli oggetti ``ViewModel`` vengono conservati automaticamente in modo che i dati siano disponibili alla ripresa o al passaggio dell'``Activity`` o ``Fragment`` successiva. 

Nel <a href="#lst2"> listato 2 </a>  è mostrato un esempio di creazione del ``ViewModel``. In particolare ``GreenhouseViewModel`` deve estendere dalla classe base ``AndroidViewModel`` e fornire il contesto dell'applicazione.

Il ``ViewModel`` contiene degli oggetti particolari, contenitori di dati, chiamati ``LiveData``, oppure ``MutableLiveData`` i quali sono oggetti che contengono dati che possono essere modificati. Il ``ViewModel`` contiene anche dei **Repository** da cui richiedere il caricamento dei dati.

Gli oggetti ``LiveData`` e ``MutableLiveData`` possono essere osservati dai controller, attraverso la loro registrazione come osservatori. In tal modo, ogni volta che la lista si modifica, il controller viene notificato e quindi può aggiornare l'interfaccia.

Per fare ciò, nelle Activity o nei Fragment interessati, tramite il ``ViewModelProvider`` è possibile richiedere l'istanza del ViewModel se è già stato creato dai precedenti controller, oppure richiedere una nuova istanza del ViewModel.
```java
public class GreenhouseViewModelImpl extends AndroidViewModel implements GreenhouseViewModel {
    private final MutableLiveData<Plant> plantLiveData;
    //...
    
    @Override
    public LiveData<Plant> getPlantLiveData() {
         return this.plantLiveData;
    }
    
    @Override
    public void updatePlantInformation(Plant plant) {
         this.plantLiveData.postValue(plant);
         //...
    }
```
<p align="center" id="lst2">[Listato 2] Esempio di creazione del ViewModel