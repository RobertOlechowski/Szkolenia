# Skill: Research — badanie tematu

## Kiedy używać

Gdy użytkownik zadaje pytanie badawcze: o sektor, spółkę, ETF, korelację, dostępność instrumentów.

## Procedura

### 1. Rozpoznanie kontekstu

Przeczytaj `knowledge/INDEX.md` — sprawdź:
- Czy temat już istnieje w bazie?
- Czy to rozszerzenie istniejącego tematu czy nowy?
- Gdzie w hierarchii powinien trafić wynik?

### 2. Research

Przeprowadź wyszukiwanie. Dla każdego źródła:
- WebSearch z precyzyjnym zapytaniem
- WebFetch dla obiecujących wyników
- Minimum 3 niezależne źródła na fakt

Typy pytań i podejście:
- **Sektor/branża** → lista głównych graczy, kapitalizacja, giełdy
- **ETF** → nazwa, ticker, TER, skład top 10, AUM, giełda
- **Spółka** → profil, sektor, ticker, giełda, kluczowe dane
- **Korelacja** → dane historyczne, okresy, interpretacja
- **Dostępność u brokera** → ticker + giełda vs. config.yaml

### 3. Zapis wyników

Dla każdego znalezionego podmiotu/tematu:

**Nowy katalog** (jeśli temat nie istnieje):
1. Utwórz katalog w `knowledge/`
2. Utwórz `INDEX.md` z opisem tematu
3. Utwórz pliki artykułów

**Istniejący katalog:**
1. Dodaj nowe pliki
2. Zaktualizuj `INDEX.md` katalogu

### 4. Aktualizacja indeksów

Od dołu do góry:
1. `INDEX.md` w katalogu artykułu
2. `INDEX.md` katalogu nadrzędnego (jeśli istnieje)
3. `knowledge/INDEX.md` — główny indeks

### 5. Powiązania

Sprawdź czy nowe artykuły łączą się z istniejącymi:
- Firma pojawia się w ETF? → dodaj `→ patrz:` w obu
- Firma dostępna u brokera? → dodaj link do pliku brokera
- Korelacja dotyczy sektora? → dodaj powiązanie

## Format artykułu — szablon

```markdown
# [Nazwa podmiotu/tematu]

| Pole | Wartość |
|------|---------|
| Ticker | ... |
| Giełda | ... |
| Sektor | ... |
| Kraj | ... |
| Kapitalizacja | ... |

## [Treść specyficzna dla tematu]

...

---
*Źródła: [lista] · Ostatnia aktualizacja: [data]*
*Materiał informacyjny — nie stanowi rekomendacji inwestycyjnej.*
```
