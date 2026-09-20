# Zestaw 3 — Agregacje, `GROUP BY` i `HAVING`

**Baza:** `world` (plik `world_pl.sql`) | **Narzędzie:** XAMPP → phpMyAdmin → zakładka **SQL**
**Materiał pomocniczy:** ściąga „GROUP BY + HAVING" (na końcu ściągi jest animacja krok po kroku)
**Zakres:** `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `COUNT(DISTINCT …)`, `GROUP BY`, `HAVING`,
pełny schemat zapytania. **Bez `JOIN`** — cały czas pracujemy na jednej tabeli.

**Tabele w bazie:**

| Tabela | Co zawiera | Kolumny przydatne w tym zestawie |
|---|---|---|
| `kraje` | 239 państw | `kod`, `nazwa`, `kontynent`, `region`, `powierzchnia`, `rok_niepodleglosci`, `liczba_mieszkancow`, `srednia_dlugosc_zycia`, `pkb`, `ustroj` |
| `miasta` | 4079 miast | `id`, `nazwa`, `kod_kraju`, `wojewodztwo`, `liczba_mieszkancow` |
| `jezyki` | 984 wpisy | `kod_kraju`, `jezyk`, `oficjalny` (`T`/`F`), `procent` |

**Zasady:**
- każde zapytanie kończymy średnikiem `;`, tekst w apostrofach (`'POL'`), ułamki z **kropką**,
- w każdym zadaniu z `GROUP BY` nadawaj **aliasy** (`AS Ilu`, `AS Ludnosc`) — inaczej nagłówki są nieczytelne,
- po każdym zapytaniu sprawdź **ile wierszy** zwróciło (liczba jest nad wynikiem) i zapisz ją w zeszycie,
- zadania z gwiazdką ⭐ są dla szybszych.

---

## Część I — Jedna liczba z całej tabeli

**Zadanie 1.** Ile jest krajów w tabeli `kraje`?
> Potrzebne: `COUNT(*)`. Nadaj wynikowi nagłówek `IleKrajow`.

**Zadanie 2.** Ile jest miast w tabeli `miasta`, a ile wpisów w tabeli `jezyki`?
(dwa osobne zapytania)

**Zadanie 3.** Wykonaj jedno zapytanie na tabeli `kraje`, które pokaże obok siebie
`COUNT(*)` oraz `COUNT(srednia_dlugosc_zycia)`.
**Dlaczego liczby są różne?** Zapisz wniosek jednym zdaniem.

**Zadanie 4.** Ile krajów ma wpisany `rok_niepodleglosci`, a ile nie ma?
Liczbę braków policz **dwoma sposobami**: przez `COUNT(*) - COUNT(rok_niepodleglosci)`
oraz przez `COUNT(*)` z warunkiem `WHERE rok_niepodleglosci IS NULL`.

**Zadanie 5.** Ilu jest łącznie mieszkańców wszystkich krajów świata?
> Potrzebne: `SUM(liczba_mieszkancow)`

**Zadanie 6.** Jaka jest średnia długość życia na świecie? Zaokrąglij do **2 miejsc** po kropce
i podpisz kolumnę `SredniaZycia`.
> Potrzebne: `ROUND(AVG(...), 2)`

**Zadanie 7.** Podaj powierzchnię najmniejszego i największego kraju — w jednym zapytaniu.
> Potrzebne: `MIN` i `MAX`

**Zadanie 8.** Jaka jest największa liczba mieszkańców miasta w bazie (`MAX`)?
Następnie **drugim zapytaniem** (`ORDER BY … DESC LIMIT 1`) sprawdź, **które to miasto**.
Dlaczego pierwsze zapytanie nie mogło pokazać nazwy?

**Zadanie 9.** Ile jest **różnych** kontynentów, a ile różnych regionów w tabeli `kraje`?
> Potrzebne: `COUNT(DISTINCT kolumna)`

**Zadanie 10.** Ile jest **różnych** języków w tabeli `jezyki`? Porównaj tę liczbę
z wynikiem `COUNT(*)` z zadania 2 i wyjaśnij różnicę.

---

## Część II — Agregat z warunkiem `WHERE`

**Zadanie 11.** Ile miast Polski (`kod_kraju = 'POL'`) jest w bazie?

**Zadanie 12.** Ile krajów leży w Europie?

**Zadanie 13.** Dla polskich miast podaj w jednym zapytaniu: **sumę** mieszkańców
i **średnią** liczbę mieszkańców (średnią zaokrąglij do liczby całkowitej).
> Potrzebne: `SUM`, `ROUND(AVG(...))` oraz `WHERE`

**Zadanie 14.** Jaka jest średnia długość życia w Europie? Zaokrąglij do 1 miejsca.

**Zadanie 15.** Ile mieszkańców ma największe miasto Niemiec (`DEU`)?
Sprawdź drugim zapytaniem, jak się nazywa.

**Zadanie 16.** Ile wpisów w tabeli `jezyki` dotyczy języków **oficjalnych** (`oficjalny = 'T'`)?

**Zadanie 17.** Ile krajów Afryki ma średnią długość życia **poniżej 50 lat**?
> Dwa warunki naraz — `AND`.

---

## Część III — `GROUP BY`, czyli podsumowanie w grupach

**Zadanie 18.** Ile krajów leży na każdym kontynencie? Posortuj malejąco.
> Wzór do zapamiętania:
> ```sql
> SELECT kontynent, COUNT(*) AS Kraje
> FROM kraje
> GROUP BY kontynent
> ORDER BY Kraje DESC;
> ```

**Zadanie 19.** Ilu mieszkańców ma każdy kontynent (`SUM`)? Posortuj od najludniejszego.

**Zadanie 20.** Jaka jest średnia długość życia na każdym kontynencie (`ROUND` do 1 miejsca)?
Jeden kontynent ma w wyniku `NULL` — który i dlaczego?

**Zadanie 21.** Ile miast z bazy należy do poszczególnych krajów (`kod_kraju`)?
Pokaż **10 krajów z największą liczbą miast**.

**Zadanie 22.** Ile polskich miast przypada na każde województwo?
Posortuj malejąco. Które województwo wygrywa i czy to Cię dziwi?

**Zadanie 23.** Dla każdego województwa policz **sumę** mieszkańców polskich miast.
Pokaż 5 najludniejszych województw.

**Zadanie 24.** Ile krajów leży w poszczególnych regionach (`region`)? Pokaż 5 największych regionów.

**Zadanie 25.** W tabeli `jezyki` policz, **w ilu krajach** występuje każdy język.
Pokaż 5 najpopularniejszych języków.

**Zadanie 26.** Dla każdego kontynentu podaj liczbę mieszkańców **najludniejszego** kraju
(`MAX(liczba_mieszkancow)`). Posortuj malejąco.

**Zadanie 27.** Ile krajów ma poszczególne ustroje (`ustroj`)? Pokaż 5 najczęstszych.

**Zadanie 28.** W tabeli `jezyki` pogrupuj wiersze po kolumnie `oficjalny` i podaj dla każdej grupy:
liczbę wpisów oraz średni `procent` (zaokrąglony do 2 miejsc).
Co mówią te dwie liczby?

**Zadanie 29.** Grupowanie po **dwóch** kolumnach: dla krajów Europy pokaż
`kontynent`, `ustroj` i liczbę krajów. Posortuj malejąco.
> `GROUP BY kontynent, ustroj` — pamiętaj, że **obie** kolumny muszą być też w `SELECT`.
> Więcej zadań na grupowanie po dwóch kolumnach znajdziesz w **Części VII**.

**Zadanie 30.** ⭐ Dla każdego województwa Polski podaj liczbę mieszkańców **największego** miasta
i posortuj malejąco. Pokaż 5 pierwszych wierszy.

---

## Część IV — `HAVING`, czyli filtr na grupy

**Zadanie 31.** Które kontynenty mają **więcej niż 40** krajów?
> ```sql
> SELECT kontynent, COUNT(*) AS Kraje
> FROM kraje
> GROUP BY kontynent
> HAVING COUNT(*) > 40
> ORDER BY Kraje DESC;
> ```
> **Pytanie:** dlaczego nie można tu napisać `WHERE COUNT(*) > 40`?

**Zadanie 32.** Które kontynenty mają łączną populację **powyżej 500 milionów**?
Zanim wykonasz zapytanie — obstaw, ile ich będzie. Który kontynent jest najbliżej granicy?

**Zadanie 33.** Które województwa mają w bazie **więcej niż 2** polskie miasta?

**Zadanie 34.** Które kraje mają w bazie **więcej niż 100** miast?

**Zadanie 35.** Które języki występują w **więcej niż 20** krajach?

**Zadanie 36.** Na których kontynentach średnia długość życia przekracza **70 lat**?

**Zadanie 37.** Które regiony mają **co najmniej 15** krajów?

**Zadanie 38.** Które województwa Polski mają łączną populację miast **powyżej 1 000 000**?

**Zadanie 39.** Dwa warunki w `HAVING` naraz: kontynenty, które mają **więcej niż 30 krajów**
**i jednocześnie** średnią długość życia **poniżej 70 lat**.
W wyniku pokaż kontynent, liczbę krajów i średnią długość życia.

---

## Część V — Pełny schemat zapytania

**Zadanie 40.** Złóż wszystko w jedno zapytanie na tabeli `miasta`:
- tylko polskie miasta (`WHERE`),
- grupuj po województwie (`GROUP BY`),
- pokaż liczbę miast i sumę mieszkańców (`COUNT`, `SUM`),
- zostaw tylko województwa z **co najmniej 2** miastami (`HAVING`),
- posortuj malejąco po sumie mieszkańców (`ORDER BY`),
- pokaż **5 pierwszych** wierszy (`LIMIT`).

Następnie **opisz w zeszycie**, w jakiej kolejności serwer wykonał te klauzule.

**Zadanie 41.** Dla krajów Europy pokaż `region`, liczbę krajów w regionie
oraz średnie `pkb` zaokrąglone do liczby całkowitej.
Zostaw tylko regiony mające **co najmniej 5** krajów, posortuj malejąco po średnim `pkb`.

**Zadanie 42.** Które kraje mają **więcej niż jeden język oficjalny**?
Pokaż `kod_kraju` i liczbę języków oficjalnych, posortuj malejąco.
> Uwaga na kolejność: najpierw `WHERE oficjalny = 'T'`, potem `GROUP BY`, na końcu `HAVING`.

**Zadanie 43.** ⭐ Wypisz kody krajów, które mają w bazie **co najmniej 5** języków
(bez względu na to, czy oficjalne). Posortuj malejąco po liczbie języków.
Ile jest takich krajów?

**Zadanie 44.** ⭐ Które kraje mają **co najmniej 2** miasta powyżej 5 000 000 mieszkańców?
> Najpierw odfiltruj duże miasta (`WHERE`), potem policz je w grupach (`GROUP BY` + `HAVING`).

---

## Część VI — Popraw błąd

**Zadanie 45.** To zapytanie **nie działa**. Uruchom je, przeczytaj komunikat błędu,
a potem popraw tak, żeby pokazywało województwa mające więcej niż 2 miasta.
```sql
SELECT wojewodztwo, COUNT(*) AS Miasta
FROM miasta
WHERE COUNT(*) > 2
GROUP BY wojewodztwo;
```
Zapisz: jaki numer błędu pokazał MySQL i dlaczego tak się stało?

**Zadanie 46.** To zapytanie wykonuje się, ale wynik jest **bezsensowny**.
Wyjaśnij, co jest nie tak, i napraw je na dwa sposoby:
a) usuwając niewłaściwą kolumnę, b) zamieniając ją na funkcję agregującą.
```sql
SELECT nazwa, kontynent, COUNT(*) AS Kraje
FROM kraje
GROUP BY kontynent;
```

**Zadanie 47.** Poniższe dwa zapytania różnią się jedną literą w nazwie klauzuli.
Wykonaj oba i wyjaśnij, **czym różnią się wyniki** (ile wierszy, co znaczą liczby).
```sql
SELECT kontynent, COUNT(*) AS Kraje FROM kraje
WHERE liczba_mieszkancow > 50000000
GROUP BY kontynent;
```
```sql
SELECT kontynent, COUNT(*) AS Kraje FROM kraje
GROUP BY kontynent
HAVING SUM(liczba_mieszkancow) > 50000000;
```

---

## Część VII — Grupowanie po **dwóch** kolumnach

W `GROUP BY` można wypisać kilka kolumn po przecinku. Grupa to wtedy **kombinacja wartości**,
więc grup jest **więcej**, a każda jest **mniejsza**. Wszystkie kolumny grupujące powinny
znaleźć się także w `SELECT` — inaczej nie wiadomo, czego dotyczy policzona liczba.
(Animacja: stepper porównawczy na samym końcu ściągi.)

**Zadanie 48.** Dla krajów `POL`, `CHE` i `ZAF` pokaż `kod_kraju`, `oficjalny`
i liczbę wpisów językowych. Posortuj po kodzie kraju.
> ```sql
> SELECT kod_kraju, oficjalny, COUNT(*) AS Wpisy
> FROM jezyki
> WHERE kod_kraju IN ('POL','CHE','ZAF')
> GROUP BY kod_kraju, oficjalny
> ORDER BY kod_kraju, oficjalny;
> ```
> **Pytania:** który z tych krajów ma najwięcej języków **oficjalnych**?
> Dlaczego jeden kraj zajmuje w wyniku tylko **jeden** wiersz, a nie dwa?

**Zadanie 49.** Policz, ile jest grup, gdy grupujesz tabelę `kraje`:
a) tylko po `kontynent`, b) tylko po `ustroj`, c) po `kontynent, ustroj`.
Liczbę grup odczytaj z licznika wierszy nad wynikiem. Ułóż te trzy liczby od najmniejszej
i zapisz wniosek o tym, jak druga kolumna wpływa na liczbę wierszy.

**Zadanie 50.** Pokaż wszystkie pary `kontynent` + `ustroj`, które występują w **co najmniej
5 krajach**. Posortuj malejąco po liczbie krajów.
> `GROUP BY` po dwóch kolumnach + `HAVING` — filtr grup działa dokładnie tak samo.

**Zadanie 51.** Dla krajów Azji pokaż `kontynent`, `region` i liczbę krajów w każdym regionie.
Posortuj malejąco.

**Zadanie 52.** Pogrupuj całą tabelę `kraje` po `kontynent, region` i policz kraje.
Ile wyszło grup? Porównaj tę liczbę z wynikiem `COUNT(DISTINCT region)` z zadania 9
i wyjaśnij, **dlaczego są równe**.

**Zadanie 53.** Na całej tabeli `miasta` znajdź pary `kod_kraju` + `wojewodztwo`,
w których jest **co najmniej 20 miast**. Posortuj malejąco i podaj pierwsze trzy wiersze.
> Tu grupowanie po dwóch kolumnach jest **konieczne**: nazwy regionów mogą się powtarzać
> w różnych krajach, więc samo `GROUP BY wojewodztwo` zlepiłoby je w jedną grupę.

**Zadanie 54.** ⭐ Dla Polski i Czech (`POL`, `CZE`) pokaż pary kraj + województwo/region,
które mają **co najmniej 2** miasta. Posortuj malejąco po liczbie miast.

---

## Pytania kontrolne

1. Co zwraca funkcja agregująca, gdy w zapytaniu **nie ma** `GROUP BY`?
2. Czym różni się `COUNT(*)` od `COUNT(kolumna)`?
3. Jak funkcje agregujące traktują `NULL`?
4. Co dokładnie robi `GROUP BY` — na co dzieli tabelę i ile wierszy zwraca?
5. Jakie kolumny wolno wpisać w `SELECT`, gdy używamy `GROUP BY`?
6. Czym różni się `WHERE` od `HAVING`? Podaj po jednym przykładzie.
7. Dlaczego `WHERE COUNT(*) > 5` kończy się błędem, a `HAVING COUNT(*) > 5` działa?
8. W jakiej kolejności serwer wykonuje: `SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`?
9. Po ilu kolumnach można grupować? Co się dzieje z liczbą wierszy w wyniku, gdy dopiszemy
   drugą kolumnę do `GROUP BY`?
10. Dlaczego przy `GROUP BY kod_kraju, wojewodztwo` obie kolumny powinny być w `SELECT`?
