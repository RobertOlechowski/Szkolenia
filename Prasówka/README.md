# Prasówka — Agregator Newsów

Automatyczny agregator wiadomości działający w Claude Cowork. Zbiera artykuły z polskich i zagranicznych portali, filtruje je według Twoich zainteresowań i generuje pogrupowane raporty po polsku.

## Szybki start

1. Otwórz ten folder w Claude Cowork
2. Napisz `zrób prasówkę` lub `zbierz newsy`
3. Gotowy raport pojawi się w folderze `reports/`

## Struktura projektu

```
Prasówka/
├── CLAUDE.md              — instrukcje dla agenta (nie edytuj ręcznie)
├── README.md              — ten plik
├── data.yaml              — lista źródeł informacyjnych
├── preferences.yaml       — Twoje preferencje (tematy, priorytety, wykluczenia)
├── reports/               — wygenerowane raporty
│   └── YYYY-MM-DD_HH-MM.md
└── skills/                — skille (umiejętności agenta)
    ├── collect-news/      — zbieranie i analiza newsów
    └── organize-knowledge/— porządkowanie bazy raportów
```

## Konfiguracja

### Źródła wiadomości (`data.yaml`)

Plik zawiera listę portali, z których agent pobiera artykuły. Każde źródło ma następujące pola:

```yaml
- name: "Nazwa portalu"
  url: "https://portal.pl"
  rss: "https://portal.pl/feed/"   # opcjonalne — jeśli null, agent użyje strony głównej
  enabled: true                     # false = źródło pominięte
```

Aby dodać nowe źródło, dopisz wpis do listy `sources`. Aby tymczasowo wyłączyć źródło, zmień `enabled` na `false` (nie usuwaj wpisu — dzięki temu zachowujesz historię konfiguracji).

Na dole pliku znajdują się ustawienia globalne:

```yaml
settings:
  max_article_age_hours: 48    # artykuły starsze niż 48h są pomijane
  request_timeout_seconds: 15  # timeout połączenia
```

### Preferencje użytkownika (`preferences.yaml`)

Tu definiujesz, co Cię interesuje, a co ma być pomijane.

**Priorytety** — tematy z `high_priority` trafiają na górę raportu z czerwonym znacznikiem, tematy z `normal` niżej z niebieskim:

```yaml
interests:
  high_priority:
    - "cyberbezpieczeństwo"
    - "polityka"
    - "gospodarka"
  normal:
    - "AI"
    - "technologia"
```

**Wykluczenia** — tematy, które agent całkowicie pomija:

```yaml
exclude_topics:
  - "sport"
  - "celebryci"
  - "plotki"
```

**Inne opcje:**

- `summary_style` — `"zwięzły"` (2–3 zdania) lub `"szczegółowy"` (3–5 zdań z kontekstem)
- `language` — język raportów (domyślnie `"pl"`, wszystkie streszczenia zawsze po polsku)
- `max_articles` — maksymalna liczba artykułów w jednym raporcie

## Dostępne skille

Skille to gotowe procedury, które agent wykonuje na Twoje polecenie. Projekt zawiera dwa skille:

### 1. Zbieranie i analiza newsów (`collect-news`)

Główny skill projektu. Uruchamiasz go prosząc o prasówkę.

**Przykładowe polecenia:**
- „Zrób prasówkę"
- „Zbierz najnowsze newsy"
- „Wygeneruj raport wiadomości"

**Co robi:**
- Odwiedza wszystkie włączone źródła z `data.yaml` (najpierw RSS, potem stronę główną)
- Filtruje artykuły według Twoich zainteresowań i wykluczeń
- Deduplikuje — jeśli wiele portali pisze o tym samym, łączy je w jeden wpis
- Pisze podsumowania własnymi słowami po polsku
- Zapisuje raport do `reports/YYYY-MM-DD_HH-MM.md`
- Informuje o źródłach, do których nie udało się uzyskać dostępu

**Tip:** Możesz poprosić o zaplanowanie automatycznego wykonywania, np. „Rób prasówkę codziennie o 7:00".

### 2. Porządkowanie bazy raportów (`organize-knowledge`)

Skill do pracy z historią wcześniejszych prasówek.

**Przykładowe polecenia:**
- „Pokaż listę raportów"
- „Znajdź co pisano o AI w ostatnich prasówkach"
- „Porównaj trendy z ostatniego tygodnia"
- „Jakie tematy pojawiały się najczęściej?"

**Co robi:**
- Przegląda raporty w `reports/` i pokazuje statystyki
- Wyszukuje konkretny temat w historii prasówek
- Porównuje raporty — co nowego, co zniknęło, jakie trendy
- Może zaproponować archiwizację starych raportów

## Format raportów

Każdy raport ma stałą strukturę: najpierw newsy o wysokim priorytecie (oznaczone czerwonym znacznikiem), potem normalne (niebieskim). Każdy wpis zawiera krótkie podsumowanie po polsku, kategorię tematyczną i linki do oryginalnych artykułów.

Raporty zapisywane są jako pliki Markdown w folderze `reports/` z nazwą opartą na dacie i godzinie generowania.

## FAQ

**Jak dodać nowe źródło?**
Edytuj `data.yaml` i dopisz nowy wpis do listy `sources` z odpowiednimi polami.

**Jak zmienić priorytety tematów?**
Edytuj `preferences.yaml` — przenieś temat między `high_priority` a `normal` lub dodaj nowy.

**Jak wykluczyć temat?**
Dodaj go do listy `exclude_topics` w `preferences.yaml`.

**Jak zaplanować automatyczną prasówkę?**
Poproś agenta: „Rób prasówkę codziennie o 7 rano". Użyje wbudowanego mechanizmu zaplanowanych zadań.

**Agent nie może się połączyć ze źródłem — co robić?**
Agent spróbuje alternatywnych metod (RSS → strona główna → wyszukiwanie). Jeśli żadna nie zadziała, zaznaczy to w raporcie. Możesz wyłączyć problematyczne źródło ustawiając `enabled: false` w `data.yaml`.
