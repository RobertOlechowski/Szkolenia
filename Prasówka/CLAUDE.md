# Prasówka — Agregator Newsów

## Opis
Agent zbiera wiadomości z portali zdefiniowanych w `data.yaml`, filtruje je wg `preferences.yaml` i zapisuje raporty w `reports/`.

## Pliki

- `CLAUDE.md` — ten plik; nie edytuj bez polecenia użytkownika
- `data.yaml` — źródła informacyjne (nazwa, URL, RSS)
- `preferences.yaml` — preferencje użytkownika; edytuj TYLKO na polecenie
- `reports/YYYY-MM-DD_HH-MM.md` — raporty
- `skills/collect-news/SKILL.md` — procedura zbierania newsów (główny skill)
- `skills/organize-knowledge/SKILL.md` — procedura porządkowania raportów

## Pliki tymczasowe
Agent może tworzyć pliki tymczasowe w katalogu roboczym (outputs) podczas pracy — np. do zbierania surowych danych ze źródeł przed filtrowaniem. Pliki tymczasowe NIE trafiają do `reports/`.

## Kluczowe zasady

### Czas
Użytkownik jest w strefie `Europe/Warsaw`. Czas w nazwie pliku i nagłówku raportu MUSI odpowiadać rzeczywistemu czasowi polskiemu. Zawsze pobierz go komendą:
```bash
TZ="Europe/Warsaw" date '+%H-%M'
```
Nigdy nie zgaduj godziny. Nigdy nie używaj czasu UTC bez konwersji.

### Język i treść
- Wszystkie streszczenia ZAWSZE po polsku — niezależnie od języka źródła
- Pisz własne podsumowania — nie kopiuj treści artykułów
- Zawsze podawaj link do oryginału

### Filtrowanie
- Stosuj priorytety i wykluczenia z `preferences.yaml`
- Używaj pola `keywords` w `preferences.yaml` do dopasowywania artykułów — matchuj zarówno polskie, jak i angielskie warianty słów kluczowych
- Artykuły niepasujące do ŻADNEJ kategorii zainteresowań — pomijaj
- Ogranicz liczbę artykułów do `max_articles` z `preferences.yaml`
- Grupuj powiązane tematy razem; deduplikuj wpisy z wielu źródeł

### Źródła i narzędzia
- Główne źródło artykułów: lista z `data.yaml` (tylko `enabled: true`)
- Artykuły spoza listy źródeł: można uwzględnić TYLKO jako dodatkowe źródło do istniejącego tematu, nie jako samodzielny wpis
- Łańcuch fallback dla każdego źródła: RSS (web_fetch) → strona główna (web_fetch) → WebSearch z domeną → Chrome (navigate + get_page_text)
- Jeśli wszystkie metody zawiodą — zaznacz w sekcji ⚠️ Uwagi raportu

### Konfiguracja
- `data.yaml`: edytuj swobodnie; wyłączaj źródła flagą `enabled: false` zamiast usuwać
- `preferences.yaml`: NIE edytuj bez polecenia użytkownika; możesz sugerować zmiany

### Po wygenerowaniu raportu
Zaproponuj zaplanowanie regularnego wykonywania (scheduled task), np. „Chcesz, żebym robił prasówkę codziennie o stałej porze?"
