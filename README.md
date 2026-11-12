# Sentiment Analysis per Bed & Breakfast

Progetto di **Basi di Dati** (a.a. 2022–2023, Canale I–Z, Traccia 5) realizzato dal Gruppo 07.  
L’obiettivo è progettare e implementare una base di dati relazionale per la gestione delle opinioni e dei sentiment, con granularità a livello di *aspect*, relativi a Bed & Breakfast della Campania.

## Descrizione

Il sistema modella una piattaforma di recensioni ispirata a Booking.com, in cui i clienti pubblicano recensioni sui Bed & Breakfast. Il testo di ciascuna recensione viene scomposto in frasi complesse e proposizioni; ogni proposizione è associata a un singolo *aspect* — ad esempio camere, colazione, personale, posizione o servizi — e a un punteggio di sentiment compreso tra 0 e 1.

La base di dati supporta la gestione di clienti, proprietari, strutture B&B, recensioni, aspetti valutati, punteggi di sentiment, indirizzi, città e punti di interesse nelle vicinanze delle strutture.

## Funzionalità

- Gestione di utenti, distinti in clienti e proprietari
- Registrazione di Bed & Breakfast, con sede, immagini, orari di check-in/check-out e punti di interesse
- Inserimento e consultazione delle recensioni
- Memorizzazione di frasi complesse e proposizioni estratte dalle recensioni
- Associazione di ogni proposizione a un aspect e a un punteggio di sentiment
- Query per analisi delle valutazioni, dei B&B e dei punti di interesse
- Viste per semplificare l’interrogazione dei dati
- Trigger per l’applicazione di vincoli aziendali e di cardinalità

## Requisiti

- PostgreSQL 12 o versione successiva
- Un client SQL, ad esempio `psql`, pgAdmin o DBeaver

## Installazione

### 1. Creare un database

Con `psql`:

```bash
createdb sentiment_analysis_bnb
```

Oppure, dalla console SQL:

```sql
CREATE DATABASE sentiment_analysis_bnb;
```

### 2. Eseguire gli script nell’ordine corretto

Dalla cartella del progetto:

```bash
psql -d sentiment_analysis_bnb -f "Creazione-e-Popolamento-Gruppo-7-Canale-IZ.sql"
psql -d sentiment_analysis_bnb -f "Query-e-Viste-Gruppo-7-Canale-IZ.sql"
psql -d sentiment_analysis_bnb -f "Trigger-Gruppo-7-Canale-IZ.sql"
```

In alternativa, in `psql`:

```sql
\i Creazione-e-Popolamento-Gruppo-7-Canale-IZ.sql
\i Query-e-Viste-Gruppo-7-Canale-IZ.sql
\i Trigger-Gruppo-7-Canale-IZ.sql
```

## Entità

- **Cliente**: utente che pubblica recensioni sui B&B
- **Proprietario**: utente titolare di una o più strutture
- **BnB**: struttura ricettiva identificata dal Codice Identificativo Regionale (CIR)
- **Recensione**: opinione pubblicata da un cliente su un B&B
- **FraseComplessa**: porzione di recensione ottenuta separando il testo in frasi
- **Proposizione**: porzione di frase associata a un singolo aspect
- **Valutazione**: punteggio di sentiment e tipo di aspect associati a una proposizione
- **Indirizzo** e **Città**: informazioni di localizzazione
- **PuntoDiInteresse**: attrazione o luogo nelle vicinanze di un B&B

## Vincoli principali

- Una recensione può essere pubblicata solo dopo la fine del pernottamento
- L’ID della recensione è composto da 10 cifre
- Il testo della recensione deve contenere tra 50 e 1000 caratteri
- Il punteggio del sentiment deve essere compreso tra 0 e 1
- Un cliente non può recensire un B&B se il suo codice fiscale coincide con quello di un proprietario della struttura
- Le recensioni più vecchie di 36 mesi vengono contrassegnate come non visibili
- Un punto di interesse può essere associato a un B&B solo se si trova entro 5 km

## Query e viste

Lo script `Query-e-Viste-Gruppo-7-Canale-IZ.sql` include query per:

- Calcolare valutazioni minime, massime e medie per un determinato aspect
- Identificare i B&B con le migliori valutazioni
- Analizzare relazioni tra clienti e proprietari
- Selezionare clienti in base a nazionalità, residenza, allergie e attività di recensione
- Ottenere i B&B recensiti vicini a punti di interesse
- Calcolare la percentuale di gradimento dei B&B vicini a musei o chiese

## Trigger

Lo script `Trigger-Gruppo-7-Canale-IZ.sql` implementa trigger PL/pgSQL per:

- Verificare la cardinalità minima della relazione tra proprietari e B&B
- Impedire che un cliente pubblichi una recensione per una struttura di cui è proprietario
- Applicare vincoli aziendali prima dell’inserimento o della modifica delle recensioni

## Autori

- Pietro Martano
- Luca Serafini
- Matteo Lauretano
- Luca Ianniello

## Licenza

Progetto realizzato per scopi didattici nell’ambito del corso di Basi di Dati nell'Anno Accademico 2022/2023 presso l’Università degli Studi di Salerno.
