# Lekcja 2 — Podstawowe pojęcia dotyczące baz danych w praktyce

**Temat (jednostka metodyczna):** Podstawowe pojęcia dotyczące baz danych — praca z gotową bazą w XAMPP/phpMyAdmin
**Efekty kształcenia:** INF.03.4 (1), INF.03.4 (3), INF.03.4 (4)
**Czas:** 45 min | **Stanowisko:** XAMPP (Apache + MySQL), phpMyAdmin
**Materiały:** plik `Bazy/world_pl.sql` (oficjalna przykładowa baza MySQL „world" z polskimi nazwami tabel i kolumn: `miasta`, `kraje`, `jezyki`)

**Cele lekcji — uczeń:**
- uruchamia serwer MySQL w XAMPP i importuje gotową bazę z pliku .sql,
- wskazuje na realnej bazie pojęcia: baza, tabela, kolumna (atrybut), rekord, klucz główny,
- pisze proste zapytania `SELECT` (wszystkie kolumny, wybrane kolumny, warunek WHERE, sortowanie, LIMIT, DISTINCT).

---

## Część I — Przygotowanie środowiska

**Zadanie 1.** Uruchom XAMPP Control Panel. Wystartuj moduły **Apache** i **MySQL** (przyciski Start). Otwórz w przeglądarce `http://localhost/phpmyadmin`.

**Zadanie 2.** W phpMyAdmin wybierz zakładkę **Import**, kliknij **Choose file** i wskaż plik `world_pl.sql`. Kliknij **Import (Go)**.
*Wskazówka:* plik sam tworzy bazę `world` — nie musisz tworzyć jej ręcznie.

**Zadanie 3.** Odśwież panel po lewej stronie. Sprawdź, jakie tabele zawiera baza `world`. Ile ich jest?

---

## Część II — Pojęcia bazodanowe „na żywo"

**Zadanie 4.** Otwórz tabelę `miasta` (kliknij jej nazwę), zakładka **Przeglądaj**. Wskaż na ekranie:
a) jedną **kolumnę** (atrybut),
b) jeden **rekord** (wiersz),
c) **klucz główny** tabeli (zakładka **Struktura** — który atrybut ma oznaczenie klucza?).
Zapisz w zeszycie definicje: tabela, rekord, kolumna, klucz główny.

**Zadanie 5.** Tak samo zbadaj tabelę `kraje`. Jaki atrybut jest tu kluczem głównym? Dlaczego nie mógł być nim `nazwa`?
*Wskazówka:* pomyśl, czy nazwa kraju zawsze jest unikalna i czy nigdy się nie zmienia.

**Zadanie 6.** W zakładce **Struktura** tabeli `kraje` przejrzyj **typy danych** kolumn. Zapisz 3 przykłady typów, które widzisz (np. `char(52)`, `int`, `enum`, `decimal`). Co oznacza liczba w nawiasie przy `char`?

---

## Część III — Pierwsze zapytania SELECT

Wszystkie zapytania wykonuj w zakładce **SQL** (najpierw zaznacz bazę `world` w panelu po lewej).

**Zadanie 7.** Wyświetl **wszystkie dane** tabeli miast:
```sql
SELECT * FROM miasta;
```
Ile wierszy zwróciło zapytanie? Co oznacza gwiazdka `*`?

**Zadanie 8.** Wyświetl **tylko nazwę i populację** miast (kolumny `nazwa`, `liczba_mieszkancow`).

**Zadanie 9.** Nadaj kolumnom polskie nagłówki używając aliasu `AS`: `Miasto`, `LiczbaMieszkancow`.

**Zadanie 10.** Wyświetl **tylko 10 pierwszych** miast (klauzula `LIMIT`).

**Zadanie 11.** Wyświetl wszystkie kraje posortowane **alfabetycznie** (`ORDER BY nazwa`). A potem **od końca** (`ORDER BY nazwa DESC`).

**Zadanie 12.** Wyświetl 5 miast o **największej** populacji (połącz `ORDER BY ... DESC` z `LIMIT`).
*Oczekiwany wynik:* na liście powinno być miasto nr 1 świata z tej bazy — Mumbai (10 500 000 mieszkańców).

**Zadanie 13.** Wyświetl listę kontynentów bez powtórzeń:
```sql
SELECT DISTINCT kontynent FROM kraje;
```
Ile jest wartości? (To kolumna typu `enum` — porównamy z listą w zakładce Struktura.)

---

## Część IV — Zadania dodatkowe (dla szybszych)

**Zadanie 14.** Wyświetl wszystkie miasta Polski (`WHERE kod_kraju = 'POL'`). Ile ich jest?

**Zadanie 15.** Wyświetl miasta Polski, w których mieszka **więcej niż 300 000** osób. Posortuj malejąco po populacji.

**Zadanie 16.** Wyświetl dane Polski z tabeli `kraje`: nazwę, powierzchnię (`powierzchnia`), populację i długość życia (`srednia_dlugosc_zycia`).

**Zadanie 17.** Znajdź w bazie Warszawę (`WHERE nazwa = 'Warszawa'`). Zapisz jej `id` i `wojewodztwo`.

---

## Pytania kontrolne (na koniec lekcji)
1. Czym różni się rekord od kolumny?
2. Po co tabela klucz główny?
3. Co robi klauzula `WHERE`, a co `ORDER BY`?
4. Kiedy użyjemy `DISTINCT`?

---

# Odpowiedzi dla nauczyciela (usunąć przed wydrukiem)

- **Z3:** 3 tabele: `miasta`, `kraje`, `jezyki`.
- **Z4:** klucz główny `miasta` to `id` (INT, AUTO_INCREMENT).
- **Z5:** klucz główny `kraje` to `kod` (3-literowy kod kraju). Nazwa `nazwa` teoretycznie mogłaby się zmienić/powtórzyć (państwa o podobnych nazwach), kod jest stabilny i unikalny.
- **Z6:** m.in. `char(52)` — tekst o stałej maks. długości 52 znaki, `int`, `decimal(10,2)`, `enum(...)` — lista dozwolonych wartości, `smallint`.
- **Z7:** zapytanie zwraca wszystkie kolumny i wszystkie wiersze (4079 miast); `*` = „wszystkie kolumny".
- **Z8:** `SELECT nazwa, liczba_mieszkancow FROM miasta;`
- **Z9:** `SELECT nazwa AS Miasto, liczba_mieszkancow AS LiczbaMieszkancow FROM miasta;`
- **Z10:** `SELECT * FROM miasta LIMIT 10;`
- **Z11:** `SELECT * FROM kraje ORDER BY nazwa;` / `... ORDER BY nazwa DESC;`
- **Z12:** `SELECT nazwa, liczba_mieszkancow FROM miasta ORDER BY liczba_mieszkancow DESC LIMIT 5;` — 1. Mumbai (Bombay) 10 500 000.
- **Z13:** 7 kontynentów (Azja, Europa, Ameryka Północna, Afryka, Oceania, Antarktyda, Ameryka Południowa) — zgodnie z definicją `enum` w strukturze tabeli.
- **Z14:** 44 wiersze.
- **Z15:** `SELECT nazwa, liczba_mieszkancow FROM miasta WHERE kod_kraju='POL' AND liczba_mieszkancow > 300000 ORDER BY liczba_mieszkancow DESC;` (np. Warszawa 1 615 369, Łódź 800 110, Kraków 738 150, Wrocław, Poznań, Gdańsk, Szczecin, Bydgoszcz, Lublin, Katowice).
- **Z16:** `SELECT nazwa, powierzchnia, liczba_mieszkancow, srednia_dlugosc_zycia FROM kraje WHERE kod='POL';` — powierzchnia 323 250 km², populacja 38 653 600, życie 73,2.
- **Z17:** Warszawa: `id = 2928`, `wojewodztwo = Mazowieckie`.
