
# Database Project for "AtelierAuto"

The scope of this project is to use all the SQL knowledge gained throughout the Software Testing course and apply them in practice.

**Application under test:** AtelierAuto – gestiune clienți, mașini, mecanici, reparații și piese auto

**Tools used:** MySQL Workbench

**Database description:**  
Baza de date simulează activitatea unui atelier auto. Aceasta permite evidența clienților și a mașinilor lor, înregistrarea reparațiilor efectuate, a mecanicilor implicați și a pieselor utilizate. Informațiile sunt salvate în mod structurat pentru a permite generarea de rapoarte și interogări utile în gestionarea operațiunilor.

---

## Database Schema

The tables are connected in the following way:

- **Clienti** este conectată cu **Masini** printr-o relație **1:N** (`Clienti.id_client` → `Masini.id_client`)
- **Masini** este conectată cu **Reparatii** printr-o relație **1:N** (`Masini.id_masina` → `Reparatii.id_masina`)
- **Reparatii** este conectată cu **Piese** printr-o relație **N:N** via tabela intermediară **Reparatii_Piese**
- **Mecanici** este conectată cu **Reparatii** printr-o relație **N:N** via tabela intermediară **Mecanici_Reparatii**
- **Permise** este conectată cu **Mecanici** printr-o relație **1:1** (`Permise.id_mecanic` → `Mecanici.id_mecanic`)

---

## Database Queries

### DDL (Data Definition Language)

**CREATE TABLE:**

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

-- ALTER
ALTER TABLE Reparatii MODIFY cost DOUBLE;
ALTER TABLE Clienti DROP COLUMN adresa;


**DROP, TRUNCATE:**
```sql
DROP TABLE IF EXISTS TestTable;
TRUNCATE TABLE Mecanici;
```

---

### DML (Data Manipulation Language)

-- INSERT implicit
INSERT INTO Clienti VALUES (NULL, 'Cristi', '1980101223344', '0724000000', 'vasile@email.com');

-- INSERT explicit
INSERT INTO Clienti (nume, telefon, email, cnp) 
VALUES ('Vasile Pop', '0724000000', 'vasile@email.com', '1980101223344');

-- UPDATE
UPDATE Clienti SET email = 'gina@yahoo.com' WHERE nume = 'Gina';

-- DELETE
DELETE FROM Clienti WHERE nume = 'Denisa';

---

### DQL (Data Query Language)

-- SELECT simplu
SELECT * FROM Clienti;

-- SELECT coloane
SELECT nume, email FROM Clienti;

-- WHERE + LIKE
SELECT * FROM Clienti WHERE email LIKE '%gmail.com';

-- WHERE + AND + OR
SELECT * FROM Clienti WHERE email LIKE '%gmail.com' AND telefon LIKE '07%';
SELECT * FROM Clienti WHERE email LIKE '%yahoo.com' OR telefon LIKE '07%';

-- ORDER BY + LIMIT
SELECT * FROM Piese ORDER BY pret DESC LIMIT 2;

-- GROUP BY + HAVING
SELECT id_masina, COUNT(*) AS nr_reparatii FROM Reparatii GROUP BY id_masina HAVING nr_reparatii > 1;

---

## Conclusions

Acest proiect mi-a oferit ocazia să aplic în mod practic noțiunile învățate în cadrul cursului: crearea structurii unei baze de date relaționale, implementarea relațiilor între tabele, utilizarea comenzilor DDL, DML și DQL, precum și simularea unor scenarii realiste. Am înțeles mai bine importanța relațiilor 1:N, N:N și cum se structurează corect datele pentru interogări eficiente. Lecția principală: organizarea logică a datelor e esențială pentru scalabilitatea și utilizarea practică a unei baze de date.
