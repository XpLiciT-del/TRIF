# Database Project for "Auto Repair Workshop Management"

The scope of this project is to use all the SQL knowledge gained throughout the Software Testing course and apply it in practice.

## Application under test:
Auto Repair Workshop Management

## Tools used:
MySQL Workbench

## Database description:
This database simulates the management of an auto repair workshop. It stores information about clients, their vehicles, the repairs performed, parts used in those repairs, and the mechanics involved. It also tracks the permits of the mechanics.

## Database Schema:

## Database Schema

You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

The tables are connected in the following way:
- **Clienti** is connected with **Masini** through a one-to-many relationship.
- **Masini** is connected with **Reparatii** through a one-to-many relationship.
- **Reparatii** is connected with **Piese** through a many-to-many relationship via the **Reparatii_Piese** table.
- **Reparatii** is connected with **Mecanici** through a many-to-many relationship via the **Mecanici_Reparatii** table.
- **Mecanici** is connected with **Permise** through a one-to-one relationship.

These relationships were implemented using the following primary and foreign keys:
- **Clienti.id_client** as a primary key and **Masini.id_client** as a foreign key.
- **Masini.id_masina** as a primary key and **Reparatii.id_masina** as a foreign key.
- **Reparatii.id_reparatie** as a primary key and **Reparatii_Piese.id_reparatie** as a foreign key.
- **Piese.id_piesa** as a primary key and **Reparatii_Piese.id_piesa** as a foreign key.
- **Reparatii.id_reparatie** as a primary key and **Mecanici_Reparatii.id_reparatie** as a foreign key.
- **Mecanici.id_mecanic** as a primary key and **Mecanici_Reparatii.id_mecanic** as a foreign key.
- **Mecanici.id_mecanic** as a primary key and **Permise.id_mecanic** as a foreign key.

## SQL Commands

### DDL (Data Definition Language)

In order to define the structure of the database, I used the following DDL commands:

```sql
CREATE DATABASE  atelierauto;
    USE atelierauto;

CREATE TABLE Clienti (
    id_client INT PRIMARY KEY AUTO_INCREMENT,
    nume VARCHAR(100),
    telefon VARCHAR(15),
    email VARCHAR(100),
    cnp CHAR(13)
);

CREATE TABLE Masini (
    id_masina INT PRIMARY KEY AUTO_INCREMENT,
    id_client INT,
    marca VARCHAR(50),
    model VARCHAR(50),
    FOREIGN KEY (id_client) REFERENCES Clienti(id_client)
);

CREATE TABLE Reparatii (
    id_reparatie INT PRIMARY KEY AUTO_INCREMENT,
    id_masina INT,
    descriere VARCHAR(255),
    cost DECIMAL(10,2),
    FOREIGN KEY (id_masina) REFERENCES Masini(id_masina)
);

CREATE TABLE Piese (
    id_piesa INT PRIMARY KEY AUTO_INCREMENT,
    nume_piesa VARCHAR(100),
    stoc INT
);

CREATE TABLE Reparatii_Piese (
    id_reparatie INT,
    id_piesa INT,
    PRIMARY KEY (id_reparatie, id_piesa),
    FOREIGN KEY (id_reparatie) REFERENCES Reparatii(id_reparatie),
    FOREIGN KEY (id_piesa) REFERENCES Piese(id_piesa)
);

CREATE TABLE Mecanici (
    id_mecanic INT PRIMARY KEY AUTO_INCREMENT,
    nume VARCHAR(100),
    specializare VARCHAR(100)
);

CREATE TABLE Mecanici_Reparatii (
    id_mecanic INT,
    id_reparatie INT,
    PRIMARY KEY (id_mecanic, id_reparatie),
    FOREIGN KEY (id_mecanic) REFERENCES Mecanici(id_mecanic),
    FOREIGN KEY (id_reparatie) REFERENCES Reparatii(id_reparatie)
);

CREATE TABLE Permise (
    id_permis INT PRIMARY KEY AUTO_INCREMENT,
    id_mecanic INT UNIQUE,
    tip_permis VARCHAR(50),
    FOREIGN KEY (id_mecanic) REFERENCES Mecanici(id_mecanic)
);
```

Other DDL operations used:

```sql
ALTER TABLE Clienti ADD cnp CHAR(13);
DROP TABLE IF EXISTS Test;
TRUNCATE TABLE Reparatii;
```

### DML (Data Manipulation Language)

In order to populate the database with data and manipulate it, I used the following DML commands:

```sql
INSERT INTO Clienti (nume, telefon, email, cnp) VALUES ('Marcel Popescu', '0722000000', 'marcel@email.com', '1980101223344');
INSERT INTO Masini (id_client, marca, model) VALUES (1, 'Dacia', 'Logan');
INSERT INTO Reparatii (id_masina, descriere, cost) VALUES (1, 'Schimb ulei', 200.00);
INSERT INTO Piese (nume_piesa, stoc) VALUES ('Filtru ulei', 10);
INSERT INTO Reparatii_Piese (id_reparatie, id_piesa) VALUES (1, 1);
INSERT INTO Mecanici (nume, specializare) VALUES ('Ion Ionescu', 'Electrician');
INSERT INTO Mecanici_Reparatii (id_mecanic, id_reparatie) VALUES (1, 1);
INSERT INTO Permise (id_mecanic, tip_permis) VALUES (1, 'B');

UPDATE Piese SET stoc = 9 WHERE id_piesa = 1;
DELETE FROM Clienti WHERE id_client = 10;
```

### DQL (Data Query Language)

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

```sql
SELECT * FROM Clienti;
SELECT nume, email FROM Clienti WHERE telefon LIKE '072%';
SELECT * FROM Reparatii WHERE cost > 100 AND cost < 500;
SELECT marca, model FROM Masini ORDER BY marca DESC LIMIT 5;

SELECT id_masina, COUNT(*) AS nr_reparatii FROM Reparatii GROUP BY id_masina HAVING COUNT(*) > 2;

SELECT c.nume, m.marca, r.descriere
FROM Clienti c
JOIN Masini m ON c.id_client = m.id_client
JOIN Reparatii r ON m.id_masina = r.id_masina;

SELECT nume FROM Clienti WHERE id_client IN (SELECT id_client FROM Masini WHERE marca = 'Dacia');
```

## Relationships

In the current project I used and demonstrated the following types of relationships:

- One-to-One: `Mecanici` ↔ `Permise`
- One-to-Many: `Clienti` → `Masini`, `Masini` → `Reparatii`
- Many-to-Many: `Reparatii` ↔ `Piese`, `Mecanici` ↔ `Reparatii`

## Conclusion

This project demonstrated the creation and management of a structured relational database for a real-world scenario — an auto repair workshop. It involved designing normalized tables, enforcing referential integrity through primary and foreign keys, and performing CRUD operations. Through DDL, DML, and DQL commands, we validated the schema design, tested constraints, and simulated meaningful business operations. Additionally, JOINs and subqueries were used to retrieve relevant data.

## Author

Cristian Trif
