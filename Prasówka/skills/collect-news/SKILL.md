---
name: collect-news
description: "Zbiera artykuły z portali informacyjnych, filtruje wg preferencji użytkownika i generuje pogrupowany raport po polsku. Uruchamiaj gdy użytkownik prosi o prasówkę, newsy, raport wiadomości."
---

# Skill: Zbieranie i analiza newsów

## Kiedy używać
Gdy użytkownik prosi o prasówkę, zebranie newsów, wygenerowanie raportu lub aktualizację wiadomości.

## Procedura krok po kroku

### 1. Wczytaj konfigurację
Przeczytaj oba pliki — kolejność ma znaczenie:
1. `preferences.yaml` — zainteresowania (z keywords), wykluczenia, styl, limit
2. `data.yaml` — źródła (`enabled: true`), ustawienia globalne (`max_article_age_hours`)

### 2. Pobierz aktualny czas
Przed rozpoczęciem zbierania, pobierz czas polski:
```bash
TZ="Europe/Warsaw" date '+%Y-%m-%d %H-%M'
```
Zapisz wynik — użyjesz go w nazwie pliku i nagłówku raportu.

### 3. Zbierz artykuły ze źródeł

**Przetwarzaj źródła równolegle** — wysyłaj wiele zapytań jednocześnie (np. kilka wywołań WebSearch lub web_fetch naraz), aby przyspieszyć zbieranie. Agent sam czyta każdy artykuł i na podstawie treści klasyfikuje go tematycznie.

Dla każdego włączonego źródła stosuj łańcuch fallback w podanej kolejności:

**Krok A — RSS przez web_fetch:**
Użyj `web_fetch` z adresem RSS z `data.yaml`. Parsuj XML — wyciągnij tytuł, link, datę, opis.

**Krok B — Strona główna przez web_fetch (jeśli RSS zawiódł):**
Użyj `web_fetch` z polem `url` ze źródła. Wyciągnij nagłówki i linki z HTML.

**Krok C — WebSearch z domeną (jeśli web_fetch zawiódł):**
Wykonaj `WebSearch` z zapytaniem: `site:{domena} najnowsze` lub `site:{domena} latest`.

**Krok D — Chrome (jeśli wszystko powyżej zawiodło):**
Użyj `navigate` + `get_page_text` z narzędzi Chrome, aby pobrać treść strony renderowanej przez JavaScript.

**Krok E — Zaznacz niedostępność:**
Jeśli żadna metoda nie zadziałała, dodaj źródło do listy niedostępnych.

**Uwagi do zbierania:**
- Filtruj artykuły starsze niż `max_article_age_hours` (porównaj daty z RSS/strony z bieżącą datą)
- Agent może tworzyć pliki tymczasowe w katalogu roboczym do przechowywania surowych danych ze źródeł

### 4. Filtruj i kategoryzuj

Agent sam czyta treść/tytuł/opis każdego artykułu i dopasowuje do kategorii na podstawie `keywords` z `preferences.yaml`.

Dla każdego zebranego artykułu:

1. **Sprawdź wykluczenia:** Porównaj tytuł i opis z listą `keywords` w `exclude_topics`. Jeśli artykuł matchuje wykluczenie — odrzuć.
2. **Przypisz kategorię:** Porównaj tytuł i opis z listami `keywords` w `interests.high_priority` i `interests.normal`. Szukaj dopasowań zarówno w polskich, jak i angielskich wariantach.
3. **Przypisz priorytet:**
   - 🔴 **Wysoki** — matchuje `interests.high_priority`
   - 🔵 **Normalny** — matchuje `interests.normal`
   - **Pomiń** — nie matchuje żadnej kategorii (nie umieszczaj w raporcie)
4. Jeśli artykuł pasuje do wielu kategorii, przypisz go do tej o wyższym priorytecie.

### 5. Zdeduplikuj

- Porównaj zebrane artykuły — jeśli wiele źródeł opisuje to samo wydarzenie, połącz w jeden wpis
- Zachowaj linki do WSZYSTKICH źródeł opisujących dany temat
- Wybierz najpełniejsze źródło jako bazę podsumowania

### 6. Napisz podsumowania

Dla każdego unikalnego newsa:
- **ZAWSZE po polsku** — niezależnie od języka źródła
- **Własnymi słowami** — nie kopiuj treści artykułów
- Styl zależy od `summary_style`:
  - `zwięzły` — 2–3 zdania, sam rdzeń informacji
  - `szczegółowy` — 3–5 zdań, z kontekstem i tłem
- Ogranicz łączną liczbę artykułów do `max_articles`
- Priorytet przy obcinaniu: zachowaj artykuły 🔴 Wysoki priorytet, obcinaj 🔵 Normalny

### 7. Wygeneruj raport

Nazwa pliku: `reports/YYYY-MM-DD_HH-MM.md` (czas z kroku 2).

Struktura:
```markdown
# Prasówka — YYYY-MM-DD HH:MM

> Źródła: [lista użytych źródeł] | Artykułów: N | Niedostępne: [lista lub "brak"]

---

## 🔴 Wysoki priorytet

### [Nazwa kategorii]

#### Tytuł newsa
[Podsumowanie 3-5 zdań po polsku]
- **Kategoria:** nazwa
- **Źródła:** [link1](url), [link2](url)

---

## 🔵 Normalny priorytet

[ta sama struktura co wyżej]

---

## ⚠️ Uwagi
- [niedostępne źródła, problemy techniczne, uwagi o fallbackach]
```

### 8. Zapisz i potwierdź

1. Zapisz raport do `reports/`
2. Poinformuj użytkownika: ile artykułów, ile źródeł, ewentualne problemy
3. Zaproponuj scheduled task: „Chcesz, żebym robił prasówkę codziennie o stałej porze?"

## Typowe błędy — unikaj ich

- **Błędny czas:** Nie używaj czasu UTC w nazwie pliku. Zawsze konwertuj na Europe/Warsaw.
- **Kopiowanie treści:** Nie wklejaj fragmentów artykułów. Pisz podsumowania własnymi słowami.
- **Artykuły spoza listy:** Nie dodawaj artykułów znalezionych przez WebSearch jako samodzielnych wpisów. Mogą być jedynie dodatkowym źródłem do istniejącego tematu.
- **Puste kategorie:** Jeśli w danym priorytecie nie ma artykułów, pomiń całą sekcję (nie pisz „brak artykułów").
- **Ignorowanie keywords:** Używaj pola `keywords` z `preferences.yaml` do dopasowywania — nie polegaj tylko na nazwie kategorii.
- **Sekwencyjne przetwarzanie:** Przeszukuj źródła równolegle (wiele wywołań naraz), nie jedno po drugim.
