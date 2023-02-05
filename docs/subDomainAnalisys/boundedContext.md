---
title: Bounded context
parent: Analisi dei sottodomini
has_children: false
nav_order: 1
---
# Bounded context

Dopo aver individuato i diversi sotto-domini di cui si compone il sistema, si è passati alla definizione e individuazione dei diversi bounded context che possono essere contenuti al loro interno (<a href="#fig1"> figura 1</a>). 

<div align="center">
<img src="img/bounded_context.png" alt="Individuazione dei diversi bounded context del sistema">
<p align="center" id="fig1">[Fig 1] Individuazione dei diversi bounded context del sistema</p>
</div>

Il sub-domain **sistema di automazione serra**, si compone di tre diversi bounded context:

- **Rilevazione valori,** il quale racchiude i diversi sensori necessari per poter rilevare i parametri vitali delle piante presenti all’interno della serra
- **Esecuzione operazioni,** il quale racchiude gli elementi che si occupano di gestire la logica per l’esecuzione delle diverse operazioni che possono essere richieste dall'operatore o dal sistema.
- **Comunicazioni**, che comprende tutti i diversi elementi necessari per poter inviare i dati rilevati e ricevere i comandi relativi alle operazioni da eseguire.

Il sub-domain **greenhouse core** presenta al suo interno quattro bounded context che sono:

- **Gestione comunicazioni serra**, il quale si occupa di gestire le comunicazioni con il sistema di automazione della serra;
- **Gestione serra**, che si occupa di raccogliere e gestire i dati rilevati all’interno della serra dai rispettivi sensori;
- **Gestione operazioni**, il quale si occupa di raccogliere le operazioni che il sistema o l’operatore hanno richiesto di eseguire;
- **Gestione comunicazioni client**, il quale si occupa di gestire le comunicazioni con i client Desktop e Mobile.

Il sub-domain client, è costituito dai seguenti bounded-context:

- **Mobile**, il quale detiene gli elementi necessari per consentire il monitoraggio dello stato della serra e la sua gestione in modalità manuale da parte di un operatore tramite un’applicazione Mobile
- **Desktop**, che detiene gli elementi affinché possa essere effettuata un’analisi della situazione attuale della serra e delle operazioni compiute al suo interno, tramite l’utilizzo di un’applicazione Desktop.

## Bounded context canvas

Per comprendere meglio cosa ci rappresentano i diversi bounded context e le comunicazioni che possono intercorrere fra di questi sono stati realizzati diversi bounded context canvas, riportati di seguito.

**Context canvas relativi al sub-domain: sistema di automazione serra**

| Nome | Rilevazione valori |
| --- | --- |
| Descrizione | È costituito dai sensori presenti nella serra per il monitoraggio della coltivazione  dal sistema di invio dei messaggi |
| Decisione di business | Possibile aggiunta di nuovi sensori |
| Ruolo nel dominio | Si occupa di rilevare i valori dei diversi sensori presenti all’interno della serra |
| Classificazione strategica | Rientra nel support domain sistema di automatizzazione serra |
| Ubiquitous language | Fotoresistenza, sensore di temperatura e umidità, sensore di umidità del suolo |
| Inbound communication | / |
| Outbound communication | Invia i dati raccolti dai sensori al bounded context comunicazioni |

| Nome | Esecuzione operazioni |
| --- | --- |
| Descrizione | È costituito dagli elementi che si occupano di attuare le operazioni richieste per modificare i parametri monitorati all’interno della serra |
| Decisione di business | Possibile aggiunta o aggiornamento dei sistemi esistenti, ridurre lo spreco di energia e il consumo di acqua richieste per la coltivazione |
| Ruolo nel dominio | Si occupa di eseguire le operazioni richieste dal sistema per correggere i valori rilevati |
| Classificazione strategica | Rientra nel support domain sistema di automatizzazione serra |
| Ubiquitous language | Ventilazione, irrigazione, luminosità, temperatura, pompa dell’acqua, ventola, lampada, lampada termica, attuatore |
| Inbound communication | Riceve dal bounded context comunicazioni comandi che richiedono di attivare o disattivare uno degli attuatori |
| Outbound communication | / |

| Nome | Comunicazioni |
| --- | --- |
| Descrizione | È rappresentato dalla scheda nodeMCU e dal sistema di invio e ricezione dei dati attraverso la rete |
| Decisione di business | / |
| Ruolo nel dominio | Essenziale per la comunicazione fra diversi bounded context, svolge un ruolo di intermediario per lo scambio di messaggi, si occupa di inoltrarli al corretto destinatario |
| Classificazione strategica | Rientra nel support domain sistema di automatizzazione serra |
| Ubiquitous language | Temperatura, umidità del terreno, umidità dell’aria, operazione correttiva, irrigazione, ventilazione, luminosità  |
| Inbound communication | Riceve i messaggi dei dati rilevati dal bounded context rilevazione valori e delle operazioni da svolgere dal bounded context gestione comunicazioni serra |
| Outbound communication | Invia i messaggi dei dati rilevati al bounded context gestione comunicazioni serra e delle operazioni da svolgere al bounded context esecuzione operazioni |

**Context canvas relativi al sub-domain: greenhouse core**

| Nome | Gestione comunicazioni serra |
| --- | --- |
| Descrizione | Rappresenta il modulo che comunica con il sottodominio sistema di  automazione serra |
| Decisioni di business | / |
| Ruolo nel dominio | Si occupa di mediare la comunicazione tra i sottodomini greenhouse core e sistema di automazione serra.  |
| Classificazione strategica | Rientra nel core domain di greenhouse core |
| Ubiquitous language | Temperatura, umidità del terreno, umidità dell’aria, luminosità, operazione correttiva, serra, pianta |
| Inbound communication | Riceve i messaggi dal bounded context comunicazioni contenenti i dati rilevati e riceve le operazioni di correzione da eseguire dal bounded context gestione operazioni |
| Outbound communication | invia le operazioni correttive da eseguire al bounded context comunicazioni e i dati rilevati al bounded context gestione operazioni |

| Nome | Gestione serra |
| --- | --- |
| Descrizione | Rappresenta il modulo che amministra la logica di gestione di una serra intelligente  |
| Decisioni di business | / |
| Ruolo nel dominio | si occupa di analizzare i dati rilevati dai sensori per individuare le situazioni di allarme e in tal caso stabilire l’operazione correttiva da intraprendere, memorizza i dati a lui recapitati e lo stato di gestione della serra (modalità automatica o manuale) |
| Classificazione strategica | rientra nel core domain di greenhouse core |
| Ubiquitous language | serra, pianta, valori ottimali, allarme, temperatura, umidità dell’aria, luminosità, umidità del terreno, modalità manuale, modalità automatica, operazioni correttive |
| Inbound communication | riceve i dati comunicati dal  bounded context gestione comunicazioni serra e i messaggi relativi al cambio di modalità di gestione e delle operazioni manuali dal bounded context gestione client |
| Outbound communication | invia i dati relativi alle operazioni correttive da effettuare al bounded context gestione operazioni e le informazioni relative alla serra al bounded context gestione client  |

| Nome | Gestione operazioni |
| --- | --- |
| Descrizione | Rappresenta il modulo che gestisce tutte le operazioni effettuate nella serra |
| Decisioni di business | Attivare i sistemi di correzione in modo da effettuare azioni correttive al dine di ripristinare i valori ottimali previsti per la pianta |
| Ruolo nel dominio | Si occupa di memorizzare le operazioni effettuate e comunicarle al bounded context gestione comunicazioni serra |
| Classificazione strategica | Rientra nel core domain di greenhouse core |
| Ubiquitous language | Temperatura, umidità dell’aria, luminosità, umidità del terreno, modalità manuale, modalità automatica, operazioni correttive, pompa dell’acqua, ventola, lampada, lampada termica |
| Inbound communication | Riceve le operazioni da memorizzare dal bounded context gestione serra  |
| Outbound communication | Invia i dati memorizzati al bounded context gestione client e l’operazione correttiva effettuata al bounded context gestione comunicazioni serra |

| Nome | Gestione client |
| --- | --- |
| Descrizione | Rappresenta il modulo che gestisce tutte le comunicazioni con i diversi client del sistema |
| Decisioni di business | / |
| Ruolo nel dominio | Si occupa di inviare i dati relativi alla serra ai client e gestire le richieste dell’operatore |
| Classificazione strategica | Rientra nel core domain di greenhouse core |
| Ubiquitous language | Temperatura, umidità dell’aria, luminosità, umidità del terreno, modalità manuale, modalità automatica, operazioni correttive, pompa dell’acqua, ventola, lampada, lampada termica, serra, pianta  |
| Inbound communication | Riceve le richieste dai generic sub-domains desktop e mobile |
| Outbound communication | Invia i dati richiesti ai generic sub-domains desktop e mobile |

**Context canvas relativi al sub-domain: client**

| Nome | Desktop |
| --- | --- |
| Descrizione | Rappresenta l'applicazione Desktop che si vuole realizzare |
| Decisioni di business | Cross-platform, l’interfaccia deve essere reattiva alle azioni dell’utente |
| Ruolo nel dominio | Si occupa di mostrare i dati relativi allo stato della serra e alle operazioni svolte su di essa in modo da consentire un’analisi da parte degli utenti |
| Classificazione strategica | Rientra nel generic domain client |
| Ubiquitous language | Temperatura, umidità dell’aria, luminosità, umidità del terreno, modalità manuale, modalità automatica, operazioni correttive, pompa dell’acqua, ventola, lampada, lampada termica, serra, pianta  |
| Inbound communication | Riceve i dati da mostrare all’utente dal bounded context client-communication |
| Outbound communication | / |

| Nome | Mobile |
| --- | --- |
| Descrizione | Rappresenta il modulo che gestisce tutte le comunicazioni con i diversi client del sistema |
| Decisioni di business | l’interfaccia deve essere reattiva alle azioni dell’utente, l’applicazione deve essere in grado di funzionare su dispositivi Android |
| Ruolo nel dominio | Si occupa di mostrare lo stato attuale alla serra e gestire la modalità manuale  |
| Classificazione strategica | Rientra nel generic domain client |
| Ubiquitous language | Temperatura, umidità dell’aria, luminosità, umidità del terreno, modalità manuale, modalità automatica, operazioni correttive, pompa dell’acqua, ventola, lampada, lampada termica, serra, pianta  |
| Inbound communication | Riceve i dati da mostrare all’utente dal bounded context client-communication |
| Outbound communication | Invia i dati relativi alle operazioni eseguite dall'utente e al cambio di modalità al bounded context client communication |

## Context map

A seguito di quanto è stato detto nelle precedenti sezioni le relazioni fra i diversi bounded context possono essere rappresentate tramite la seguente context map (<a href="#fig2"> figura 2</a>).

<div align="center">
<img src="img/context_map.png" alt="Context map">
<p align="center" id="fig2">[Fig 2] Context map</p>
</div>

All’interno della context map, i bounded-context colorati in grigio fanno riferimento al core domain: greenhouse core; mentre i bounded context bianchi fanno riferimento ai support e generic sub-domain: sistema di automazione serra e client.

Tutte le diverse relazioni di tipo customer-supplier, tranne quella che lega il bounded context rilevazione valori con il bounded context comunicazioni, sono conformiste, il che significa che il *downstream* si adatta alle informazioni che vengono passate dall'*upstream* così come sono senza porre vincoli o cambiamenti. Nel caso, invece, del bounded context **rilevazione valori** è stata adottata la strategia *Open Host Service,* di conseguenza, è il bounded context *upstream* che si impegna nel fornire il miglior servizio possibile al bounded context *downstream*, adattando le informazioni inviate alle sue esigenze; nel nostro caso le informazioni inviate vengono adattate in modo tale che possano essere direttamente comunicate al bounded context gestione comunicazioni serra, da parte del bounded context comunicazioni, senza ulteriori elaborazioni.

Fra i bounded context comunicazioni e gestione comunicazioni serra vi è una relazione di *partnership*, in quanto, comunicazioni si occupa di fornire i dati rilevati dai sensori a gestione comunicazioni serra, mentre quest’ultimo, si occupa di inviare al bounded context comunicazioni le operazioni correttive che devono essere intraprese.

