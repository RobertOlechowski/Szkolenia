# Szkolenia — przykłady projektów z agentem AI

Repozytorium z przykładowymi projektami pokazywanymi podczas prezentacji. Każdy folder to samodzielny projekt przeznaczony do otwarcia w [Claude Code](https://claude.ai/claude-code) (CLI, desktop app lub IDE plugin).

## Projekty

### Prasówka — agregator newsów

Automatyczny agregator wiadomości. Agent zbiera artykuły z polskich i zagranicznych portali, filtruje je według zainteresowań użytkownika i generuje pogrupowane raporty po polsku.

**Szybki start:** otwórz folder `Prasówka` w Claude Code i napisz `zrób prasówkę`.

```
Prasówka/
├── data.yaml          — źródła informacyjne (portale, RSS)
├── preferences.yaml   — preferencje (tematy, priorytety, wykluczenia)
├── reports/           — wygenerowane raporty
└── skills/            — collect-news, organize-knowledge
```

### Research — baza wiedzy inwestycyjnej

Agent-analityk budujący bazę wiedzy w Markdown. Użytkownik zadaje pytania o sektory, spółki, ETF-y — agent przeprowadza research i zapisuje wyniki w hierarchicznej strukturze katalogów.

**Szybki start:** otwórz folder `Research` w Claude Code i zadaj pytanie, np. `zbadaj sektor uranowy`.

```
Research/
├── ai/instrukcje.md   — instrukcje dla agenta
├── knowledge/         — baza wiedzy (rośnie z każdym pytaniem)
│   ├── INDEX.md
│   └── <tematy>/      — artykuły pogrupowane tematycznie
└── skills/            — research, organize, validate
```

## Wymagania

- [Claude Code](https://claude.ai/claude-code) (CLI, desktop app lub plugin do IDE)
- Konto Anthropic z dostępem do Claude
