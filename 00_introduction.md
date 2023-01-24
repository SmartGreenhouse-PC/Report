# Introduzione
Per il progetto di Smart City si vuole realizzare un'applicazione che consenta la gestione e il monitoraggio di una serra intelligente.

All'interno della serra viene coltivata una sola tipologia di piantagione, per i quali verranno monitorati i seguenti parametri: temperatura ambientale, umidità dell'aria e del terreno e luminosità.

Conosciuti i valori ottimali, delle grandezze da rilevare per la pianta di riferimento, si vuole monitorare i valori dei parametri in modo da individuare eventuali situazioni critiche, cioè situazioni in cui il valore rilevato non rientra nei range ottimali previsti. Nel caso in cui si verifichi questa condizione, il sistema deve essere in grado automaticamente di porvi rimedio, vale a dire intraprendere delle azioni correttive che consentano di aggiustare i valori rilevati all'interno della serra. Ad esempio, se la temperatura ambientale è troppo elevata il sistema azionerà il modulo di ventilazione, mentre se risulta troppo bassa attiverà le lampade termiche presenti all'interno della serra.

Il sistema sarà caratterizzato da quattro componenti: un modulo Arduino, un Server e due tipologie di clients.

Il modulo Arduino, facente parte del sistema, comprenderà: i diversi sensori presenti nella serra per monitorare lo stato di salute delle piante e i moduli correttivi per la gestione delle operazioni al suo interno. Tra i sensori che verranno utilizzati troviamo:
    - una fotoresistenza per rilevare la luminosità;
    - un sensore (DHT11) per misurare la temperatura e l'umidità dell'aria;
    - un sensore per misurare l'umidità del terreno.


Per quanto riguarda invece i moduli correttivi questi consistono in: 

    - una sistema di ventilazione, costituito da una ventola e un motore DC;
    - un sistema di irrigazione, costituito da una pompa ad acqua;
    - un led per regolare la luminosità;
    - un led rosso per simulare una lampada termica.

Come detto precedentemente, si intendono realizzare due tipologie di clients: un Client Desktop e un Client Mobile.

Tramite il Client Desktop l'utente avrà la possibilità di poter monitorare lo stato attuale della serra e dei singoli parametri rilevati, visualizzare lo storico delle operazioni compiute sia automaticamente che manualmente ed infine, avrà anche la possibilità di visualizzare i dati storici (relativi a un certo periodo di tempo), per la serra monitorata nel suo complesso o filtrati per parametro: luminosità, temperatura ed umidità del suolo e dell'ambiente.

Il Client Mobile, invece, potrà essere utilizzato dall'operatore sul campo e gli darà sia la possibilità di visualizzare i valori rilevati che di prendere il controllo manuale della gestione della serra, potendo attivare e disattivare i diversi sistemi in essa presenti. In particolare, le operazioni che possono essere compiute sono le seguenti:

    - regolare l'intensità delle lampade per gestire la luminosità dell'ambiente,
    - attivare il sistema di ventilazione per gestire l'umidità dell'aria,
    - gestire la temperatura tramite sia il sistema di ventilazione che l'attivazione delle lampade termiche,
    - attivare il sistema di irrigazione per aumentare l’umidità del terreno.
