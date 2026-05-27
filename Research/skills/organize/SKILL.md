# Skill: Organize — porządkowanie bazy wiedzy

## Kiedy używać

- Po serii pytań, gdy baza urosła i wymaga reorganizacji
- Gdy indeksy są nieaktualne
- Gdy katalogi mają za dużo lub za mało plików
- Na żądanie użytkownika

## Procedura

### 1. Audyt struktury

Przejdź całe drzewo `knowledge/`:
- Sprawdź każdy `INDEX.md` — czy lista plików zgadza się z rzeczywistością
- Znajdź pliki bez wpisu w indeksie
- Znajdź wpisy w indeksie bez pliku

### 2. Reorganizacja katalogów

Reguły:
- Katalog z ≥5 artykułami jednego podtematu → wydziel podkatalog
- Katalog z 1 artykułem → rozważ scalenie z katalogiem nadrzędnym
- Osieroce pliki (bez katalogu tematycznego) → przypisz lub utwórz katalog

### 3. Odświeżenie indeksów

Dla każdego `INDEX.md` od dołu do góry:
1. Lista wszystkich plików i podkatalogów
2. Jednolinijkowy opis każdego elementu
3. Data ostatniej aktualizacji
4. Powiązania z innymi tematami

### 4. Czyszczenie treści

- Usuń duplikaty (ta sama firma/ETF opisana w dwóch miejscach)
- Połącz fragmentaryczne artykuły
- Oznacz artykuły bez źródeł → `⚠️ wymaga weryfikacji`

### 5. Raport

Na koniec wypisz:
- Ile artykułów w bazie
- Ile katalogów/podkatalogów
- Co zostało zmienione
- Co wymaga uwagi użytkownika
