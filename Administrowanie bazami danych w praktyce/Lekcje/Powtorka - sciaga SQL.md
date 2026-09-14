# Szybka powtórka — SQL (lekcje 2–4)

## 1. Pojęcia bazodanowe
- **Tabela** = zbiór rekordów (wierszy) i kolumn (atrybutów)
- **Klucz główny (PK)** — unikalny, stabilny identyfikator wiersza (np. `miasta.id`, `kraje.kod`)
- **Klucz obcy (FK)** — kolumna wskazująca PK innej tabeli (np. `kraje.stolica` → `miasta.id`, `jezyki.kod_kraju` → `kraje.kod`). Tak działa relacja.

## 2. Typy danych
- `int` — liczby całkowite (ludność) | `decimal(10,2)` — z częścią dziesiętną (powierzchnia, PKB)
- `char(52)` — tekst o **stałej** maks. długości | `smallint` — małe całkowite (rok)
- `enum(...)` — tylko wartości z listy → spójność danych (kontynenty)
- **NULL = brak wartości** — nie równa się niczemu, nawet sobie

## 3. Szkielet zapytania i kolejność wykonywania

```sql
SELECT kolumny, agregaty          -- 6
FROM tabela                       -- 1
WHERE warunek_na_wiersze          -- 2  (PRZED grupowaniem, nie zna aliasów!)
GROUP BY kolumna_grupujaca        -- 3
HAVING warunek_na_grupy           -- 4  (PO agregacji, zna aliasy)
ORDER BY ... ASC|DESC             -- 7  (wielokluczowe: ORDER BY a, b DESC)
LIMIT n OFFSET m;                 -- 8  (OFFSET 10 = pomiń 10 pierwszych)
```

## 4. Warunki w WHERE
- Porównania: `=`, `<>`, `>`, `<`
- Łączenie: `AND`, `OR`, `NOT`
- `BETWEEN 100000 AND 200000` — **włącznie** z krańcami
- `IN ('POL','DEU','CZE')` — zamiast długiego łańcucha OR
- `LIKE 'W%'` — zaczyna się na W | `'%w'` — kończy na w | `'_a%'` — druga litera a
  - **`%`** = dowolny ciąg znaków (także pusty), **`_`** = dokładnie 1 znak
- `IS NULL` / `IS NOT NULL` — **nigdy `= NULL`** (porównanie z NULL zwraca nic!)

## 5. Funkcje
- Tekstowe: `UPPER`, `LOWER`, `LENGTH`, `CONCAT(kod, ' — ', nazwa)`
- `ROUND(wyrażenie, 0)` — zaokrąglenie; arytmetyka na kolumnach: `pkb * 1000000 / liczba_mieszkancow`
- Warunek na wyniku wyrażenia → ta sama formuła w `WHERE` (i pilnuj `IS NOT NULL` przy dzieleniu!)

## 6. Agregacje
- `COUNT(*)` — liczy wszystkie wiersze | `COUNT(kolumna)` — **pomija NULL-e** (stąd różne wyniki)
- `COUNT(DISTINCT kolumna)` — ile różnych wartości
- `SUM`, `AVG`, `MIN`, `MAX` — agregaty też ignorują NULL
- Bez `GROUP BY` → agregacja na **całej tabeli** (1 wiersz wyniku)

## 7. WHERE vs HAVING — najczęstsza pułapka

| | WHERE | HAVING |
|---|---|---|
| Filtruje | pojedyncze **wiersze** | **grupy** |
| Kiedy | przed grupowaniem | po agregacji |
| Agregaty/aliasy | ❌ nie wolno | ✅ tak |

Przykład pełny:

```sql
SELECT wojewodztwo,
       COUNT(*) AS Miasta,
       SUM(liczba_mieszkancow) AS Ludnosc
FROM miasta
WHERE kod_kraju = 'POL'
GROUP BY wojewodztwo
HAVING Miasta >= 2
ORDER BY Ludnosc DESC
LIMIT 5;
```
