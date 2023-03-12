---
title: Smart Greenhouse
has_children: false
nav_order: 1
---

# Smart Greenhouse
Il progetto si propone di realizzare un'applicazione che consenta la gestione e il monitoraggio di un complesso di serre intelligenti.

In particolare, verrà simulata una situazione in cui un committente richieda l’automatizzazione dei processi che regolano le diverse serre e di monitorarne la loro gestione.

## Scenario
All'interno di una serra sarà presente una sola tipologia di piantagione, per i quali verranno monitorati i seguenti parametri:

- temperatura ambientale;
- umidità dell'aria e del terreno
- luminosità.

Conosciuti i valori ottimali, delle grandezze da rilevare per la pianta di riferimento, sarà possibile gestire eventuali situazioni di allarme per uno o più dei parametri monitorati.

Il sistema sarà in grado di compiere delle correzioni in modo automatico e/o manuale, al fine di riportare i valori rilevati nei range ottimali, in particolare le azioni che possono essere compiute sono le seguenti:

- regolare l’intensità delle lampade, per gestire la luminosità dell’ambiente;
- gestire l’umidità dell’aria tramite il sistema di ventilazione;
- gestire la temperatura sia tramite lampade temriche che attraverso il sistema di ventilazione;
- aumentare l’umidità del terreno tramite l’attivazione del sistema di irrigazione.

## Componenti del sistema
**Modulo Arduino**

Il sistema è costituito da un modulo Arduino che comprende i diversi sensori presenti nella serra per monitorare lo stato di salute delle piante e i moduli aggiuntivi per la gestione delle operazioni al suo interno (es. modulo d’irrigazione).

**Applicazione mobile**

L’applicazione mobile potrà essere utilizzata da un operatore sul campo, dandogli la possibilità di:

- scegliere la serra da monitorare;
- sapere i parametri attuali rilevati all'interno della serra;
- eventuali situazioni di allarme;
- intervenire manualmente per poter modificare i valori monitorati e applicare opportune correzioni.

**Applicazione desktop**

L'applicazione in oggetto ha lo scopo di permettere un'analisi dei dati rilevati nel lungo periodo, consentendo anche la visualizzazione in tempo reale dello stato della serra e delle eventuali operazioni di correzione compiute manualmente/automaticamente.

<div align="center">
<img src="https://github.com/SmartGreenhouse-PC/Report/blob/doc/img/smartgh.png" alt="Smart Greenhouse Logo"  width="220px" height="210px">
<p align="center">Smart Greenhouse Logo</p>
</div>

### Componenti del gruppo
- Mengozzi Maria  - <maria.mengozzi3@studio.unibo.it>
- Vitali Anna  - <anna.vitali4@studio.unibo.it>

### Repository
- [Report](https://github.com/SmartGreenhouse-PC/Report) - [GitHub Pages](https://SmartGreenhouse-PC.github.io/Report/)
- [Sistema di automazione](https://github.com/SmartGreenhouse-PC/ArduinoSensor)
- [Server](https://github.com/SmartGreenhouse-PC/Server)
- [Client Desktop](https://github.com/SmartGreenhouse-PC/ClientDesktop.git)
- [Client Mobile](https://github.com/SmartGreenhouse-PC/ClientMobile)
