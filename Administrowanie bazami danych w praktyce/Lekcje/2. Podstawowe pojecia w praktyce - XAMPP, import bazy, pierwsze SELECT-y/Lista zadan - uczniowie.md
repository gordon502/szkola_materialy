# Lekcja 2 — Lista zadań

## Część I — Przygotowanie środowiska

**Zadanie 1.** Uruchom XAMPP Control Panel. Wystartuj moduły **Apache** i **MySQL**. Otwórz w przeglądarce `http://localhost/phpmyadmin`.

**Zadanie 2.** Zaimportuj do phpMyAdmin plik `world_pl.sql` (zakładka **Import**).

**Zadanie 3.** Sprawdź, jakie tabele zawiera baza `world`. Ile ich jest?

## Część II — Pojęcia bazodanowe

**Zadanie 4.** Otwórz tabelę `miasta`. Wskaż na ekranie:
a) jedną **kolumnę** (atrybut),
b) jeden **rekord** (wiersz),
c) **klucz główny** tabeli.
Zapisz w zeszycie definicje: tabela, rekord, kolumna, klucz główny.

**Zadanie 5.** Zbadaj tabelę `kraje`. Jaki atrybut jest tu kluczem głównym? Dlaczego nie mogła być nim `nazwa`?

**Zadanie 6.** W zakładce **Struktura** tabeli `kraje` przejrzyj typy danych kolumn. Zapisz 3 przykłady typów, które widzisz. Co oznacza liczba w nawiasie przy `char`?

## Część III — Pierwsze zapytania SELECT

**Zadanie 7.** Wyświetl wszystkie dane tabeli miast.

**Zadanie 8.** Wyświetl tylko nazwę i populację miast.

**Zadanie 9.** Nadaj kolumnom polskie nagłówki używając aliasu `AS`: `Miasto`, `LiczbaMieszkancow`.

**Zadanie 10.** Wyświetl tylko 10 pierwszych miast.

**Zadanie 11.** Wyświetl wszystkie kraje posortowane alfabetycznie, a potem od końca.

**Zadanie 12.** Wyświetl 5 miast o największej populacji.

**Zadanie 13.** Wyświetl listę kontynentów bez powtórzeń.

## Część IV — Zadania dodatkowe

**Zadanie 14.** Wyświetl wszystkie miasta Polski. Ile ich jest?

**Zadanie 15.** Wyświetl miasta Polski, w których mieszka więcej niż 300 000 osób. Posortuj malejąco po populacji.

**Zadanie 16.** Wyświetl dane Polski z tabeli `kraje`: nazwę, powierzchnię, populację i średnią długość życia.

**Zadanie 17.** Znajdź w bazie Warszawę. Zapisz jej `id` i `wojewodztwo`.
