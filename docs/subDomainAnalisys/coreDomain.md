---
title: Core domain - Greenhouse core
parent: Analisi dei sottodomini
has_children: false
nav_order: 2
---

## Core domain: Greenhouse core
Una delle linee guida del Domain Driven Desigin, specifica di conentrarsi maggiormente su quei domini che sono stati classificati come core, e nel nostro caso, il sub-domain greenhouse core  presenta le seguenti caratteristiche: 

- è costituito da quattro bounded context, la cui architettura verrà discussa nei successivi capitoli;
- il bounded-context **gestione serra**, si occupa della storicizzazione dei parametri rilevati all’interno della serra e di verificare i loro valori, pertanto le entità del dominio che rientreranno in questo bounded context risultano essere: la serra, la pianta, la luminosità, l’umidità dell’aria, l’umidità del suolo e la temperatura;
- il bounded context **gestione operazioni**, è incaricato di amministrare tutte le operazioni che avvengono all’interno della serra, sia se queste vengono eseguite automaticamente che manualmente, per tanto all’interno di questo bounded context rientra il concetto di operazione;
- per quanto riguarda i bounded context **gestione comunicazioni serra** e **gestione client**, essi rappresentano dei servizi necessari per poter gestire le interazioni fra la serra e i clients.

La seguente figura mostra, con maggior dettaglio, le caratteristiche relative ai bounded-context del sub-domain greenhouse core.
<div align="center">

![Scomposizione del sub-domain Greenhouse Core nei diversi bounded context](img/GreenhouseCore_boundedContext.png)

Scomposizione del sub-domain Greenhouse Core nei diversi bounded context
</div>