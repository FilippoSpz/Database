# Studio di Modellazione 3D - Progetto di Basi di Dati

## Table of Contents
1. [Descrizione del Progetto](#descrizione-del-progetto)
2. [Struttura della Base di Dati](#struttura-della-base-di-dati)
3. [Schema Entity-Relationship](#schema-entity-relationship)
4. [Normalizzazione](#normalizzazione)
5. [Implementazione SQL](#implementazione-sql)
6. [Funzionalità del Database](#funzionalita-del-database)
   - [Fatturazione automatizzata](#fatturazione-automatizzata)
8. [Conclusione](#conclusione)
9. [Licenza](#licenza)
10. [Autore](#autore)

---

## Descrizione del Progetto
Questo progetto riguarda la gestione di uno studio professionale di modellazione 3D, che realizza progetti per diversi clienti e tiene traccia dei modelli creati, delle risorse utilizzate e dei software impiegati. L'obiettivo è costruire una base di dati ben strutturata per supportare le operazioni dello studio.

---

## Struttura della Base di Dati
Il database è progettato per memorizzare e gestire le informazioni relative a:
- **Clienti**: aziende o privati che commissionano progetti allo studio.
- **Dipendenti**: artisti specializzati in modellazione, texturing, rigging, animazione, illuminazione e rendering.
- **Progetti**: lavori commissionati, con dettagli su budget, date e categorie.
- **Modelli 3D**: creati per ogni progetto e classificati per tipologia e complessità.
- **Risorse**: texture e asset utilizzati nei modelli 3D.
- **Software**: applicativi di modellazione con informazioni sulle licenze.
- **Corsi di Formazione**: offerti ai clienti dello studio e tenuti dai dipendenti.

---

## Schema Entity-Relationship
Il database segue un modello **Entity-Relationship (ER)** per rappresentare le varie entità e le loro relazioni. Alcuni aspetti chiave:
- Ogni **progetto** è associato a un **cliente** e coinvolge uno o più **dipendenti**.
- Ogni **modello** è parte di un progetto e utilizza **risorse** e **software** specifici.
- I **dipendenti** possono insegnare **corsi** di formazione ai clienti.
- I **corsi** hanno un numero massimo di partecipanti e possono essere frequentati solo da clienti esistenti.

---

## Normalizzazione
La struttura della base di dati è normalizzata secondo i seguenti principi:
1. **Prima Forma Normale (1NF)**: tutti gli attributi contengono valori atomici.
2. **Seconda Forma Normale (2NF)**: ogni attributo dipende completamente dalla chiave primaria.
3. **Terza Forma Normale (3NF)**: nessun attributo dipende da attributi non chiave.

---

## Implementazione SQL
Il database è stato implementato in **MySQL**. Alcuni elementi chiave:
- Tabelle con chiavi primarie e riferimenti alle entità correlate.
- **Trigger** per garantire vincoli, come il numero massimo di partecipanti ai corsi e l'assegnazione di almeno un project manager a ogni progetto.
- **Stored procedures** per facilitare la gestione dei progetti e l'invio di comunicazioni ai clienti.
- **View** per analizzare statistiche sull'uso dei software e la complessità dei modelli.

---

### Creazione delle Tabelle
Esempio di creazione della tabella `cliente`:
```sql
CREATE TABLE cliente (
    partitaIVA VARCHAR(15) NOT NULL PRIMARY KEY,
    ragioneSociale VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefono INT(20) NOT NULL,
    sitoWeb VARCHAR(100) DEFAULT NULL,
    tipo ENUM('azienda', 'privato') NOT NULL,
    citta VARCHAR(50) NOT NULL,
    via VARCHAR(100) NOT NULL,
    civico INT(10) NOT NULL,
    cap INT(5) NOT NULL
);
```

---

## Funzionalità del Database
Il database supporta diverse operazioni fondamentali per la gestione dello studio di modellazione 3D. Una delle principali funzionalità implementate include:

### **Fatturazione automatizzata**
L'amministrazione deve ottenere la lista dei progetti completati nell'ultimo mese per procedere alla fatturazione. Per agevolare questo processo, è stata sviluppata una **stored procedure** che restituisce i progetti completati recentemente.

```sql
DELIMITER $$
CREATE PROCEDURE sp_getProgettiCompletatiUltimoMese ()
BEGIN
    SELECT
        p.codiceProgetto,
        p.titolo,
        p.dataInizio,
        p.dataConsegna,
        p.budget,
        c.ragioneSociale AS cliente,
        c.partitaIVA
    FROM progetto p
    INNER JOIN cliente c ON p.idCliente = c.partitaIVA
    WHERE p.dataConsegna BETWEEN DATE_SUB(CURDATE(), INTERVAL 1 MONTH) AND CURDATE()
    ORDER BY p.dataConsegna DESC;
END $$
DELIMITER ;
```

---

## Conclusione
Il **Progetto di Basi di Dati per lo Studio di Modellazione 3D** fornisce una solida infrastruttura per la gestione dei progetti, dei clienti, dei dipendenti e delle risorse necessarie. Attraverso un modello ben strutturato e normalizzato, il database garantisce efficienza e scalabilità.

---

📅 **Ultimo aggiornamento**: Maggio 2025  
🛠 **Tecnologie usate**: MySQL, SQL, LaTex

---

## Licenza
Questo progetto è destinato esclusivamente a fini educativi e non è pensato per un uso commerciale.

---

## Autore
- **Nome**: [Filippo Spazzali]  
- **Università**: [Università degli Studi di Trieste]  
- **Corso**: [Ingegneria Informatica]
