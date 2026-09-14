# Lekcja 4 — Typy danych w praktyce: agregacje, grupowanie (GROUP BY / HAVING)

**Temat (jednostka metodyczna):** Typy danych stosowanych w bazach danych — analiza struktury i podsumowanie danych funkcjami agregującymi
**Efekty kształcenia:** INF.03.4 (1), INF.03.4 (4)
**Czas:** 45 min | **Stanowisko:** XAMPP, phpMyAdmin, baza `world`

**Cele lekcji — uczeń:**
- rozpoznaje typy danych w strukturze tabel i uzasadnia ich dobór (`int`, `smallint`, `decimal`, `char`, `enum`, NULL),
- stosuje funkcje agregujące: `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`,
- grupuje dane (`GROUP BY`) i filtruje grupy (`HAVING`),
- składa pełne zapytanie: `WHERE` + `GROUP BY` + `HAVING` + `ORDER BY` + `LIMIT`.

**Cała lekcja bez `JOIN`** — grupujemy w ramach jednej tabeli.

---

## Część I — Typy danych „na żywo" (powtórka jednostki 4)

**Zadanie 1.** Otwórz zakładkę **Struktura** tabeli `kraje` i uzupełnij tabelkę w zeszycie:

| Kolumna | Typ danych | Dlaczego ten typ jest dobry? |
|---|---|---|
| `kod` | char(3) | zawsze dokładnie 3 znaki |
| `nazwa` | char(52) | |
| `kontynent` | enum(...) | |
| `powierzchnia` | decimal(10,2) | |
| `rok_niepodleglosci` | smallint | |
| `liczba_mieszkancow` | int | |
| `srednia_dlugosc_zycia` | decimal(3,1) | |
| `PKB` | decimal(10,2) | |

**Zadanie 2.** Odpowiedz ustnie/słownie:
a) Czym różni się `int` od `decimal(10,2)`? Dlaczego `liczba_mieszkancow` może być `int`, a `powierzchnia` już nie?
b) Co oznacza `NULL` w kolumnie `srednia_dlugosc_zycia`? Które kraje tak mają?
c) Po co jest `enum` zamiast `char`? Jakie wartości dopuszcza kolumna `kontynent`?
d) Dlaczego w `miasta` kolumna `wojewodztwo` jest typu `char(20)`, a `liczba_mieszkancow` typu `int`?

---

## Część II — Funkcje agregujące na całej tabeli

**Zadanie 3.** Ile jest miast na świecie?
```sql
SELECT COUNT(*) FROM miasta;
```
A ile krajów? A ile wpisów języków (`jezyki`)?

**Zadanie 4.** Różnica `COUNT(*)` vs `COUNT(kolumna)`:
```sql
SELECT COUNT(*), COUNT(srednia_dlugosc_zycia) FROM kraje;
```
Dlaczego wyniki są **różne**? Zapisz wniosek (funkcja agregująca ignoruje `NULL`).

**Zadanie 5.** Statystyki świata w jednym zapytaniu:
```sql
SELECT MAX(liczba_mieszkancow), MIN(liczba_mieszkancow), AVG(liczba_mieszkancow), SUM(liczba_mieszkancow)
FROM kraje;
```
Jaki kraj ma maksimum? (sprawdź znane Ci zapytaniem z `ORDER BY ... LIMIT 1`).

**Zadanie 6.** `COUNT(DISTINCT ...)` — ile **różnych** województw/dystryktów występuje w tabeli miast dla Polski?
```sql
SELECT COUNT(DISTINCT wojewodztwo) FROM miasta WHERE kod_kraju = 'POL';
```

---

## Część III — GROUP BY

**Zadanie 7.** Liczba miast per województwo (Polska):
```sql
SELECT wojewodztwo, COUNT(*) AS LiczbaMiast
FROM miasta
WHERE kod_kraju = 'POL'
GROUP BY wojewodztwo
ORDER BY LiczbaMiast DESC;
```
Które województwo ma najwięcej miast w bazie?

**Zadanie 8.** Liczba mieszkańców per województwo (Polska) — użyj `SUM(liczba_mieszkancow)` i `AVG(liczba_mieszkancow)` w grupach. Posortuj po sumie malejąco. Które województwo jest najludniejsze?

**Zadanie 9.** Liczba krajów per kontynent:
```sql
SELECT kontynent, COUNT(*) AS Kraje
FROM kraje
GROUP BY kontynent
ORDER BY Kraje DESC;
```

**Zadanie 10.** Liczba mieszkańców per kontynent (`SUM(liczba_mieszkancow)`). Który kontynent jest najludniejszy, a który najmniej?

**Zadanie 11.** Tabela `jezyki` — policz, ile krajów ma wpisany **każdy** język:
```sql
SELECT Language, COUNT(*) AS Kraje
FROM jezyki
GROUP BY Language
ORDER BY Kraje DESC
LIMIT 5;
```
Który język występuje w największej liczbie krajów?

**Zadanie 12.** W tabeli `miasta` pogrupuj miasta Polski po `wojewodztwo` i wyświetl dla każdej grupy `MAX(liczba_mieszkancow)` — czyli największe miasto każdego województwa.

---

## Część IV — HAVING i pełny schemat zapytania

**Zadanie 13.** Kontynenty z **więcej niż 40 krajami**:
```sql
SELECT kontynent, COUNT(*) AS Kraje
FROM kraje
GROUP BY kontynent
HAVING Kraje > 40
ORDER BY Kraje DESC;
```
Dlaczego nie można było napisać `WHERE Kraje > 40`?

**Zadanie 14.** Województwa Polski, które w bazie mają **więcej niż 2 miasta**, posortowane malejąco.

**Zadanie 15.** Kontynenty, których **łączna** populacja przekracza 500 mln.

**Zadanie 16.** Pełny schemat — połącz wszystko:
```sql
SELECT wojewodztwo, COUNT(*) AS Miasta, SUM(liczba_mieszkancow) AS Ludnosc
FROM miasta
WHERE kod_kraju = 'POL'      -- filtr wierszy PRZED grupowaniem
GROUP BY wojewodztwo              -- tworzenie grup
HAVING Miasta >= 2             -- filtr grup PO grupowaniu
ORDER BY Ludnosc DESC          -- sortowanie wyniku
LIMIT 5;                       -- ograniczenie liczby wierszy
```
Przepisz zapytanie i opisz słowami, na jakiej kolejności działa każda klauzula.

---

## Pytania kontrolne (na koniec lekcji)
1. Wymień 5 funkcji agregujących i opisz, co robią.
2. Czym różni się `WHERE` od `HAVING`?
3. Dlaczego `COUNT(kolumna)` może dać mniejszy wynik niż `COUNT(*)`?
4. Podaj przykład: jaki typ danych byłby odpowiedni dla kolumny „cena" w sklepie i dlaczego?

---

# Odpowiedzi dla nauczyciela (usunąć przed wydrukiem)

- **Z1:** kontynuacja tabelki: `nazwa` — tekst o zmiennej długości do 52 znaków; `enum` — tylko wartości z listy (7 kontynentów); `decimal(10,2)` — liczba z częścią dziesiętną (km²); `smallint` — mała liczba całkowita (rok 4 cyfry się zmieści); `int` — ludność to liczby całkowite; `decimal(3,1)` — np. 73,2 lat; `PKB` — wartość w mln $ z groszami.
- **Z2a:** `int` bez części dziesiętnej — ludność liczona w osobach; powierzchnia bywa zapisywana z dokładnością do 0,01 km².
- **Z2b:** brak danych — 17 terytoriów (m.in. Antarctica, French Southern territories, Holy See, Pitcairn).
- **Z2c:** `enum` wymusza jedną z wartości listy → spójność danych, mniejsze ryzyko literówek („Azja" vs „azja" vs „Asia").
- **Z3:** `miasta` 4079, `kraje` 239, `jezyki` 984.


- **Z4:** `COUNT(*)` liczy wszystkie wiersze (239), `COUNT(srednia_dlugosc_zycia)` pomija wiersze z NULL (239 − 17 = 222). Wniosek: agregaty ignorują NULL.
- **Z5:** MAX to China (1 277 558 000), MIN to Antarktyda (0), AVG ~25,4 mln; suma wszystkich to ~6,08 mld. Minimum potwierdź zapytaniem `ORDER BY liczba_mieszkancow ASC LIMIT 1`.
- **Z6:** 16 województw.
- **Z7:** Śląskie — aż 14 miast (baza zawiera całe konurbacje), dalej Dolnośląskie i Kujawsko-Pomorskie (po 4), Mazowieckie i Pomorskie (po 3). Łącznie 44 miasta POL.
- **Z8:** najludniejsze Śląskie (~2,54 mln w grupach: Katowice, Sosnowiec, Gliwice, Bytom, Zabrze, Częstochowa...), dopiero potem Mazowieckie (~1,98 mln: Warszawa 1 615 369 + Radom 232 262 + Płock 131 011). Dobry moment na dyskusję: baza liczy miasta granicznymi granicami konurbacji, więc „województwo śląskie wygrywa".
- **Z9:** najwięcej: Afryka (58), potem Azja (51), Europa (46), Ameryka Północna (37), Oceania (28), Ameryka Południowa (14), Antarktyda (5).
- **Z10:** najludniejszy Azja (~3,7 mld), potem Afryka (~784 mln), Europa (~730 mln); najmniej Antarktyda (0).
- **Z11:** 1. English (60 krajów), potem Arabic (33), Spanish (28), French (25), German i Chinese (po 19).
- **Z12:** `SELECT wojewodztwo, MAX(liczba_mieszkancow) FROM miasta WHERE kod_kraju='POL' GROUP BY wojewodztwo;`
- **Z13:** `WHERE` filtruje pojedyncze wiersze **przed** grupowaniem i nie zna aliasów agregatów; `HAVING` filtruje **grupy** po agregacji.
- **Z14:** `HAVING COUNT(*) > 2` — 5 województw: Śląskie (14 miast), Dolnośląskie (4), Kujawsko-Pomorskie (4), Mazowieckie (3), Pomorskie (3).
- **Z15:** `HAVING SUM(liczba_mieszkancow) > 500000000` — 3 kontynenty: Azja (~3,7 mld), Afryka (~784 mln), Europa (~730 mln). Uwaga: Ameryka Północna ma ~483 mln — celowy „pułapkaż" pokazujący, że trzeba policzyć, a nie zgadywać.
- **Z16:** kolejność logiczna wykonywania: `FROM` → `WHERE` → `GROUP BY` → agregacje → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`.
