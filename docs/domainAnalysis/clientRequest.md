---
title: Richiesta committente
parent: Analisi del dominio
has_children: false
nav_order: 1
---

# Richiesta committente

>"Sono un produttore agricolo e mi occupo della coltivazione in serra di diverse tipologie di piante.
>
>Nell'ultimo periodo, dato il crescente aumento dei costi per la produzione e le problematiche legate ai cambiamenti climatici, sarei interessato a rendere la mie serre smart, ovvero rinnovare le mie attuali serre rendendole in grado di gestire in modo automatico o semi automatico, quindi senza il costante intervento umano, le principali attività per il benessere della crescita della pianta, cercando di realizzare un’agricoltura di precisione, che mi consenta di aumentare e migliorare la produzione, riducendo i costi ad essa legati. 
>
>Le attività che mi interessa automatizzare sono: 
>
 >   - l'irrigazione, quindi capire quando è opportuno irrigare il terreno, così da ridurre al minimo lo spreco di acqua;
 >   - la regolazione della luminosità dell'ambiente, quindi, essere in grado di dare alle piante la condizione ottimale di luminosità ideale per la loro crescita;
 >   - regolare la temperatura in modo tale da adattarla a quella ideale per la pianta coltivata;
 >   - regolare l’umidità ambientale, in quanto anch'essa importante per il benessere e la salute della pianta.
>
>
>Questi aspetti, fino ad ora, sono stati gestiti manualmente da un operatore in base all’esperienza, ora vorrei invece, come detto prima, che venissero il quanto più possibile automatizzati.
>
>Inoltre, mi piacerebbe avere la possibilità di poter gestire ogni serra e i diversi sistemi presenti al suo interno dal telefono. Vorrei avere un’applicazione sul mio smartphone, che mi dia la possibilità di prendere il controllo manuale della gestione della serra e poter richiedere l’esecuzione delle operazioni dette prima, come l’irrigazione, la regolazione della luminosità della temperatura etc. Oltre a poter compiere le operazioni sulla serra, mi interesserebbe anche sapere i valori attuali dei parametri rilevati al suo interno, quali luminosità, temperatura, umidità del terreno e dell'aria.
>
>Infine, vorrei avere un’applicazione Desktop che mi consenta di vedere i dati storici, quelli attuali e le operazioni compiute dall'operatore o dal sistema in automatico e di sapere lo stato attuale della serra."

## Impact map

A seguito della richiesta precedentemente illustrata, il team di sviluppo ha deciso di produrre la seguente impact map, per riuscire a comprendere maggiormente il problema e formulare domande significative nelle successive interviste.

![Impact map](img/impact-map.png)
<p align="center">Impact map</p>

Nell’immagine, il primo livello dell’impact map rappresenta l’obiettivo che si vuole ottenere: **Automatizzare la serra,** gli attori coinvolti nel raggiungere questo obiettivo sono sia gli sviluppatori che gli operatori all’interno della serra. 

Nello specifico, gli sviluppatori per ottenere l’automatizzazione della serra dovranno essere in grado di:

- Ridurre il consumo di acqua
- Ridurre lo spreco di energia elettrica
- Sfruttare gli apparati presenti nella serra per effettuare operazioni correttive dei parametri

Per poter raggiungere questi obiettivi il team di sviluppo dovrà programmare i sistemi presenti nella serra, affinché si attivino in base alle condizioni dell’ambiente e per raccogliere i dati dai sensori dovranno utilizzare appositi protocolli per lo scambio di informazioni, come ad esempio MQTT.

Mentre gli operatori avranno come obiettivo quello di monitorare le attività svolte e i valori rilevati. Per poter raggiungere tale obiettivo, l’operatore dovrà poter visualizzare e gestire le informazioni tramite un applicativo appositamente realizzato.