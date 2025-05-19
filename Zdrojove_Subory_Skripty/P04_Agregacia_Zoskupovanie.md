
# 📘 P04 – Agregácia a zoskupovanie v Oracle

Ukážky agregácie údajov pomocou SQL funkcií a príkazov `GROUP BY` a `HAVING`.

---

## 📊 Základné agregačné funkcie

```sql
SELECT
    COUNT(*) AS "Počet riadkov",
    SUM(plat) AS "Celkový plat",
    AVG(plat) AS "Priemerný plat",
    MIN(plat) AS "Minimálny plat",
    MAX(plat) AS "Maximálny plat"
FROM zamestnanci;
```

➡️ Tieto funkcie vypočítajú štatistiky z celej tabuľky `zamestnanci`.

---

## 👥 Agregácia podľa oddelenia – GROUP BY

```sql
SELECT
    oddelenie,
    COUNT(*) AS "Počet zamestnancov",
    AVG(plat) AS "Priemerný plat"
FROM zamestnanci
GROUP BY oddelenie;
```

➡️ `GROUP BY` zoskupí riadky podľa stĺpca `oddelenie` a vykoná agregáciu v rámci každej skupiny.

---

## 📌 Použitie GROUP BY s viacerými stĺpcami

```sql
SELECT
    oddelenie,
    pozicia,
    COUNT(*) AS "Počet"
FROM zamestnanci
GROUP BY oddelenie, pozicia;
```

➡️ Kombinácia viacerých skupinových kritérií – napr. podľa oddelenia a zároveň pozície.

---

## 🧾 Filtrovanie agregovaných skupín – HAVING

```sql
SELECT
    oddelenie,
    AVG(plat) AS "Priemerný plat"
FROM zamestnanci
GROUP BY oddelenie
HAVING AVG(plat) > 2000;
```

➡️ `HAVING` filtruje výsledky po zoskupení – napr. iba oddelenia s priemerným platom nad 2000.

---

## 🔎 Porovnanie WHERE vs HAVING

```sql
-- WHERE filtruje pred zoskupením
SELECT * FROM zamestnanci
WHERE oddelenie = 'IT';

-- HAVING filtruje po zoskupení
SELECT oddelenie, COUNT(*) 
FROM zamestnanci
GROUP BY oddelenie
HAVING COUNT(*) > 3;
```

➡️ `WHERE` filtruje riadky pred výpočtom, `HAVING` filtruje výsledné skupiny.

---

## 🧪 Agregácia bez NULL hodnôt

```sql
SELECT
    COUNT(email) AS "Počet e-mailov",
    COUNT(*) AS "Počet všetkých záznamov"
FROM zakaznici;
```

➡️ `COUNT(stlpec)` počíta len tie riadky, kde `stlpec` nie je `NULL`.



---

## 🧮 Agregácia s podmienkou (WHERE)

```sql
SELECT
    oddelenie,
    COUNT(*) AS "Počet"
FROM zamestnanci
WHERE pozicia = 'Programátor'
GROUP BY oddelenie;
```

➡️ Zobrazí počet programátorov v každom oddelení.

---

## 📅 Agregácia po mesiacoch (použitie TO_CHAR)

```sql
SELECT
    TO_CHAR(datum_nastupu, 'YYYY-MM') AS "Mesiac",
    COUNT(*) AS "Počet nástupov"
FROM zamestnanci
GROUP BY TO_CHAR(datum_nastupu, 'YYYY-MM')
ORDER BY "Mesiac";
```

➡️ Zoskupenie podľa mesiaca nástupu a výpočet počtu nástupov.

---

## 🧠 Viacnásobná agregácia v jednej tabuľke

```sql
SELECT
    COUNT(DISTINCT oddelenie) AS "Počet oddelení",
    COUNT(DISTINCT pozicia) AS "Počet pozícií"
FROM zamestnanci;
```

➡️ Počet unikátnych oddelení a pozícií.

---

## 💼 Skupinová štatistika podľa pracovnej pozície

```sql
SELECT
    pozicia,
    MIN(plat) AS "Najnižší",
    MAX(plat) AS "Najvyšší",
    ROUND(AVG(plat), 2) AS "Priemer"
FROM zamestnanci
GROUP BY pozicia;
```

➡️ Zobrazí platové štatistiky podľa jednotlivých pozícií.

---

## 🔁 Kombinácia GROUP BY a ORDER BY

```sql
SELECT
    oddelenie,
    COUNT(*) AS "Počet"
FROM zamestnanci
GROUP BY oddelenie
ORDER BY COUNT(*) DESC;
```

➡️ Oddelenia zoradené podľa počtu zamestnancov zostupne.

---

## 🧾 Použitie GROUP BY ROLLUP

```sql
SELECT
    oddelenie,
    pozicia,
    COUNT(*) AS "Počet"
FROM zamestnanci
GROUP BY ROLLUP (oddelenie, pozicia);
```

➡️ `ROLLUP` vráti súčty medzistupňov a celkový súčet.

---

## 🧾 Použitie GROUP BY CUBE

```sql
SELECT
    oddelenie,
    pozicia,
    COUNT(*) AS "Počet"
FROM zamestnanci
GROUP BY CUBE (oddelenie, pozicia);
```

➡️ `CUBE` zobrazí všetky možné kombinácie skupín a ich súčtov.

---

## 🧩 Použitie GROUPING funkcie

```sql
SELECT
    DECODE(GROUPING(oddelenie), 1, 'Spolu', oddelenie) AS oddelenie,
    COUNT(*) AS "Počet"
FROM zamestnanci
GROUP BY ROLLUP (oddelenie);
```

➡️ `GROUPING()` pomáha identifikovať riadky, ktoré reprezentujú medzisúčty.

---

## 📚 Agregácia nad poddotazom

```sql
SELECT AVG(pocet) AS "Priemerný počet v skupinách"
FROM (
    SELECT oddelenie, COUNT(*) AS pocet
    FROM zamestnanci
    GROUP BY oddelenie
);
```

➡️ Vnútorný dotaz zoskupuje a vonkajší počíta priemer zoskupených výsledkov.

