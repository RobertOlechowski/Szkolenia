# Skill: Validate — szukanie sprzeczności i problemów

## Kiedy używać

- Po dodaniu nowych artykułów
- Gdy użytkownik chce zweryfikować spójność bazy
- Przed podjęciem decyzji inwestycyjnych na podstawie bazy

## Procedura

### 1. Przegląd cross-referencji

Dla każdego artykułu sprawdź:
- Czy dane liczbowe (kapitalizacja, TER, AUM) są spójne między artykułami
- Czy ta sama firma ma identyczne dane wszędzie gdzie jest wymieniona
- Czy skład ETF zgadza się z profilami firm

### 2. Sprzeczności faktyczne

Szukaj:
- Firma przypisana do różnych sektorów w różnych artykułach
- Różne wartości tego samego wskaźnika
- Sprzeczne informacje o dostępności u brokerów
- Nieaktualne dane (porównaj daty źródeł)

### 3. Kompletność

Sprawdź:
- Artykuły bez sekcji "Źródła" → oznacz `⚠️ brak źródeł`
- Artykuły z jednym źródłem → oznacz `⚠️ jedno źródło`
- Powiązania jednostronne (A linkuje do B, ale B nie do A)
- Firmy wymienione w ETF bez własnego artykułu

### 4. Aktualność

- Artykuły starsze niż 30 dni → oznacz `📅 wymaga odświeżenia`
- Dane rynkowe (ceny, kapitalizacja) → zawsze oznacz datę
- Zmiany regulacyjne → sprawdź czy nie zaszły aktualizacje

### 5. Raport walidacji

Przygotuj raport w formacie:

```
## Raport walidacji — [data]

### 🔴 Sprzeczności
- [opis + pliki]

### 🟡 Niekompletne
- [opis + pliki]

### 📅 Do odświeżenia
- [lista artykułów]

### ✅ Spójne
- [podsumowanie]
```

Zapisz raport w `knowledge/VALIDATION_REPORT.md` i wyświetl użytkownikowi.
