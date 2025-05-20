
## 📊 Agregácia a práca s viacerými tabuľkami

---

### 🔢 Agregačné funkcie

```sql
SELECT COUNT(*) FROM zamestnanci;
SELECT SUM(plat) FROM zamestnanci;
SELECT AVG(cena) FROM produkty;
SELECT MIN(datum_narodenia), MAX(datum_narodenia) FROM zakaznici;
```

> Agregačné funkcie umožňujú vypočítať súhrnné hodnoty ako počet, súčet, priemer, minimum a maximum.

---

### 📈 Priemerná cena objednávok

```sql
SELECT AVG(cena) AS priemerna_cena FROM objednavky;
```

> Výpočet priemernej ceny pomocou funkcie `AVG`. Alias `priemerna_cena` zlepší čitateľnosť výsledku.

---

### 🧱 Vytvorenie tabuľky a vloženie údajov

```sql
CREATE TABLE produkty (
  id NUMBER GENERATED ALWAYS AS IDENTITY,
  nazov VARCHAR2(100),
  cena NUMBER(10, 2),
  kategoria VARCHAR2(50),
  PRIMARY KEY (id)
);

INSERT INTO produkty (nazov, cena, kategoria)
VALUES ('Notebook ASUS', 799.99, 'Elektronika');
```

> Automaticky generovaný primárny kľúč, vhodné pre e-shopy.

---

### 📊 Použitie GROUP BY

#### Základné zoskupenie:

```sql
SELECT kategoria, COUNT(*) FROM produkty GROUP BY kategoria;
```

> Spočíta počet produktov v jednotlivých kategóriách.

#### Viacúrovňové zoskupenie:

```sql
SELECT mesto, pohlavie, COUNT(*) 
FROM zamestnanci 
GROUP BY mesto, pohlavie;
```

> Detailnejšie štatistiky podľa viacerých stĺpcov.

#### Kombinovaná agregácia:

```sql
SELECT mesto, AVG(plat), MIN(plat), MAX(plat)
FROM zamestnanci 
GROUP BY mesto;
```

> Zobrazí priemerný, minimálny a maximálny plat pre každé mesto.

---

### 🧮 Filtrovanie skupín pomocou HAVING

```sql
SELECT mesto, COUNT(*) 
FROM zakaznici 
GROUP BY mesto 
HAVING COUNT(*) > 5;
```

> Zobrazí mestá s viac než 5 zákazníkmi.

---

```sql
SELECT kategoria, AVG(cena) AS priemerna_cena 
FROM produkty 
GROUP BY kategoria 
HAVING AVG(cena) > 100;
```

> Priemerná cena vyššia než 100 v rámci kategórie.

---

### 🔍 WHERE vs. HAVING

```sql
SELECT * FROM zamestnanci WHERE plat > 1000;
```
> Filtrovanie pred agregáciou.

```sql
SELECT oddelenie, AVG(plat) 
FROM zamestnanci 
GROUP BY oddelenie 
HAVING AVG(plat) > 1000;
```
> Filtrovanie po agregácii.

---

### 🎯 Praktické príklady

```sql
SELECT * FROM zakaznici WHERE mesto IS NULL;
SELECT nazov, cena, cena * 1.23 AS cena_s_dph FROM produkty;
SELECT * FROM produkty WHERE kategoria IN ('Kancelária', 'Elektronika');
SELECT * FROM produkty ORDER BY cena DESC FETCH FIRST 3 ROWS ONLY;
SELECT * FROM zakaznici WHERE meno LIKE '%a';
SELECT COUNT(*) AS drahsie_objednavky FROM objednavky WHERE cena > 100;
SELECT kategoria, AVG(cena) AS priemerna_cena FROM produkty GROUP BY kategoria;
SELECT * FROM objednavky WHERE TRUNC(datum_objednavky) = TRUNC(SYSDATE);
```

---

### 🔗 Práca s viacerými tabuľkami (JOIN)

```sql
SELECT * FROM zakaznici
WHERE id NOT IN (
  SELECT zakaznik_id FROM objednavky
);
```

> Zákazníci bez objednávky.

```sql
SELECT z.meno, o.id, o.cena
FROM zakaznici z
JOIN objednavky o ON z.id = o.zakaznik_id;
```

> INNER JOIN: zákazníci a ich objednávky.

```sql
SELECT z.meno, COUNT(o.id) AS pocet_objednavok
FROM zakaznici z
LEFT JOIN objednavky o ON z.id = o.zakaznik_id
GROUP BY z.meno;
```

> LEFT JOIN: všetci zákazníci a počet ich objednávok (aj nulový).

---


---

### 🎁 Bonusové príklady

#### 📅 Počet objednávok podľa dňa

```sql
SELECT TRUNC(datum_objednavky) AS den, COUNT(*) AS pocet
FROM objednavky
GROUP BY TRUNC(datum_objednavky)
ORDER BY den;
```

> Zobrazí počet objednávok uskutočnených v jednotlivých dňoch.

---

#### 🏢 Priemerný plat podľa oddelenia s minimálne 3 zamestnancami

```sql
SELECT oddelenie, ROUND(AVG(plat), 2) AS priemerny_plat
FROM zamestnanci
GROUP BY oddelenie
HAVING COUNT(*) >= 3;
```

> Skupinová analýza s filtrovaním podľa počtu záznamov.

---

#### 🧾 Zákazníci a celková hodnota ich objednávok

```sql
SELECT z.meno, SUM(o.cena) AS celkova_hodnota
FROM zakaznici z
JOIN objednavky o ON z.id = o.zakaznik_id
GROUP BY z.meno;
```

> Spojenie a agregácia celkovej hodnoty objednávok za každého zákazníka.

---

#### 🔄 Zákazníci bez objednávky (alternatíva NOT EXISTS)

```sql
SELECT * FROM zakaznici z
WHERE NOT EXISTS (
  SELECT 1 FROM objednavky o WHERE o.zakaznik_id = z.id
);
```

> Efektívna alternatíva ku klasickému `NOT IN` na zistenie zákazníkov bez objednávky.

---

#### 🧮 Počet produktov v každej kategórii, zoradené zostupne

```sql
SELECT kategoria, COUNT(*) AS pocet
FROM produkty
GROUP BY kategoria
ORDER BY pocet DESC;
```

> Triedenie podľa počtu produktov v kategórii.

---
