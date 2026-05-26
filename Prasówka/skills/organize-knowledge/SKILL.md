---
name: organize-knowledge
description: "Przegląda historię raportów z prasówek, wyszukuje tematy, porównuje trendy i porządkuje katalog reports/. Uruchamiaj gdy użytkownik pyta o wcześniejsze raporty, trendy lub szuka informacji w historii."
---

# Skill: Porządkowanie bazy raportów

## Kiedy używać
Gdy użytkownik prosi o: przegląd raportów, wyszukanie tematu w historii prasówek, porównanie trendów, statystyki lub porządkowanie katalogu `reports/`.

## Możliwości

### Przegląd raportów
- Wylistuj dostępne raporty z katalogu `reports/`
- Pokaż statystyki: ile raportów, zakres dat, najczęstsze tematy

### Wyszukiwanie w historii
- Znajdź konkretny temat lub wydarzenie we wcześniejszych raportach
- Pokaż jak temat ewoluował w kolejnych prasówkach

### Porównanie trendów
- Porównaj dwa lub więcej raportów — co nowego, co zniknęło
- Zidentyfikuj tematy przewijające się regularnie

### Porządkowanie
- Sprawdź spójność formatowania raportów
- Zaproponuj archiwizację starych raportów (np. > 30 dni)

## Procedura wyszukiwania

1. Wylistuj pliki w `reports/` posortowane chronologicznie
2. Użyj Grep do szybkiego wyszukania fraz w raportach
3. Przeczytaj odpowiednie raporty (Read) dla kontekstu
4. Przeanalizuj wyniki i odpowiedz użytkownikowi

## Procedura podsumowania trendów

1. Przeczytaj ostatnie N raportów (domyślnie 5)
2. Wyodrębnij tematy i ich częstość
3. Wygeneruj podsumowanie trendów z listą najgorętszych tematów
4. Opcjonalnie zapisz do pliku `reports/trends-YYYY-MM.md`
