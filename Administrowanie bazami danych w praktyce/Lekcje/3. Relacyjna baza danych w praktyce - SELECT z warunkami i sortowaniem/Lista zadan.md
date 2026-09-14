# Lekcja 3 — Relacyjna baza danych w praktyce: SELECT z warunkami, sortowaniem i funkcjami

**Temat (jednostka metodyczna):** Relacyjne bazy danych — wyszukiwanie informacji przy użyciu języka SQL (cz. 1)
**Efekty kształcenia:** INF.03.4 (1), INF.03.4 (4)
**Czas:** 45 min | **Stanowisko:** XAMPP, phpMyAdmin, baza `world` (zaimportowana na lekcji 2)

**Cele lekcji — uczeń:**
- pokazuje na danych, jak tabele są ze sobą powiązane (klucz główny ↔ klucz obcy),
- stosuje w `WHERE` operatory: porównania, `AND`/`OR`/`NOT`, `BETWEEN`, `IN`, `LIKE`, `IS NULL`,
- sortuje wyniki wielokluczowo, stosuje stronicowanie (`LIMIT`/`OFFSET`),
- używa prostych funkcji: `UPPER`, `LOWER`, `CONCAT`, `ROUND` oraz arytmetyki na kolumnach.

**Bez `JOIN`** — powiązania między tabelami pokazujemy „ręcznie", wartością klucza.

---

## Część I — Jak tabele są powiązane (bez JOIN-a)

**Zadanie 1.** Zobacz w tabeli `kraje` wiersz Polski — kolumna `stolica` zawiera **liczbę** (u nas: 2928). Co to za liczba?
Sprawdź:
```sql
SELECT * FROM miasta WHERE id = 2928;
```
**Wniosek do zeszytu:** `kraje.stolica` wskazuje na `miasta.id`. Kolumna wskazująca to **klucz obcy**, a `miasta.id` to **klucz główny**. Tak działa relacja w bazie relacyjnej.

**Zadanie 2.** Wypisz z tabeli `kraje` 5 krajów z ich kolumną `stolica`, a następnie dla każdego sprawdz zapytaniem, jak to miasto się nazywa. Zapisz pary: kraj → stolica.

**Zadanie 3.** Tabela `jezyki` też jest powiązana — jej kolumna `kod_kraju` wskazuje na `kraje.kod`. Wyświetl wszystkie wiersze dotyczące Polski:
```sql
SELECT * FROM jezyki WHERE kod_kraju = 'POL';
```
Który język ma wartość `oficjalny = 'T'`?

---

## Część II — Warunki w WHERE

**Zadanie 4.** Operatory porównania. Wyświetl miasta o populacji **dokładnie** równej 1000000. A potem miasta o populacji **różnej od** 0 (`<>`).

**Zadanie 5.** `AND` — miasta Polski o populacji **powyżej 200 000 i poniżej 500 000**.

**Zadanie 6.** `OR` — miasta o nazwie `Warszawa` **lub** `Kraków` **lub** `Gdansk` (zapisz warunek jednym zapytaniem).

**Zadanie 7.** `BETWEEN` — kraje o powierzchni (`powierzchnia`) **od 100 000 do 200 000 km²** włącznie. Posortuj rosnąco.

**Zadanie 8.** `IN` — kraje o kodach: Polska, Niemcy (`DEU`), Czechy (`CZE`), Słowacja (`SVK`), Litwa (`LTU`). Wyświetl ich nazwy, populacje i stolicę.

**Zadanie 9.** `LIKE` — wyświetl miasta Polski, których nazwa:
a) zaczyna się na **W** (`'W%'`),
b) kończy się na **w** (`'%w'`),
c) ma na **drugiej** pozycji literę **a** (`'_a%'`).
Co oznaczają znaki `%` i `_`?

**Zadanie 10.** `IS NULL` — wyświetl kraje, które **nie mają** wpisanej wartości `srednia_dlugosc_zycia`:
```sql
SELECT nazwa, srednia_dlugosc_zycia FROM kraje WHERE srednia_dlugosc_zycia IS NULL;
```
Ile takich krajów/terytoriów jest? Dlaczego zapis `= NULL` **nie działa**? (zapisz wniosek)

**Zadanie 11.** Połącz warunki: kraje, które mają wpisaną oczekiwaną długość życia (`IS NOT NULL`) **i** wynosi ona **poniżej 55 lat**. Posortuj po `srednia_dlugosc_zycia` rosnąco.

---

## Część III — Sortowanie i stronicowanie

**Zadanie 12.** Wyświetl 10 najludniejszych **krajów** świata (`ORDER BY liczba_mieszkancow DESC LIMIT 10`). Czy Chiny są na 1. miejscu?

**Zadanie 13.** Sortowanie dwukluczowe: kraje posortowane po kontynencie **A→Z**, a w ramach kontynentu od **najludniejszego**:
```sql
SELECT nazwa, kontynent, liczba_mieszkancow FROM kraje
ORDER BY kontynent ASC, liczba_mieszkancow DESC;
```

**Zadanie 14.** Stronicowanie: wyświetl miasta na pozycjach 11–20 (wg alfabetycznej listy nazw). Użyj `ORDER BY nazwa LIMIT 10 OFFSET 10`.

---

## Część IV — Funkcje i arytmetyka na kolumnach

**Zadanie 15.** Funkcje tekstowe:
```sql
SELECT nazwa, UPPER(nazwa), LOWER(nazwa), LENGTH(nazwa) FROM kraje LIMIT 5;
```
Co zwraca każda z funkcji?

**Zadanie 16.** `CONCAT` — sklej kod i nazwę kraju w jedną kolumnę, np. `POL — Poland`:
```sql
SELECT CONCAT(kod, ' — ', nazwa) AS Kraj FROM kraje LIMIT 5;
```

**Zadanie 17.** Arytmetyka na kolumnach — „PKB na mieszkańca" (PKB w dolarach / populacja):
```sql
SELECT nazwa, pkb, liczba_mieszkancow,
       ROUND(pkb * 1000000 / liczba_mieszkancow, 0) AS pkb_na_mieszk
FROM kraje
WHERE kod IN ('POL', 'DEU', 'NOR', 'UKR')
ORDER BY pkb_na_mieszk DESC;
```
(Uwaga: w bazie `pkb` jest w milionach — dlatego mnożymy przez 1 000 000.) Który z tych krajów ma najwyższy wynik?

**Zadanie 18.** Warunek na wyniku wyrażenia: kraje z pkb na mieszkańca powyżej 20 000 $ (ta sama formuła w `WHERE`), posortowane malejąco, pierwsze 10.

---

## Pytania kontrolne (na koniec lekcji)
1. Czym różni się klucz główny od klucza obcego?
2. Kiedy w `WHERE` użyjemy `IS NULL`, a nie `= NULL`?
3. Do czego służą `%` i `_` w `LIKE`?
4. Jak wyświetlić „drugą stronę" listy po 10 wierszy?

---

# Odpowiedzi dla nauczyciela (usunąć przed wydrukiem)

- **Z1:** 2928 = id Warszawy w tabeli `miasta`.
- **Z2:** przykładowo POL→Warszawa (2928), DEU→Berlin (3068), USA→Washington (3813), FRA→Paris (2974), GBR→London (456).
- **Z3:** `Polish` ma `oficjalny = 'T'` (97,6% mieszkańców).
- **Z4:** `SELECT * FROM miasta WHERE liczba_mieszkancow = 1000000;` oraz `... WHERE liczba_mieszkancow <> 0;`
- **Z5:** `SELECT nazwa, liczba_mieszkancow FROM miasta WHERE kod_kraju='POL' AND liczba_mieszkancow > 200000 AND liczba_mieszkancow < 500000;` — np. Gdańsk 458 988, Szczecin 416 988, Bydgoszcz 386 855, Lublin 356 251, Katowice 345 934.
- **Z6:** `WHERE nazwa = 'Warszawa' OR nazwa = 'Kraków' OR nazwa = 'Gdansk';`
- **Z7:** `SELECT nazwa, powierzchnia FROM kraje WHERE powierzchnia BETWEEN 100000 AND 200000 ORDER BY powierzchnia;` — 23 kraje, m.in. Islandia, Gwatemala, Kuba, Bułgaria, Grecja, Tadżykistan, Bangladesz, Nepal, Tunezja, Urugwaj, Kambodża, Syria, Kirgistan (opcjonalnie: `BETWEEN` można zapisać jako dwa warunki `AND`).
- **Z8:** `SELECT nazwa, liczba_mieszkancow, stolica FROM kraje WHERE kod IN ('POL','DEU','CZE','SVK','LTU');`
- **Z9:** a) Warszawa, Wloclawek, Walbrzych; b) np. Tarnów, Chorzów, Jelenia Góra (kończą się na „w"!); c) np. Warszawa, Katowice, Radom, Jaworzno, Kalisz. `%` = dowolny ciąg znaków (także pusty), `_` = dokładnie jeden znak.
- **Z10:** 17 wierszy — głównie małe terytoria zależne i okolice biegunów (Antarctica, French Southern territories, Bouvet Island, Holy See, Norfolk Island, Pitcairn...). Wniosek: `NULL` to „brak wartości", nie jest równy niczemu — nawet sobie; dlatego porównanie `= NULL` zwraca nic i trzeba używać `IS NULL`.
- **Z11:** `SELECT nazwa, srednia_dlugosc_zycia FROM kraje WHERE srednia_dlugosc_zycia IS NOT NULL AND srednia_dlugosc_zycia < 55 ORDER BY srednia_dlugosc_zycia;` — na początku lista krajów Afryki (najniżej ~37–40 lat).
- **Z12:** 1. China ([1 277 558 000]), potem India, USA, Indonezja, Brazylia...
- **Z13:** zapytanie jak w treści; w ramach każdego kontynentu kraje malejąco po populacji.
- **Z14:** `SELECT nazwa FROM miasta ORDER BY nazwa LIMIT 10 OFFSET 10;` (składnia alternatywna: `LIMIT 10, 10`).
- **Z15:** `UPPER` — wielkie litery, `LOWER` — małe, `LENGTH` — liczba znaków nazwy.
- **Z16:** zapytanie jak w treści.
- **Z17:** Norwegia najwyższy wynik, potem Niemcy, Polska, najniżej Ukraina.
- **Z18:** `SELECT nazwa, ROUND(pkb*1000000/liczba_mieszkancow) AS pkb_na_mieszk FROM kraje WHERE pkb IS NOT NULL AND ROUND(pkb*1000000/liczba_mieszkancow) > 20000 ORDER BY pkb_na_mieszk DESC LIMIT 10;` (uwaga na `PKB IS NOT NULL` — kraje z NULL-em muszą być odfiltrowane).
