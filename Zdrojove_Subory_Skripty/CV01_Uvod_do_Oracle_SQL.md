
## 🧠 Úvod do Oracle databáz a SQL

Táto podkapitola predstavuje základné koncepty Oracle SQL, vrátane testovania spojenia, vytvárania tabuliek a základných dotazov.

---

### ✅ Overenie funkčnosti spojenia s databázou

```sql
SELECT * FROM dual;
```

- `DUAL` je špeciálna systémová tabuľka v Oracle.
- Obsahuje **jeden riadok** a **jeden stĺpec** `DUMMY` s hodnotou `'X'`.
- Používa sa na výpočty, testovanie výrazov alebo funkcií bez nutnosti pracovať s reálnymi tabuľkami.
- **Vytvára sa automaticky** a je dostupná pre všetkých používateľov.

#### 🧪 Príklady použitia DUAL:

```sql
-- Matematika:
SELECT 2 + 2 FROM dual;

-- Dátum a čas:
SELECT TO_CHAR(SYSDATE, 'DD-MM-YYYY HH24:MI:SS') FROM dual;

-- Konverzia typu:
SELECT TO_NUMBER('123.45') FROM dual;
```

---

### 🔍 Zistenie verzie Oracle databázy

```sql
SELECT * FROM v$version;
```

- `v$version` je systémový pohľad, ktorý poskytuje informácie o:
  - číselnej verzii databázy
  - edícii (napr. Express, Enterprise)
  - dátume vydania
  - nainštalovaných komponentoch

---

### 📐 Vytvorenie jednoduchej tabuľky

```sql
CREATE TABLE test_tab (
  id NUMBER PRIMARY KEY,
  popis VARCHAR2(100)
);
```

- Vytvára tabuľku `test_tab` s dvoma stĺpcami: `id` a `popis`.
- `id` je primárny kľúč – zabezpečuje jedinečnosť a neumožňuje NULL hodnoty.
- `VARCHAR2(100)` predstavuje textový reťazec s max. dĺžkou 100 znakov.

---

### 🧾 Vytvorenie tabuľky produktov

```sql
CREATE TABLE produkty (
  id NUMBER GENERATED ALWAYS AS IDENTITY,
  nazov VARCHAR2(100),
  cena NUMBER(10, 2),
  kategoria VARCHAR2(50),
  PRIMARY KEY (id)
);
```

- `id` je automaticky generovaný identifikátor (sekvencia).
- `NUMBER(10,2)` – číslo s max. 10 číslicami, z toho 2 desatinné.
- `PRIMARY KEY (id)` – zabezpečí jedinečnosť a povinnosť hodnoty v stĺpci `id`.

---

### ➕ Vloženie záznamu

```sql
INSERT INTO produkty (nazov, cena, kategoria)
VALUES ('Notebook ASUS', 799.99, 'Elektronika');
```

- Nevkladáme `id`, pretože sa generuje automaticky.
- Tento príkaz vloží nový produkt do tabuľky.

---

### 🔍 Zobrazenie údajov

#### Všetky údaje:

```sql
SELECT * FROM produkty;
```

#### Len konkrétne stĺpce:

```sql
SELECT nazov, cena FROM produkty;
```

- Používame výber len požadovaných údajov (projekcia stĺpcov).

---


---

### 🎁 Bonusové príklady

#### 🔢 Počet produktov v tabuľke

```sql
SELECT COUNT(*) AS pocet_produktov FROM produkty;
```

➡️ Vráti počet všetkých riadkov v tabuľke `produkty`.

---

#### 💰 Výber produktov nad určitou cenou

```sql
SELECT nazov, cena FROM produkty
WHERE cena > 500;
```

➡️ Zobrazí produkty, ktorých cena je vyššia ako 500.

---

#### 🧮 Priemerná cena produktov

```sql
SELECT ROUND(AVG(cena), 2) AS priemerna_cena FROM produkty;
```

➡️ Spočíta priemernú cenu všetkých produktov.

---

#### 📂 Skupinový výber podľa kategórie

```sql
SELECT kategoria, COUNT(*) AS pocet
FROM produkty
GROUP BY kategoria;
```

➡️ Zobrazí počet produktov v jednotlivých kategóriách.

---

#### 🔍 Vyhľadávanie podľa časti textu

```sql
SELECT * FROM produkty
WHERE nazov LIKE '%ASUS%';
```

➡️ Nájde všetky produkty, ktoré obsahujú text `ASUS` v názve.

---

