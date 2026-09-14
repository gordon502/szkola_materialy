# Bazy danych do zajęć

## `world_pl.sql` — GŁÓWNA baza na zajęcia (polskie nazwy w stylu egzaminu INF.03)

Oficjalna przykładowa baza MySQL „world" po modyfikacji: **nazwy tabel i kolumn po polsku**
(małe litery, bez znaków diakrytycznych, snake_case — jak na egzaminie INF.03), wartości
kontynentów przetłumaczone. Baza nazywa się `world`. Zweryfikowana na MariaDB 11 (import bez błędów).

**Źródło danych:** https://dev.mysql.com/doc/world-setup/en/ (swobodne użycie edukacyjne).

### Struktura

| Tabela | Opis | Klucz główny |
|---|---|---|
| `kraje` | 239 państw/terytoriów: `kod`, `nazwa`, `kontynent` (enum, PL wartości), `region`, `powierzchnia`, `rok_niepodleglosci`, `liczba_mieszkancow`, `srednia_dlugosc_zycia`, `pkb` (w mln $), `pkb_poprzedni`, `nazwa_lokalna`, `ustroj`, `glowa_panstwa`, `stolica` (→ `miasta.id`), `kod2` | `kod` (char 3) |
| `miasta` | 4079 miast: `id`, `nazwa`, `kod_kraju`, `wojewodztwo`, `liczba_mieszkancow` | `id` (AUTO_INCREMENT) |
| `jezyki` | 984 wpisów języki×kraj: `kod_kraju`, `jezyk`, `oficjalny` (enum T/F), `procent` | (`kod_kraju`,`jezyk`) |

Relacje (bez JOIN, „ręcznie"): `kraje.stolica` → `miasta.id`, `miasta.kod_kraju` → `kraje.kod`,
`jezyki.kod_kraju` → `kraje.kod` (te ostatnie dwa zdefiniowane jako fizyczne klucze obce).

Uwagi merytoryczne:
- kolumna `wojewodztwo` dla Polski to województwo, dla innych państw odpowiednik regionu/stanu,
- `pkb` to technicznie GNP z oryginalnej bazy — nazwa dobrana pod terminologię szkolną,
- wartości enum `kontynent`: Azja, Europa, Ameryka Północna, Afryka, Oceania, Antarktyda, Ameryka Południowa,
- nazwy krajów/miast w danych pozostały w oryginale (np. `Poland`, `Warszawa`, `Mumbai (Bombay)`).

### Import w XAMPP
1. XAMPP Control Panel → Start **Apache** i **MySQL**.
2. `http://localhost/phpmyadmin` → zakładka **Import** → wybierz `world_pl.sql` → **Import**.
   (Plik sam tworzy bazę `world` — nie trzeba jej tworzyć wcześniej.)

### Dobre właściwości dydaktyczne
- `enum` (kontynenty, `oficjalny`) — idealne do `GROUP BY` i `DISTINCT`,
- kolumny z `NULL` (`srednia_dlugosc_zycia`, `pkb`, `rok_niepodleglosci`) — do `IS NULL` i różnicy `COUNT(*)`/`COUNT(kolumna)`,
- realne liczby (populacje, PKB) — sensowne `ORDER BY`, `BETWEEN`, arytmetyka na kolumnach,
- fizyczne klucze obce — przygotowanie pod `JOIN` na kolejnych zajęciach,
- 5 tys. wierszy — uczniowie widzą sens `LIMIT`/`ORDER BY`.

## `world.sql` — oryginał (angielski, zapasowy)

Niemodyfikowany dump MySQL 8.0 (`city`, `country`, `countrylanguage`) — zapas, gdyby potrzebna
była wersja oryginalna.

## Plan na dalej
- zajęcia z `JOIN` — ta sama baza wystarczy (relacje już zdefiniowane),
- docelowo warto dodać bazę **Sakila** (wynajem DVD, oficjalny przykład MySQL) lub przykładowy
  dump w stylu egzaminu INF.03 (np. sklep/wypożyczalnia).
