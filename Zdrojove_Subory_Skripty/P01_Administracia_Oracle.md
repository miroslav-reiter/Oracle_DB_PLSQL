
# 📘 P01 – Administrácia Oracle DB (SQL príkazy)

Ukážky SQL príkazov pre správu databázových objektov, používateľov a schém v Oracle. Vhodné pre administrátorov a vývojárov.

---

## 🗃️ Zoznam všetkých tabuliek v aktuálnom používateľskom schéme

```sql
SELECT table_name AS "Tabuľka"
FROM user_tables;
```

➡️ Zobrazí všetky tabuľky, ktoré vlastní aktuálny používateľ (USER).

---

## 🏢 Zoznam všetkých tabuliek vo všetkých schémach

```sql
SELECT owner AS "Vlastník", table_name AS "Tabuľka"
FROM all_tables;
```

➡️ Zobrazí všetky tabuľky, ku ktorým má používateľ prístup (vrátane iných schém).

---

## 🧾 Zoznam všetkých používateľov (DBA)

```sql
SELECT username AS "Používateľ", account_status AS "Stav účtu", created
FROM dba_users;
```

➡️ Vyžaduje oprávnenie DBA. Zobrazí všetkých používateľov v Oracle databáze.

---

## 🔑 Zoznam priradených rolí pre používateľa

```sql
SELECT * 
FROM dba_role_privs 
WHERE grantee = 'SYSTEM';
```

➡️ Zobrazí všetky roly priradené používateľovi `SYSTEM`.

---

## 📜 Zoznam objektových práv pre používateľa

```sql
SELECT * 
FROM dba_tab_privs 
WHERE grantee = 'SYSTEM';
```

➡️ Ukáže oprávnenia na objekty udelené používateľovi `SYSTEM`.

---

## 🧩 Zoznam všetkých schém

```sql
SELECT username AS "Schéma"
FROM all_users;
```

➡️ V Oracle sú schémy identické s používateľmi – každý používateľ má vlastnú schému.

---

## 📂 Zoznam indexov v aktuálnej schéme

```sql
SELECT index_name, table_name, uniqueness 
FROM user_indexes;
```

➡️ Zobrazí všetky indexy vytvorené aktuálnym používateľom.

---

## 🧱 Zoznam pohľadov (views)

```sql
SELECT view_name 
FROM user_views;
```

➡️ Zobrazí všetky pohľady, ktoré vlastní aktuálny používateľ.

---

## 🛠️ Zoznam sekvencií

```sql
SELECT sequence_name 
FROM user_sequences;
```

➡️ Zoznam všetkých sekvencií v schéme aktuálneho používateľa.

---

## 🧠 Zoznam uložených procedúr a funkcií

```sql
SELECT object_name, object_type 
FROM user_objects
WHERE object_type IN ('PROCEDURE', 'FUNCTION');
```

➡️ Vráti všetky procedúry a funkcie v aktuálnej schéme.
