# Zestaw 3 — klucz odpowiedzi (wersja dla nauczyciela)

**Nie dawać uczniom.** Wszystkie wyniki sprawdzone na pliku `world_pl.sql`
(MariaDB 11, baza `world`: `kraje` 239, `miasta` 4079, `jezyki` 984).

W kolumnie „wynik" podane są liczby, które uczeń powinien zobaczyć — dobre do szybkiego
sprawdzania przy przechodzeniu między stanowiskami.

---

## Część I — Jedna liczba z całej tabeli

**Z1.**
```sql
SELECT COUNT(*) AS IleKrajow FROM kraje;
```
→ **239**

**Z2.** `SELECT COUNT(*) FROM miasta;` → **4079**, `SELECT COUNT(*) FROM jezyki;` → **984**

**Z3.**
```sql
SELECT COUNT(*) AS Wszystkie, COUNT(srednia_dlugosc_zycia) AS ZWartoscia FROM kraje;
```
→ **239 i 222**. Różnica 17 wierszy to kraje/terytoria z `NULL` (m.in. Antarctica, Holy See,
Pitcairn, French Southern territories). **Wniosek: funkcje agregujące pomijają `NULL`.**

**Z4.** `COUNT(rok_niepodleglosci)` → **192**, braki → **47**.
```sql
SELECT COUNT(*) - COUNT(rok_niepodleglosci) AS Braki FROM kraje;
SELECT COUNT(*) AS Braki FROM kraje WHERE rok_niepodleglosci IS NULL;
```
Oba sposoby dają **47**. Warto podkreślić: `WHERE rok_niepodleglosci = NULL` dałoby 0 wierszy.

**Z5.** `SELECT SUM(liczba_mieszkancow) FROM kraje;` → **6 078 749 450** (~6,08 mld — dane z 2000 r.)

**Z6.** `SELECT ROUND(AVG(srednia_dlugosc_zycia),2) AS SredniaZycia FROM kraje;` → **66.49**
(średnia z 222 wierszy, nie z 239!)

**Z7.** `SELECT MIN(powierzchnia), MAX(powierzchnia) FROM kraje;` → **0.40** (Holy See / Watykan)
i **17 075 400.00** (Rosja)

**Z8.** `SELECT MAX(liczba_mieszkancow) FROM miasta;` → **10 500 000**;
`SELECT nazwa, kod_kraju FROM miasta ORDER BY liczba_mieszkancow DESC LIMIT 1;` → **Mumbai (Bombay), IND**.
`MAX` zwraca samą liczbę — jedna liczba nie „pamięta", z którego wiersza pochodzi.
(Nazwę można dołożyć tylko przez `ORDER BY … LIMIT` albo podzapytanie — to materiał na później.)

**Z9.** `COUNT(DISTINCT kontynent)` → **7**, `COUNT(DISTINCT region)` → **25**

**Z10.** `SELECT COUNT(DISTINCT jezyk) FROM jezyki;` → **458** przy 984 wierszach —
ten sam język (np. English) występuje w wielu krajach, a tabela ma wiersz „język × kraj".

---

## Część II — Agregat z warunkiem `WHERE`

| Zad. | Zapytanie | Wynik |
|---|---|---|
| **Z11** | `SELECT COUNT(*) FROM miasta WHERE kod_kraju='POL';` | **44** |
| **Z12** | `SELECT COUNT(*) FROM kraje WHERE kontynent='Europa';` | **46** |
| **Z13** | `SELECT SUM(liczba_mieszkancow) AS Razem, ROUND(AVG(liczba_mieszkancow)) AS Srednio FROM miasta WHERE kod_kraju='POL';` | **11 687 431** i **265 623** |
| **Z14** | `SELECT ROUND(AVG(srednia_dlugosc_zycia),1) FROM kraje WHERE kontynent='Europa';` | **75.1** |
| **Z15** | `SELECT MAX(liczba_mieszkancow) FROM miasta WHERE kod_kraju='DEU';` | **3 386 667** → **Berlin** |
| **Z16** | `SELECT COUNT(*) FROM jezyki WHERE oficjalny='T';` | **238** (na 984 wpisy) |
| **Z17** | `SELECT COUNT(*) FROM kraje WHERE kontynent='Afryka' AND srednia_dlugosc_zycia<50;` | **25** |

---

## Część III — `GROUP BY`

**Z18.** 7 wierszy:

| kontynent | Kraje |
|---|---|
| Afryka | 58 |
| Azja | 51 |
| Europa | 46 |
| Ameryka Północna | 37 |
| Oceania | 28 |
| Ameryka Południowa | 14 |
| Antarktyda | 5 |

**Z19.** `SUM(liczba_mieszkancow)` per kontynent:
Azja **3 705 025 700** · Afryka **784 475 000** · Europa **730 074 600** ·
Ameryka Północna **482 993 000** · Ameryka Południowa **345 780 000** ·
Oceania **30 401 150** · Antarktyda **0**.
Dobra dyskusja: Antarktyda ma 5 „krajów" i 0 mieszkańców.

**Z20.** `ROUND(AVG(srednia_dlugosc_zycia),1)`:
Europa **75.1** · Ameryka Północna **73.0** · Ameryka Południowa **70.9** · Oceania **69.7** ·
Azja **67.4** · Afryka **52.6** · **Antarktyda `NULL`**.
`NULL` bierze się stąd, że **żaden** wiersz Antarktydy nie ma wpisanej średniej długości życia —
agregat nie miał z czego liczyć (to nie jest zero!).

**Z21.** Top 10 krajów po liczbie miast: CHN **363**, IND **341**, USA **274**, BRA **250**,
JPN **248**, RUS **189**, MEX **173**, PHL **136**, DEU **93**, IDN **85**.

**Z22.** 16 wierszy. Slaskie **14**, Kujawsko-Pomorskie **4**, Dolnoslaskie **4**,
Pomorskie **3**, Mazowieckie **3**, potem 5 województw po **2** (Lubuskie, Malopolskie,
Zachodnio-Pomorskie, Warminsko-Mazurskie, Wielkopolskie) i 6 po **1**.
Śląskie wygrywa, bo baza traktuje konurbację jako osobne miasta (Katowice, Sosnowiec, Gliwice,
Bytom, Zabrze, Częstochowa…). Warto to omówić: **dane opisują rzeczywistość umownie**.

**Z23.** Suma mieszkańców, top 5: Slaskie **2 536 449**, Mazowieckie **1 978 642**,
Dolnoslaskie **976 924**, Malopolskie **859 644**, Kujawsko-Pomorskie **818 820**.

**Z24.** Top 5 regionów: Caribbean **24**, Eastern Africa **20**, Middle East **18**,
Western Africa **17**, Southern Europe **15**.

**Z25.** Top 5 języków: English **60**, Arabic **33**, Spanish **28**, French **25**, German **19**.

**Z26.** `MAX(liczba_mieszkancow)` per kontynent: Azja **1 277 558 000** (Chiny),
Ameryka Północna **278 357 000** (USA), Ameryka Południowa **170 115 000** (Brazylia),
Europa **146 934 000** (Rosja), Afryka **111 506 000** (Nigeria),
Oceania **18 886 000** (Australia), Antarktyda **0**.

**Z27.** Top 5 ustrojów: Republic **123**, Constitutional Monarchy **29**, Federal Republic **14**,
Dependent Territory of the UK **12**, Monarchy **5**.

**Z28.**
```sql
SELECT oficjalny, COUNT(*) AS Wpisy, ROUND(AVG(procent),2) AS SredniProcent
FROM jezyki GROUP BY oficjalny;
```
→ `T`: **238** wpisów, średnio **51.61 %**; `F`: **746** wpisów, średnio **10.41 %**.
Interpretacja: językami oficjalnymi mówi zwykle duża część mieszkańców, nieoficjalne to
najczęściej języki mniejszości.

**Z29.** (pierwsze zadanie na **dwie** kolumny grupujące) 10 wierszy dla Europy; największe: Republic **25**, Constitutional Monarchy **9**,
Federal Republic **5**, dalej pojedyncze przypadki (Independent Church State, Federation,
Parliamentary Coprincipality, Constitutional Monarchy (Emirate)…).
Wniosek: grup jest **więcej**, bo liczy się każda kombinacja wartości.

**Z30.** ⭐ `MAX` per województwo, top 5: Mazowieckie **1 615 369** (Warszawa),
Lodzkie **800 110** (Łódź), Malopolskie **738 150** (Kraków), Dolnoslaskie **636 765** (Wrocław),
Wielkopolskie **576 899** (Poznań).
Zauważ: Śląskie **nie** jest w czołówce, mimo że miało najwięcej miast i największą sumę.

---

## Część IV — `HAVING`

**Z31.** 3 wiersze: Afryka **58**, Azja **51**, Europa **46**.
`WHERE COUNT(*) > 40` jest niemożliwe, bo `WHERE` wykonuje się **przed** `GROUP BY` —
w tym momencie grupy jeszcze nie istnieją, więc `COUNT(*)` nie ma wartości (błąd **#1111**).

**Z32.** 3 wiersze: Azja **3 705 025 700**, Afryka **784 475 000**, Europa **730 074 600**.
**Ameryka Północna ma 482 993 000** — brakuje jej ok. 17 mln do progu, więc wypada.
To celowa pułapka: trzeba policzyć, a nie zgadywać.

**Z33.** `HAVING COUNT(*) > 2` → Slaskie **14**, Kujawsko-Pomorskie **4**, Dolnoslaskie **4**,
Mazowieckie **3**, Pomorskie **3** (5 wierszy).

**Z34.** `HAVING COUNT(*) > 100` → CHN 363, IND 341, USA 274, BRA 250, JPN 248, RUS 189,
MEX 173, PHL 136 (**8 wierszy**; DEU z 93 miastami już nie przechodzi).

**Z35.** `HAVING COUNT(*) > 20` → English **60**, Arabic **33**, Spanish **28**, French **25**
(4 wiersze; German z 19 wypada).

**Z36.** `HAVING AVG(srednia_dlugosc_zycia) > 70` → Europa **75.1**, Ameryka Północna **73.0**,
Ameryka Południowa **70.9** (3 wiersze). Antarktyda z `NULL` **nie** przechodzi warunku.

**Z37.** `HAVING COUNT(*) >= 15` → Caribbean 24, Eastern Africa 20, Middle East 18,
Western Africa 17, Southern Europe 15 (5 wierszy).

**Z38.** `HAVING SUM(liczba_mieszkancow) > 1000000` → Slaskie **2 536 449**,
Mazowieckie **1 978 642** (tylko 2 wiersze).

**Z39.**
```sql
SELECT kontynent, COUNT(*) AS Kraje, ROUND(AVG(srednia_dlugosc_zycia),1) AS Zycie
FROM kraje
GROUP BY kontynent
HAVING COUNT(*) > 30 AND AVG(srednia_dlugosc_zycia) < 70;
```
→ Azja (**51**, **67.4**) i Afryka (**58**, **52.6**). Europa i Ameryka Północna wypadają
przez drugi warunek.

---

## Część V — Pełny schemat

**Z40.**
```sql
SELECT wojewodztwo,
       COUNT(*)                AS Miasta,
       SUM(liczba_mieszkancow) AS Ludnosc
FROM miasta
WHERE kod_kraju = 'POL'
GROUP BY wojewodztwo
HAVING COUNT(*) >= 2
ORDER BY Ludnosc DESC
LIMIT 5;
```
→ Slaskie (14 / 2 536 449), Mazowieckie (3 / 1 978 642), Dolnoslaskie (4 / 976 924),
Malopolskie (2 / 859 644), Kujawsko-Pomorskie (4 / 818 820).
Bez `LIMIT` warunek `HAVING` spełnia **10** województw.
Kolejność wykonywania: `FROM` → `WHERE` → `GROUP BY` → agregaty → `HAVING` → `SELECT` →
`ORDER BY` → `LIMIT`.

**Z41.**
```sql
SELECT region, COUNT(*) AS Kraje, ROUND(AVG(pkb)) AS SredniePKB
FROM kraje
WHERE kontynent = 'Europa'
GROUP BY region
HAVING COUNT(*) >= 5
ORDER BY SredniePKB DESC;
```
→ Western Europe (9 / **519 252**), Southern Europe (15 / **134 153**),
Nordic Countries (7 / **96 665**), Eastern Europe (10 / **65 998**).
`pkb` jest w milionach dolarów — warto to powiedzieć, żeby liczby miały sens.
Baltic Countries (3 kraje) i British Islands (2 kraje) wypadają przez `HAVING`.

**Z42.**
```sql
SELECT kod_kraju, COUNT(*) AS Oficjalne
FROM jezyki
WHERE oficjalny = 'T'
GROUP BY kod_kraju
HAVING COUNT(*) > 1
ORDER BY Oficjalne DESC;
```
→ na czele CHE **4** i ZAF **4**, potem po **3**: BEL, BOL, LUX, PER, SGP, VUT…
Świetny moment, żeby pokazać, że Szwajcaria z zadania o `WHERE` z Zestawu 1 wraca tu jako grupa.

**Z43.** ⭐
```sql
SELECT kod_kraju, COUNT(*) AS Jezyki
FROM jezyki
GROUP BY kod_kraju
HAVING COUNT(*) >= 5
ORDER BY Jezyki DESC;
```
→ **91 wierszy**; czoło: CAN, CHN, IND, RUS, USA po **12**, TZA **11**.
Liczbę krajów uczeń odczytuje z licznika wierszy nad wynikiem (nie trzeba podzapytania).

**Z44.** ⭐
```sql
SELECT kod_kraju, COUNT(*) AS DuzeMiasta
FROM miasta
WHERE liczba_mieszkancow > 5000000
GROUP BY kod_kraju
HAVING COUNT(*) >= 2
ORDER BY DuzeMiasta DESC;
```
→ CHN **4**, BRA **2**, IND **2**, PAK **2**. Bez `HAVING` wierszy jest 18 (po jednym mieście).

---

## Część VI — Popraw błąd

**Z45.** Komunikat: **`#1111 - Invalid use of group function`**.
Przyczyna: `WHERE` działa przed powstaniem grup, więc nie zna `COUNT(*)`.
Poprawka — warunek przenosimy do `HAVING`:
```sql
SELECT wojewodztwo, COUNT(*) AS Miasta
FROM miasta
GROUP BY wojewodztwo
HAVING COUNT(*) > 2;
```
(Jeśli chcemy tylko Polskę, `WHERE kod_kraju='POL'` zostaje — bo to warunek na **wiersz**.)

**Z46.** `nazwa` nie jest ani w `GROUP BY`, ani w funkcji agregującej.
MySQL/MariaDB w domyślnej konfiguracji szkolnej pokaże **losową** nazwę z grupy
(przy `ONLY_FULL_GROUP_BY` — błąd `#1055`). Grupa ma np. 51 krajów, a nazwa jest jedna — bezsens.
Poprawki:
```sql
-- a) usuwamy kolumnę
SELECT kontynent, COUNT(*) AS Kraje FROM kraje GROUP BY kontynent;

-- b) wciskamy ją w agregat (tu: nazwa ostatnia alfabetycznie)
SELECT kontynent, COUNT(*) AS Kraje, MAX(nazwa) AS OstatniaAlfabetycznie
FROM kraje GROUP BY kontynent;
```

**Z47.** Kluczowe rozróżnienie zestawu:

| Zapytanie | Co liczy | Wynik |
|---|---|---|
| `WHERE liczba_mieszkancow > 50000000` + `GROUP BY` | liczy **tylko duże kraje** w każdej grupie | 5 wierszy: Azja 11, Europa 6, Afryka 4, Ameryka Północna 2, Ameryka Południowa 1 |
| `GROUP BY` + `HAVING SUM(...) > 50000000` | liczy **wszystkie** kraje, ale odrzuca **słabo zaludnione kontynenty** | 5 wierszy: Azja 51, Afryka 58, Europa 46, Ameryka Północna 37, Ameryka Południowa 14 |

Ta sama liczba wierszy, zupełnie inne liczby: `WHERE` odsiał **kraje**, `HAVING` odsiał
**kontynenty** (wypadły Oceania ~30,4 mln i Antarktyda 0).

---

## Część VII — Grupowanie po dwóch kolumnach

**Z48.**
```sql
SELECT kod_kraju, oficjalny, COUNT(*) AS Wpisy
FROM jezyki
WHERE kod_kraju IN ('POL','CHE','ZAF')
GROUP BY kod_kraju, oficjalny
ORDER BY kod_kraju, oficjalny;
```
→ **5 wierszy**:

| kod_kraju | oficjalny | Wpisy |
|---|---|---|
| CHE | T | 4 |
| POL | T | 1 |
| POL | F | 3 |
| ZAF | T | 4 |
| ZAF | F | 7 |

Najwięcej języków oficjalnych: **CHE i ZAF po 4**. Szwajcaria ma **jeden** wiersz, bo
w bazie nie ma dla niej ani jednego wpisu z `oficjalny = 'F'` — grupa powstaje tylko dla
kombinacji obecnych w danych (to samo, co brakujące `2 + N` w stepperze na ściądze).

**Z49.** a) `GROUP BY kontynent` → **7** grup; b) `GROUP BY ustroj` → **35** grup;
c) `GROUP BY kontynent, ustroj` → **70** grup.
Wniosek: druga kolumna **rozdrabnia** grupy — wierszy jest więcej niż w każdym pojedynczym
grupowaniu, ale **mniej** niż 7 × 35 = 245, bo liczą się tylko kombinacje, które faktycznie
występują (np. nie ma monarchii w Antarktydzie).

**Z50.**
```sql
SELECT kontynent, ustroj, COUNT(*) AS Ile
FROM kraje
GROUP BY kontynent, ustroj
HAVING COUNT(*) >= 5
ORDER BY Ile DESC;
```
→ **11 wierszy**: Afryka/Republic **46**, Azja/Republic **27**, Europa/Republic **25**,
Ameryka Północna/Republic **10**, Europa/Constitutional Monarchy **9**,
Ameryka Południowa/Republic **9**, Ameryka Północna/Constitutional Monarchy **9**,
Ameryka Północna/Dependent Territory of the UK **6**, Oceania/Republic **6**,
Europa/Federal Republic **5**, Azja/Constitutional Monarchy **5**.
Warto pokazać: z 70 grup po `HAVING` zostaje 11 — większość kombinacji ma 1–2 kraje.

**Z51.** `WHERE kontynent='Azja' GROUP BY kontynent, region` → **4 wiersze**:
Middle East **18**, Southern and Central Asia **14**, Southeast Asia **11**, Eastern Asia **8**.
(Kolumna `kontynent` jest tu zawsze taka sama — dobry moment, by zapytać, po co ją w ogóle
trzymać w `GROUP BY`. Odpowiedź: przy filtrze na jeden kontynent nie jest konieczna.)

**Z52.** `GROUP BY kontynent, region` → **25** grup, czyli dokładnie tyle, ile
`COUNT(DISTINCT region)` z zadania 9. Powód: każdy `region` należy **tylko do jednego**
kontynentu, więc druga kolumna nie wnosi nowego podziału. To ważny kontrprzykład do Z49:
dodanie kolumny **nie zawsze** zwiększa liczbę grup.

**Z53.**
```sql
SELECT kod_kraju, wojewodztwo, COUNT(*) AS Miasta
FROM miasta
GROUP BY kod_kraju, wojewodztwo
HAVING COUNT(*) >= 20
ORDER BY Miasta DESC;
```
→ **33 wiersze**; pierwsze trzy: GBR/England **71**, BRA/São Paulo **69**, USA/California **68**.
Bez kolumny `kod_kraju` grupy by się **zlały** (np. `Punjab` występuje w Indiach i w Pakistanie,
`Northern` w kilku krajach). Bez `HAVING` grup jest **1412**.

**Z54.** ⭐ `WHERE kod_kraju IN ('POL','CZE') … HAVING COUNT(*) >= 2` → **13 wierszy**:
POL/Slaskie **14**, POL/Kujawsko-Pomorskie **4**, POL/Dolnoslaskie **4**, POL/Mazowieckie **3**,
POL/Pomorskie **3**, dalej po **2**: POL/Zachodnio-Pomorskie, POL/Wielkopolskie, POL/Malopolskie,
POL/Warminsko-Mazurskie, POL/Lubuskie oraz CZE/Severní Cechy, CZE/Severní Morava,
CZE/Východní Cechy.

---

## Pytania kontrolne — odpowiedzi

1. Jeden wiersz z jedną liczbą policzoną z **całej** tabeli (albo z tego, co zostawił `WHERE`).
2. `COUNT(*)` liczy wiersze, `COUNT(kolumna)` — tylko wiersze, gdzie ta kolumna nie jest `NULL`.
3. Pomijają je (nie traktują jak 0). Jeśli w grupie są same `NULL`-e, wynik agregatu to `NULL`.
4. Dzieli wiersze na grupy o tej samej wartości wskazanej kolumny; zwraca **tyle wierszy, ile jest grup**.
5. Tylko kolumny wymienione w `GROUP BY` oraz kolumny schowane w funkcji agregującej.
6. `WHERE` filtruje pojedyncze wiersze przed grupowaniem (`WHERE kod_kraju='POL'`),
   `HAVING` filtruje gotowe grupy po agregacji (`HAVING COUNT(*) > 2`).
7. Bo `WHERE` wykonuje się przed `GROUP BY` — agregat jeszcze nie istnieje (błąd #1111).
8. `FROM` → `WHERE` → `GROUP BY` (+ agregaty) → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`.
9. Po dowolnej liczbie kolumn (wypisujemy je po przecinku). Grupą staje się **kombinacja
   wartości**, więc wierszy jest zwykle więcej, a grupy są mniejsze — chyba że druga kolumna
   jest jednoznacznie zależna od pierwszej (Z52: region ↔ kontynent).
10. Bo liczba w wyniku dotyczy **pary** wartości. Gdyby pokazać samo `wojewodztwo`,
    nie dałoby się odróżnić Punjabu w Indiach od Punjabu w Pakistanie.

---

## Wskazówki organizacyjne

- Części I–II to rozgrzewka na ok. 10 min; jeśli klasa idzie szybko, wystarczy Z3, Z8 i Z10.
- **Serce zestawu** to Z22–Z23 (Śląskie kontra Mazowieckie), Z32 (Ameryka Północna pod progiem),
  Z45 i Z47 — z nich wynikają wszystkie wnioski o różnicy `WHERE`/`HAVING`.
- Uczniom, którzy skończą wcześniej: Z30, Z43, Z44 oraz polecenie „wymyśl własne zapytanie
  z `HAVING`, które zwróci dokładnie 2 wiersze".
- Przed zajęciami warto pokazać animację z końca ściągi „GROUP BY + HAVING" (kroki 3–5 i bonus 9) —
  zadania Z45 i Z47 są dokładnie o tym.
- **Część VII (Z48–Z54)** można spokojnie przerobić zaraz po Części III, jeszcze przed `HAVING` —
  numeracja jest ciągła tylko dla porządku w wydruku. Do tej części pasuje **drugi stepper**
  ze ściągi („Jedna kolumna czy dwie?"): krok 2 pokazuje rozpad 3 grup na 5, a krok 3 tłumaczy
  brakującą kombinację — czyli dokładnie sytuację ze Szwajcarii w Z48.
- Na egzaminie INF.03 grupowanie po kilku kolumnach pojawia się rzadziej niż po jednej, ale
  jest w zakresie kwalifikacji (typowy przykład: `GROUP BY klasa, plec`, a przy `JOIN`
  `GROUP BY nazwisko, imie`). Warto, żeby uczniowie choć raz to zobaczyli.
