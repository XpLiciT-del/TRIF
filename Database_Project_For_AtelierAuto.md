
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
-- și așa mai departe cu restul tabelelor...
```

**ALTER TABLE:**
```sql
ALTER TABLE Clienti ADD cnp CHAR(13);
ALTER TABLE Reparatii MODIFY cost DOUBLE;
```

**DROP, TRUNCATE:**
```sql
DROP TABLE IF EXISTS TestTable;
TRUNCATE TABLE Mecanici;
```

---

### DML (Data Manipulation Language)

**INSERT implicite:**
```sql
INSERT INTO Clienti VALUES (NULL, 'Trif Cristian', '1234567890123', '0721111111', 'cristi@email.com');
```

**INSERT explicite:**
```sql
INSERT INTO Clienti (nume, cnp, telefon, email) VALUES ('Trif Cristian', '1234567890123', '0721111111', 'cristi@email.com');
```

**UPDATE:**
```sql
UPDATE Clienti SET telefon = '0720000000' WHERE nume = 'Trif Cristian';
```

**DELETE:**
```sql
DELETE FROM Reparatii WHERE cost < 100;
```

---

### DQL (Data Query Language)

```sql
SELECT * FROM Clienti;
SELECT nume, telefon FROM Clienti WHERE nume LIKE 'T%';
SELECT * FROM Masini WHERE an_fabricatie > 2015 AND marca = 'Dacia';
```

**JOIN-uri:**
```sql
SELECT c.nume, m.marca, r.descriere
FROM Clienti c
JOIN Masini m ON c.id_client = m.id_client
JOIN Reparatii r ON m.id_masina = r.id_masina;
```

**AGREGARE + GROUP BY + HAVING:**
```sql
SELECT id_client, COUNT(*) AS nr_masini
FROM Masini
GROUP BY id_client
HAVING COUNT(*) > 1;
```

**SUBQUERY:**
```sql
SELECT nume FROM Clienti
WHERE id_client IN (
  SELECT id_client FROM Masini WHERE marca = 'Dacia'
);
```

---

## Conclusions

Acest proiect mi-a oferit ocazia să aplic în mod practic noțiunile învățate în cadrul cursului: crearea structurii unei baze de date relaționale, implementarea relațiilor între tabele, utilizarea comenzilor DDL, DML și DQL, precum și simularea unor scenarii realiste. Am înțeles mai bine importanța relațiilor 1:N, N:N și cum se structurează corect datele pentru interogări eficiente. Lecția principală: organizarea logică a datelor e esențială pentru scalabilitatea și utilizarea practică a unei baze de date.
