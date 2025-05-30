
# Database Project for "AtelierAuto"

**Application under test:** AtelierAuto  
**Tools used:** MySQL Workbench

---

## Database description:
Această bază de date simulează un sistem de gestiune pentru un atelier auto. Se stochează informații despre clienți, mașinile deținute, reparațiile efectuate, piesele folosite, mecanicii implicați și permisele conducătorilor auto. Baza reflectă relații reale între entități: 1:1, 1:N și N:N.

---

## Database Schema

- **Clienti** este conectat cu **Masini** printr-o relație 1:N, folosind `Clienti.id_client` ca PRIMARY KEY și `Masini.id_client` ca FOREIGN KEY.
- **Masini** este conectat cu **Reparatii** printr-o relație 1:N.
- **Reparatii** este conectat cu **Piese** prin intermediul tabelei **Reparatii_Piese** (relație N:N).
- **Mecanici** este conectat cu **Reparatii** prin **Mecanici_Reparatii** (relație N:N).
- **Clienti** este conectat cu **Permise** printr-o relație 1:1.

---

## Database Queries

### 🔧 DDL (Data Definition Language)

```sql
CREATE TABLE Clienti (
  id_client INT AUTO_INCREMENT PRIMARY KEY,
  nume VARCHAR(100),
  cnp CHAR(13),
  telefon VARCHAR(20),
  email VARCHAR(100)
);

CREATE TABLE Masini (
  id_masina INT AUTO_INCREMENT PRIMARY KEY,
  marca VARCHAR(50),
  model VARCHAR(50),
  an_fabricatie INT,
  id_client INT,
  FOREIGN KEY (id_client) REFERENCES Clienti(id_client)
);

CREATE TABLE Reparatii (
  id_reparatie INT AUTO_INCREMENT PRIMARY KEY,
  descriere VARCHAR(200),
  cost DOUBLE,
  id_masina INT,
  FOREIGN KEY (id_masina) REFERENCES Masini(id_masina)
);

CREATE TABLE Piese (
  id_piesa INT AUTO_INCREMENT PRIMARY KEY,
  nume_piesa VARCHAR(100),
  pret DECIMAL(10,2)
);

CREATE TABLE Reparatii_Piese (
  id_reparatie INT,
  id_piesa INT,
  PRIMARY KEY (id_reparatie, id_piesa),
  FOREIGN KEY (id_reparatie) REFERENCES Reparatii(id_reparatie),
  FOREIGN KEY (id_piesa) REFERENCES Piese(id_piesa)
);

CREATE TABLE Mecanici (
  id_mecanic INT AUTO_INCREMENT PRIMARY KEY,
  nume VARCHAR(100),
  experienta INT
);

CREATE TABLE Mecanici_Reparatii (
  id_mecanic INT,
  id_reparatie INT,
  PRIMARY KEY (id_mecanic, id_reparatie),
  FOREIGN KEY (id_mecanic) REFERENCES Mecanici(id_mecanic),
  FOREIGN KEY (id_reparatie) REFERENCES Reparatii(id_reparatie)
);

CREATE TABLE Permise (
  id_client INT PRIMARY KEY,
  nr_permis VARCHAR(20),
  categorie VARCHAR(5),
  FOREIGN KEY (id_client) REFERENCES Clienti(id_client)
);

ALTER TABLE Reparatii MODIFY cost DOUBLE;
ALTER TABLE Clienti DROP COLUMN adresa;
```

---

### 🧩 DML (Data Manipulation Language)

```sql
INSERT INTO Clienti VALUES (NULL, 'Cristi', '1980101223344', '0724000000', 'vasile@email.com');

INSERT INTO Clienti (nume, telefon, email, cnp) 
VALUES ('Vasile Pop', '0724000000', 'vasile@email.com', '1980101223344');

UPDATE Clienti SET email = 'gina@yahoo.com' WHERE nume = 'Gina';

DELETE FROM Clienti WHERE nume = 'Denisa';
```

---

### 🔎 DQL (Data Query Language)

```sql
SELECT * FROM Clienti;
SELECT nume, email FROM Clienti;
SELECT * FROM Clienti WHERE email LIKE '%gmail.com';
SELECT * FROM Clienti WHERE email LIKE '%gmail.com' AND telefon LIKE '07%';
SELECT * FROM Clienti WHERE email LIKE '%yahoo.com' OR telefon LIKE '07%';
SELECT * FROM Piese ORDER BY pret DESC LIMIT 2;
SELECT id_masina, COUNT(*) AS nr_reparatii FROM Reparatii GROUP BY id_masina HAVING nr_reparatii > 1;
```

---

### 🔁 JOIN-uri

```sql
SELECT c.nume, m.marca, r.descriere
FROM Clienti c
JOIN Masini m ON c.id_client = m.id_client
JOIN Reparatii r ON m.id_masina = r.id_masina;

SELECT c.nume, m.marca
FROM Clienti c
LEFT JOIN Masini m ON c.id_client = m.id_client;

SELECT c.nume, m.marca
FROM Clienti c
RIGHT JOIN Masini m ON c.id_client = m.id_client;

SELECT c.nume, p.nume_piesa
FROM Clienti c
CROSS JOIN Piese p;

SELECT nume 
FROM Clienti 
WHERE id_client IN (
  SELECT id_client FROM Masini WHERE marca = 'Dacia'
);
```

---

## Relații implementate:
- Relație 1:N: Clienti → Masini
- Relație 1:N: Masini → Reparatii
- Relație N:N: Reparatii → Piese (prin Reparatii_Piese)
- Relație N:N: Mecanici → Reparatii (prin Mecanici_Reparatii)
- Relație 1:1: Clienti → Permise
