# Zestaw 1 — Od importu bazy do `WHERE`

**Baza:** `world` (plik `world_pl.sql`) | **Narzędzie:** XAMPP → phpMyAdmin → zakładka **SQL**
**Materiał pomocniczy:** ściąga „Budowa bazy danych i SELECT"
**Zakres:** pojęcia bazodanowe, `SELECT`, `AS`, `DISTINCT`, `ORDER BY`, `LIMIT`, `WHERE` (do `LIKE`)

**Tabele w bazie:**

| Tabela | Co zawiera | Kolumny |
|---|---|---|
| `kraje` | 239 państw | `kod`, `nazwa`, `kontynent`, `region`, `powierzchnia`, `rok_niepodleglosci`, `liczba_mieszkancow`, `srednia_dlugosc_zycia`, `pkb`, `ustroj`, `glowa_panstwa`, `stolica`, `kod2` |
| `miasta` | 4079 miast | `id`, `nazwa`, `kod_kraju`, `wojewodztwo`, `liczba_mieszkancow` |
| `jezyki` | 984 wpisy | `kod_kraju`, `jezyk`, `oficjalny` (T/F), `procent` |

**Zasady:** każde zapytanie kończymy średnikiem `;` · tekst zawsze w apostrofach `'POL'` ·
ułamki z **kropką** (`78.5`, nie `78,5`) · po każdym zadaniu sprawdź, **ile wierszy** zwróciło zapytanie
(liczba jest podana nad wynikiem).

---

## Część I — Uruchomienie

**Zadanie 1.** Uruchom XAMPP Control Panel i wystartuj moduły **Apache** oraz **MySQL**.
Otwórz `http://localhost/phpmyadmin`.

**Zadanie 2.** Zakładka **Import** → **Choose file** → wskaż `world_pl.sql` → **Import (Go)**.
Plik sam tworzy bazę — nie twórz jej ręcznie.

**Zadanie 3.** Odśwież panel po lewej. Ile tabel ma baza `world`? Wypisz ich nazwy.

**Zadanie 4.** Kliknij tabelę `miasta`, potem zakładkę **Przeglądaj**. Ile wierszy ma ta tabela?

---

## Część II — Pojęcia na żywej bazie

**Zadanie 5.** W tabeli `miasta` wskaż na ekranie przykład:
a) **kolumny** (atrybutu), b) **rekordu** (wiersza), c) pojedynczego **pola** (komórki).

**Zadanie 6.** Zakładka **Struktura** tabeli `miasta` — który atrybut jest **kluczem głównym**?
Po czym to poznajesz?

**Zadanie 7.** Zakładka **Struktura** tabeli `kraje` — jaki atrybut jest tu kluczem głównym?
Dlaczego twórca bazy nie wybrał do tej roli kolumny `nazwa`?

**Zadanie 8.** W strukturze tabeli `kraje` znajdź **cztery różne typy danych**.
Co oznacza liczba w nawiasie przy `char(52)`? Co oznacza typ `enum`?

**Zadanie 9.** Przejrzyj w **Przeglądaj** kolumnę `pkb` w tabeli `kraje`. Znajdź wiersz,
w którym zamiast liczby jest `NULL`. Czym `NULL` różni się od zera?

---

## Część III — Pierwsze zapytania

**Zadanie 10.** Wyświetl całą tabelę `jezyki`. Co oznacza gwiazdka `*`?

**Zadanie 11.** Wyświetl całą tabelę `kraje`.

**Zadanie 12.** Z tabeli `kraje` wyświetl **tylko** kolumny `nazwa` i `kontynent`.

**Zadanie 13.** Z tabeli `kraje` wyświetl kolumny `kod`, `nazwa` i `srednia_dlugosc_zycia`.

**Zadanie 14.** Z tabeli `miasta` wyświetl kolumny `nazwa` i `wojewodztwo`.

**Zadanie 15.** Powtórz zadanie 14, ale nagłówki mają brzmieć `Miasto` i `Region`.
> Potrzebne: `AS`

**Zadanie 16.** Z tabeli `jezyki` wyświetl `jezyk` jako `Nazwa jezyka` i `procent` jako `Udzial`.

---

## Część IV — Bez powtórzeń

**Zadanie 17.** Wypisz wszystkie **kontynenty** bez powtórzeń. Ile ich jest?

**Zadanie 18.** Wypisz bez powtórzeń wszystkie wartości kolumny `ustroj` w tabeli `kraje`.

**Zadanie 19.** Wypisz bez powtórzeń wszystkie **języki** z tabeli `jezyki`.
Porównaj liczbę wierszy z wynikiem zadania 10 — dlaczego jest mniejsza?

---

## Część V — Sortowanie i LIMIT

**Zadanie 20.** Wyświetl nazwy wszystkich krajów posortowane alfabetycznie (A→Z).

**Zadanie 21.** To samo, ale od Z do A.

**Zadanie 22.** Wyświetl `nazwa` i `powierzchnia` krajów, od **największego** powierzchniowo.

**Zadanie 23.** Wyświetl `nazwa` i `liczba_mieszkancow` miast, od najludniejszego.

**Zadanie 24.** Wyświetl **8 pierwszych** krajów z tabeli (bez sortowania).

**Zadanie 25.** Wyświetl **5 największych powierzchniowo** krajów świata.
> Połącz `ORDER BY ... DESC` z `LIMIT`.

**Zadanie 26.** Wyświetl **10 krajów o najdłuższej średniej długości życia**.

**Zadanie 27.** Wyświetl **10 krajów o najkrótszej średniej długości życia**.
Czy na samej górze listy pojawia się coś dziwnego? Zapamiętaj ten wynik — wrócimy do niego za tydzień.

**Zadanie 28.** Sortowanie po **dwóch** kolumnach: kraje posortowane po `kontynent` (A→Z),
a wewnątrz kontynentu od największej powierzchni.
> Zapis: `ORDER BY kontynent ASC, powierzchnia DESC`

---

## Część VI — Pierwsze warunki

**Zadanie 29.** Wyświetl wszystkie dane **Czech** (`CZE`) z tabeli `kraje`.
> Warunek: `WHERE kod = 'CZE'`. Uwaga — tekst w apostrofach!

**Zadanie 30.** Wyświetl wszystkie miasta **Czech** (kolumna `kod_kraju`). Ile ich jest?

**Zadanie 31.** Wyświetl wszystkie wiersze z tabeli `jezyki` dotyczące **Szwajcarii** (`CHE`).
Ile języków ma ten kraj? Które z nich są oficjalne?

**Zadanie 32.** Wyświetl wszystkie kraje leżące w **Ameryce Południowej**.
> Uwaga: wartość wpisz dokładnie tak, jak wygląda w kolumnie `kontynent`.

---

## Część VII — Porównania

**Zadanie 33.** Wyświetl kraje o powierzchni **większej niż** 1 000 000 km².

**Zadanie 34.** Wyświetl miasta o populacji **większej niż** 7 000 000.

**Zadanie 35.** Wyświetl kraje o średniej długości życia **co najmniej** 80 lat.
> Operator: `>=`

**Zadanie 36.** Wyświetl miasta o populacji **mniejszej niż** 20 000.

**Zadanie 37.** Wyświetl wiersze z tabeli `jezyki`, w których język **nie jest** oficjalny.
> Operator „różne od": `<>`

---

## Część VIII — AND, OR, NOT

**Zadanie 38.** Miasta Czech o populacji **powyżej 100 000**.
> Dwa warunki naraz — `AND`.

**Zadanie 39.** Kraje Europy o powierzchni **poniżej 30 000 km²**.

**Zadanie 40.** Miasta Niemiec (`DEU`) o populacji **powyżej 300 000 i jednocześnie poniżej 700 000**.

**Zadanie 41.** Kraje leżące w **Afryce lub Oceanii**.
> `OR`

**Zadanie 42.** Miasta o nazwie `Praha`, `Brno` lub `Ostrava` — w jednym zapytaniu.

**Zadanie 43.** Kraje, które **nie** leżą w Azji. Wypróbuj dwa zapisy: z `NOT` i z `<>`.
Czy dają ten sam wynik?

**Zadanie 44.** Kraje Europy o powierzchni powyżej 400 000 km² **lub** populacji powyżej 60 000 000.
Jeśli łączysz `AND` z `OR` — pamiętaj o nawiasach.

---

## Część IX — BETWEEN

**Zadanie 45.** Kraje o powierzchni **od 200 000 do 400 000 km²** włącznie. Posortuj rosnąco.
(Czy jest wśród nich Polska?)

**Zadanie 46.** Miasta o populacji **od 1 000 000 do 2 000 000**.

**Zadanie 47.** Kraje, które uzyskały niepodległość **w latach 1960–1970**.
Który kontynent dominuje na tej liście? Dlaczego?

**Zadanie 48.** Zapisz zadanie 45 **bez** `BETWEEN` — używając dwóch warunków połączonych `AND`.
Czy liczba wierszy się zgadza?

---

## Część X — IN

**Zadanie 49.** Kraje o kodach `FRA`, `ITA`, `ESP`, `PRT`, `GRC` — wyświetl nazwę, populację i powierzchnię.

**Zadanie 50.** Miasta leżące w USA, Kanadzie lub Meksyku (`USA`, `CAN`, `MEX`), posortowane malejąco
po populacji, pierwsze 15.

**Zadanie 51.** Kraje leżące w Europie, Azji lub Afryce — zapisz to raz przez `OR`, a raz przez `IN`.
Który zapis jest krótszy?

---

## Część XI — LIKE

**Zadanie 52.** Kraje, których nazwa **zaczyna się na literę A**.
> Wzorzec: `'A%'`. Co oznacza znak `%`?

**Zadanie 53.** Kraje, których nazwa **kończy się na „land"** (np. Poland, Finland).

**Zadanie 54.** Kraje, których nazwa **zawiera** słowo `United`.

**Zadanie 55.** Miasta Niemiec, których nazwa **kończy się na „burg"**.

**Zadanie 56.** Kraje, których nazwa ma na **trzeciej pozycji literę r**.
> Wzorzec: `'__r%'`. Czym różni się `_` od `%`?

---

## Część XII — Zadania dodatkowe (dla szybszych)

**Zadanie 57.** Miasta Polski o populacji powyżej 200 000, posortowane alfabetycznie,
z nagłówkami `Miasto` i `Ludnosc`.

**Zadanie 58.** Kraje Europy o średniej długości życia powyżej 79 lat, posortowane malejąco,
pierwsze 10.

**Zadanie 59.** Wpisy z tabeli `jezyki`, które są oficjalne **i** dotyczą ponad 90% mieszkańców.
Posortuj malejąco po `procent`, pierwsze 20.

**Zadanie 60.** Kraje, których `ustroj` zawiera słowo `Monarchy` i które leżą w Europie.

**Zadanie 61.** Miasta o nazwie składającej się **dokładnie z 4 znaków**.
> Podpowiedź: cztery znaki `_` i żadnego `%`.

---

## Pytania kontrolne

1. Czym różni się rekord od kolumny?
2. Po co tabeli klucz główny?
3. Co robi `WHERE`, a co `ORDER BY`?
4. Kiedy użyjemy `DISTINCT`?
5. Jak zapisać `BETWEEN 10 AND 20` bez użycia `BETWEEN`?
6. Co oznaczają znaki `%` i `_` w `LIKE`?
