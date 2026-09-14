# Lekcja 4 — Lista zadań

## Część I — Typy danych

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

**Zadanie 2.** Odpowiedz:
a) Czym różni się `int` od `decimal(10,2)`? Dlaczego `liczba_mieszkancow` może być `int`, a `powierzchnia` już nie?
b) Co oznacza `NULL` w kolumnie `srednia_dlugosc_zycia`? Które kraje tak mają?
c) Po co jest `enum` zamiast `char`? Jakie wartości dopuszcza kolumna `kontynent`?
d) Dlaczego w `miasta` kolumna `wojewodztwo` jest typu `char(20)`, a `liczba_mieszkancow` typu `int`?

## Część II — Funkcje agregujące

**Zadanie 3.** Ile jest miast na świecie? A ile krajów? A ile wpisów języków?

**Zadanie 4.** Porównaj wyniki `COUNT(*)` i `COUNT(srednia_dlugosc_zycia)` dla tabeli `kraje`. Dlaczego wyniki są różne?

**Zadanie 5.** W jednym zapytaniu podaj maksymalną, minimalną, średnią i sumaryczną liczbę mieszkańców krajów. Który kraj ma maksimum?

**Zadanie 6.** Ile różnych województw występuje w tabeli miast dla Polski?

## Część III — GROUP BY

**Zadanie 7.** Wyświetl liczbę miast per województwo (Polska), posortowaną malejąco. Które województwo ma najwięcej miast w bazie?

**Zadanie 8.** Wyświetl sumę i średnią liczbę mieszkańców per województwo (Polska). Posortuj po sumie malejąco. Które województwo jest najludniejsze?

**Zadanie 9.** Wyświetl liczbę krajów per kontynent.

**Zadanie 10.** Wyświetl liczbę mieszkańców per kontynent. Który kontynent jest najludniejszy, a który najmniej?

**Zadanie 11.** Policz, ile krajów ma wpisany każdy język. Wyświetl 5 najczęstszych.

**Zadanie 12.** Pogrupuj miasta Polski po `wojewodztwo` i wyświetl dla każdej grupy największą liczbę mieszkańców.

## Część IV — HAVING

**Zadanie 13.** Wyświetl kontynenty z więcej niż 40 krajami. Dlaczego nie można było napisać `WHERE`?

**Zadanie 14.** Wyświetl województwa Polski, które w bazie mają więcej niż 2 miasta, posortowane malejąco.

**Zadanie 15.** Wyświetl kontynenty, których łączna populacja przekracza 500 mln.

**Zadanie 16.** Napisz zapytanie łączące `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY` i `LIMIT`: 5 województw Polski z co najmniej 2 miastami, z liczbą miast i łączną ludnością, posortowane po ludności malejąco.
