# Zestaw 2 — Utrwalenie i domknięcie tematu

**Baza:** `world` | **Narzędzie:** phpMyAdmin → zakładka **SQL**
**Materiał pomocniczy:** ściąga „Budowa bazy danych i SELECT"

**Części I–V** — powtórka wszystkiego, co już umiemy (`SELECT`, `AS`, `DISTINCT`, `ORDER BY`,
`LIMIT`, `WHERE`, `AND`/`OR`, `BETWEEN`, `IN`, `LIKE`).
**Części VI–X** — trzy nowe rzeczy: `IS NULL`, funkcje tekstowe i arytmetyka na kolumnach oraz `OFFSET`.

**Przypomnienie zasad:** średnik `;` na końcu · tekst w apostrofach · ułamki z **kropką** ·
`ORDER BY` zawsze po `WHERE`, `LIMIT` na samym końcu.

---

## Część I — Rozgrzewka: wybieranie kolumn

**Zadanie 1.** Wyświetl całą tabelę `kraje`.

**Zadanie 2.** Wyświetl tylko `nazwa` i `region` krajów.

**Zadanie 3.** To samo, ale nagłówki mają brzmieć `Panstwo` i `Region swiata`.

**Zadanie 4.** Wypisz bez powtórzeń wszystkie wartości kolumny `region`. Ile ich jest?

**Zadanie 5.** Wypisz bez powtórzeń wartości kolumny `oficjalny` z tabeli `jezyki`.

---

## Część II — Sortowanie i LIMIT

**Zadanie 6.** 5 krajów o **największej** populacji.

**Zadanie 7.** 5 krajów o **najmniejszej** powierzchni.

**Zadanie 8.** 10 największych miast świata (nazwa + populacja).

**Zadanie 9.** Wszystkie kraje posortowane po `kontynent` (A→Z), a wewnątrz kontynentu
alfabetycznie po nazwie.

---

## Część III — WHERE: jeden warunek

**Zadanie 10.** Wszystkie dane **Włoch** (`ITA`).

**Zadanie 11.** Wszystkie miasta **Węgier** (`HUN`).

**Zadanie 12.** Kraje leżące w **Ameryce Północnej**.

**Zadanie 13.** Kraje o populacji powyżej 100 000 000.

**Zadanie 14.** Miasta o populacji poniżej 50 000.

---

## Część IV — WHERE: warunki złożone

**Zadanie 15.** Miasta Włoch o populacji powyżej 200 000.

**Zadanie 16.** Kraje Azji o powierzchni poniżej 100 000 km².

**Zadanie 17.** Kraje leżące w Europie **lub** Ameryce Południowej, posortowane po populacji malejąco.

**Zadanie 18.** Kraje o powierzchni od 50 000 do 100 000 km² (użyj `BETWEEN`).

**Zadanie 19.** Kraje o kodach `NOR`, `SWE`, `FIN`, `DNK`, `ISL` — nazwa, populacja, długość życia.

**Zadanie 20.** Miasta, których nazwa zaczyna się na `San`.

**Zadanie 21.** Kraje, których nazwa kończy się na `ia` (np. Austria, Australia).

**Zadanie 22.** Miasta Polski, których nazwa zawiera `ow`.

---

## Część V — Sprawdź się

**Zadanie 23.** Jednym zapytaniem: **10 najludniejszych miast Chin** (`CHN`), z nagłówkami
`Miasto` i `Ludnosc`.

**Zadanie 24.** Jednym zapytaniem: kraje Europy o populacji od 5 do 15 milionów,
posortowane alfabetycznie.

---

## Część VI — `IS NULL`: brak wartości

Wróć do obserwacji z poprzednich zajęć: przy sortowaniu po `srednia_dlugosc_zycia`
na górze listy pojawiały się dziwne wiersze. Teraz się wyjaśni.

**Zadanie 25.** Wyświetl kraje, które **nie mają** wpisanej `srednia_dlugosc_zycia`.
> `WHERE srednia_dlugosc_zycia IS NULL`

Ile ich jest? Co to za państwa/terytoria?

**Zadanie 26.** Sprawdź, co zwróci `WHERE srednia_dlugosc_zycia = NULL`. Ile wierszy?
**Zapamiętaj:** `NULL` to „brak wartości" — nie jest równy niczemu, nawet samemu sobie.
Dlatego porównanie `= NULL` nigdy nie działa.

**Zadanie 27.** Wyświetl kraje, które **mają** wpisane `pkb`.
> `IS NOT NULL`

**Zadanie 28.** Wyświetl kraje bez wpisanego `rok_niepodleglosci`. Zgadnij najpierw, co to będzie
za grupa państw — potem sprawdź.

**Zadanie 29.** Kraje, które mają wpisaną średnią długość życia **i** wynosi ona **poniżej 50 lat**.
Posortuj rosnąco.

**Zadanie 30.** Kraje Europy, które nie mają wpisanego `pkb`.

---

## Część VII — Funkcje tekstowe

**Zadanie 31.** Wyświetl w jednym zapytaniu nazwy 10 krajów w czterech kolumnach:
oryginalnie, `UPPER`, `LOWER` i `LENGTH`. Co robi każda z tych funkcji?

**Zadanie 32.** Użyj `CONCAT`, żeby otrzymać kolumnę w formacie `POL — Poland`:
> `CONCAT(kod, ' — ', nazwa) AS Kraj`

**Zadanie 33.** Dla miast Polski zbuduj kolumnę w formacie `Warszawa (Mazowieckie)`.

**Zadanie 34.** Dla tabeli `jezyki` zbuduj kolumnę `POL: Polish` (kod kraju, dwukropek, język).

**Zadanie 35.** Wyświetl kraje, których nazwa ma **więcej niż 20 znaków**, posortowane od najdłuższej.
> `LENGTH` działa również w `WHERE` i w `ORDER BY`.

**Zadanie 36.** Wyświetl miasta Polski z nazwą zapisaną WIELKIMI LITERAMI, posortowane alfabetycznie.

---

## Część VIII — Arytmetyka na kolumnach

**Zadanie 37.** Dla krajów Europy wyświetl nazwę, powierzchnię, populację i dodatkową kolumnę
**gęstość zaludnienia** — `liczba_mieszkancow / powierzchnia` — zaokrągloną do liczby całkowitej.
> `ROUND(liczba_mieszkancow / powierzchnia, 0) AS Gestosc`

**Zadanie 38.** Powtórz zadanie 37, ale posortuj malejąco po gęstości i pokaż tylko 10 krajów.
Który kraj Europy jest najgęściej zaludniony?

**Zadanie 39.** Oblicz „PKB na mieszkańca" dla krajów `POL`, `DEU`, `NOR`, `UKR`:
> `ROUND(pkb * 1000000 / liczba_mieszkancow, 0) AS PkbNaMieszkanca`

Posortuj malejąco. (W bazie `pkb` jest w **milionach** dolarów — stąd mnożenie.)

**Zadanie 40.** Wyświetl kraje z PKB na mieszkańca **powyżej 25 000 $**, posortowane malejąco,
pierwsze 10.
> Pułapka 1: w `WHERE` trzeba powtórzyć **całe wyrażenie** — alias z `SELECT` tam nie działa,
> bo `WHERE` wykonuje się wcześniej.
> Pułapka 2: odfiltruj kraje bez wpisanego `pkb` (`IS NOT NULL`).

**Zadanie 41.** Sprawdź, o ile zmieniło się PKB: wyświetl nazwę, `pkb`, `pkb_poprzedni`
oraz różnicę `pkb - pkb_poprzedni` jako `Zmiana`. Posortuj malejąco po zmianie, pierwsze 15.

**Zadanie 42.** Wyświetl kraje, w których `pkb` **spadło** w porównaniu z `pkb_poprzedni`.

---

## Część IX — Stronicowanie (`OFFSET`)

**Zadanie 43.** Wyświetl miasta na pozycjach **11–20** według alfabetu.
> `ORDER BY nazwa LIMIT 10 OFFSET 10`

**Zadanie 44.** Wyświetl kraje na pozycjach **21–30** według populacji malejąco.

**Zadanie 45.** Wyświetl „trzecią stronę" listy krajów po 25 wierszy (czyli wiersze 51–75),
posortowaną alfabetycznie. Ile trzeba pominąć?

---

## Część X — Zadania podsumowujące (wszystko naraz)

**Zadanie 46.** Kraje Europy, które mają wpisane `pkb`, o populacji powyżej 10 000 000 —
wyświetl nazwę i PKB na mieszkańca, posortowane malejąco, pierwsze 10.

**Zadanie 47.** Miasta, których nazwa zaczyna się na `B`, leżące w Niemczech lub Francji,
o populacji powyżej 100 000 — posortowane malejąco po populacji.

**Zadanie 48.** Języki oficjalne (`oficjalny = 'T'`), którymi mówi od 40% do 60% mieszkańców kraju —
posortowane malejąco po `procent`.

**Zadanie 49.** Kraje, które uzyskały niepodległość po 1990 roku, posortowane od najnowszych.
Wyświetl nazwę, rok i kontynent.

**Zadanie 50.** Ułóż **własne** zapytanie, które wykorzystuje jednocześnie: `WHERE` z dwoma warunkami,
funkcję (`ROUND`, `UPPER` albo `CONCAT`), `ORDER BY` i `LIMIT`. Uruchom je
i wyjaśnij sąsiadowi, co robi.

---

## Pytania kontrolne

1. Dlaczego `WHERE kolumna = NULL` nigdy nic nie zwraca?
2. Czym różni się `LENGTH` od `ROUND`?
3. Dlaczego alias nadany w `SELECT` nie działa w `WHERE`?
4. Jak wyświetlić wiersze od 31. do 40.?
5. Wymień kolejność klauzul w zapytaniu `SELECT`.
6. Podaj dwa sposoby zapisania warunku „powierzchnia od 1000 do 5000".
