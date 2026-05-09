# Plan poprawek raportu dostępności — domknięcie audytu

> **Dla agentów wykonawczych:** WYMAGANA SUB-SKILL: użyj `superpowers:subagent-driven-development` (rekomendowane) lub `superpowers:executing-plans` do wykonania planu zadanie po zadaniu. Kroki używają składni checkbox (`- [ ]`) do śledzenia postępu.

**Cel:** Doprowadzić audyt dostępności (raport z 2026-05-07) do stanu w pełni kompletnego: (1) wykonać manualną weryfikację a11y w przeglądarce (T5.2 z planu progresu) oraz (2) skorygować w raporcie nieaktualne odniesienia (git, ścieżki plików, numery linii, liczniki testów/stron) wykryte podczas weryfikacji 2026-05-09.

**Architektura:** Plan dwuwarstwowy. Faza A wykonuje brakującą weryfikację manualną, zapisując wyniki w nowym pliku `docs/raport-dostepnosci-weryfikacja-manualna.md`. Faza B koryguje główny raport `docs/raport-dostepnosci-2026-05-07.md` — wymienia metadane gałęzi/commitów na zgodne z obecnym repo (`__METRO_catalogs_stage2`, branch `main`), aktualizuje numery linii w `src/app/globals.css`, usuwa odniesienia do nieistniejących plików `CatalogPageType2.tsx` / `CatalogPageType3.tsx`, prostuje liczniki (testy 44→82, strony 10→22). Faza C dodaje aneks weryfikacyjny i ostateczny status. Każde zadanie tworzy izolowane, weryfikowalne zmiany dokumentacji i jest commitowane osobno.

**Tech Stack:** Markdown, Next.js 15.5.12 (do uruchomienia preview na potrzeby Lighthouse / axe DevTools), Chrome DevTools (Lighthouse 11+ wbudowany), [axe DevTools](https://www.deque.com/axe/devtools/) (rozszerzenie Chrome/Firefox), VoiceOver (macOS — wbudowany, `Cmd+F5`), opcjonalnie NVDA (Windows). `npm run build` + `npm start` jako stabilne preview.

---

## Pliki

**Tworzone:**
- `docs/raport-dostepnosci-weryfikacja-manualna.md` — log weryfikacji manualnej z datami, narzędziami, rezultatami per strona.
- `docs/screenshots/lighthouse/` (katalog) — zrzuty ekranu z Lighthouse i axe DevTools (`.png`).

**Modyfikowane:**
- `docs/raport-dostepnosci-2026-05-07.md` — sekcje 3, 4 (Zasada 7), 5, 7 oraz wstęp (metadane).

**Bez zmian (referencyjne):**
- `docs/superpowers/plans/2026-05-07-accessibility-progress.md` — pozostaje jako historyczny rejestr planu.
- `.ui-design/audits/metro_catalogs_zasady_20260507_115012.md` — audyt źródłowy.

---

## Faza A — Wykonanie manualnej weryfikacji a11y (T5.2)

Cel fazy: zebrać twarde dowody (numeryczne wyniki, screenshoty, zapiski z testów AT) potwierdzające, że to, co przechodzi w testach jednostkowych, działa również w realnej przeglądarce z prawdziwym czytnikiem ekranu.

---

### Task A1: Przygotowanie środowiska i logu weryfikacyjnego

**Pliki:**
- Utwórz: `docs/raport-dostepnosci-weryfikacja-manualna.md`
- Utwórz: `docs/screenshots/lighthouse/.gitkeep` (pusty plik, by katalog istniał w repo)

- [ ] **Krok 1: Utworzenie szkieletu pliku weryfikacyjnego**

Utwórz plik `docs/raport-dostepnosci-weryfikacja-manualna.md` z poniższą zawartością:

```markdown
# Raport weryfikacji manualnej dostępności

**Data wykonania:** _do uzupełnienia_
**Wykonawca:** _do uzupełnienia (imię, nazwisko, rola)_
**Środowisko:** macOS 15.x / Chrome 132+ / VoiceOver / Firefox 134+ / axe DevTools 4.x
**Build pod testem:** `npm run build && npm start` (port 3000) — commit referencyjny: _do uzupełnienia_
**Strony reprezentatywne:**
1. `/` (strona startowa katalogu)
2. `/catalog/QX` (kolekcja QX)
3. `/catalog/QS` (kolekcja QS)

**Stosowany dokument odniesienia:** [docs/raport-dostepnosci-2026-05-07.md](./raport-dostepnosci-2026-05-07.md)

---

## A. Lighthouse Accessibility (Chrome DevTools)

| Strona | Wynik (0–100) | Liczba zaleceń | Screenshot |
|--------|---------------|----------------|------------|
| `/`           | _TBD_ | _TBD_ | `screenshots/lighthouse/home.png` |
| `/catalog/QX` | _TBD_ | _TBD_ | `screenshots/lighthouse/qx.png` |
| `/catalog/QS` | _TBD_ | _TBD_ | `screenshots/lighthouse/qs.png` |

**Próg akceptacji:** ≥95 na każdej stronie.
**Komentarze do zaleceń (jeśli były):** _TBD_

---

## B. axe DevTools (Deque)

| Strona | Violations | Needs review | Best practices |
|--------|------------|--------------|----------------|
| `/`           | _TBD_ | _TBD_ | _TBD_ |
| `/catalog/QX` | _TBD_ | _TBD_ | _TBD_ |
| `/catalog/QS` | _TBD_ | _TBD_ | _TBD_ |

**Próg akceptacji:** 0 violations na każdej stronie.
**Lista violations (jeśli były):** _TBD_

---

## C. Walka z klawiaturą — pełna obsługa Tab/Shift+Tab/Enter/Escape/strzałki

| Scenariusz | Wynik |
|---|---|
| Tab od początku strony — kolejność fokusa zgodna z wizualną | _TBD_ |
| Skip link widoczny po pierwszym Tabie, działa po Enter | _TBD_ |
| Otwórz Lightbox z Gallery — Tab cyklicznie krąży w modalu | _TBD_ |
| Escape zamyka Lightbox, fokus wraca na miniaturę | _TBD_ |
| Strzałki ←/→ nawigują w Lightbox | _TBD_ |
| Hero slider — strzałki ←/→ zmieniają slajd, autoplay zatrzymany przy interakcji | _TBD_ |
| MaterialsOptionGroup — Tab po opcjach, Space/Enter wybiera | _TBD_ |
| FinishesQX modal preview — focus trap + Escape | _TBD_ |
| PackshotsQX lightbox — focus trap + Escape | _TBD_ |
| Brak focus traps na nieoczekiwanych elementach (np. Carousel autoplay nie kradnie fokusa) | _TBD_ |

---

## D. Czytnik ekranu (VoiceOver — macOS)

Sekwencja testowa: VO+→ przejście liniowe + VO+U rotor (Headings, Landmarks, Links, Form Controls).

| Scenariusz | Wynik |
|---|---|
| `<h1>` per katalog jest pierwszym nagłówkiem — VO odczytuje tytuł kolekcji | _TBD_ |
| Rotor Landmarks pokazuje `<header>`, `<nav aria-label="Catalog sections">`, `<main>`, `<footer>` | _TBD_ |
| MaterialsOptionGroup — VO ogłasza nazwę grupy (z `aria-labelledby`) i każdą opcję jako kolor + nazwę | _TBD_ |
| ColorChip tooltip — VO ogłasza pełną etykietę (np. „Frame: RAL 9006 White") na fokus | _TBD_ |
| Lightbox przy otwarciu — VO ogłasza `dialog`, tytuł i licznik „Image N of M: <alt>" | _TBD_ |
| `aria-live="polite"` na liczniku Lightbox — przejście slajdu odczytane bez zatrzymywania nawigacji | _TBD_ |
| FeaturesQX video — VO nie odczytuje wideo (`aria-hidden="true"`), za to odczytuje `sr-only` opis | _TBD_ |
| Skip link odczytany jako pierwszy element po wejściu na stronę | _TBD_ |
| `aria-current="location"` w nawigacji — VO ogłasza aktywną sekcję jako „bieżącą lokalizację" | _TBD_ |

**Komentarze:** _TBD_

---

## E. Reflow i zoom

| Scenariusz | Wynik |
|---|---|
| Chrome DevTools → Responsive 320×640 → strona `/` renderuje bez poziomego scrollbara | _TBD_ |
| 320×640 → `/catalog/QX` renderuje bez poziomego scrollbara | _TBD_ |
| Zoom 200% (Cmd+`+`) na `/catalog/QX` — czytelny tekst, brak utraty treści, brak poziomego scrolla | _TBD_ |
| Zoom 400% (WCAG AAA 1.4.10) — content adapts (best-effort, nie wymagane przez AA) | _TBD_ |

---

## F. Wnioski

_Do uzupełnienia po zakończeniu testów A–E:_
- [ ] Wszystkie 3 strony osiągnęły Lighthouse ≥95 i axe 0 violations.
- [ ] Klawiatura w pełni funkcjonalna we wszystkich 10 scenariuszach.
- [ ] Czytnik ekranu poprawnie ogłasza wszystkie kluczowe elementy.
- [ ] Reflow 320 px i zoom 200% bez błędów.

**Ostateczny werdykt:** _PASS / FAIL z listą blokerów_

---

**Podpis:** _____________________
**Data:** _______________
```

- [ ] **Krok 2: Utworzenie katalogu na screenshoty**

Wykonaj:

```bash
mkdir -p docs/screenshots/lighthouse
touch docs/screenshots/lighthouse/.gitkeep
```

- [ ] **Krok 3: Commit szkieletu**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md docs/screenshots/lighthouse/.gitkeep
git commit -m "docs(a11y): add manual verification log skeleton"
```

---

### Task A2: Lighthouse Accessibility audit na 3 stronach

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-weryfikacja-manualna.md` (sekcja A)
- Twórz: `docs/screenshots/lighthouse/home.png`, `qx.png`, `qs.png`

- [ ] **Krok 1: Build i start lokalnego serwera produkcyjnego**

W terminalu projektu:

```bash
npm run build
npm start
```

Oczekiwane: `Local: http://localhost:3000` na czystym buildzie produkcyjnym.

- [ ] **Krok 2: Lighthouse na `/`**

W Chrome:
1. Otwórz tryb incognito (eliminuje wpływ rozszerzeń).
2. Wejdź na `http://localhost:3000/`.
3. DevTools → zakładka **Lighthouse** → kategoria: **Accessibility** (odznacz pozostałe), tryb: **Navigation**, urządzenie: **Desktop** → **Analyze page load**.
4. Zapisz screenshot wyniku jako `docs/screenshots/lighthouse/home.png` (Cmd+Shift+4 → wybór obszaru raportu Lighthouse).
5. Wpisz wynik w sekcji A pliku weryfikacyjnego.

Oczekiwane: wynik ≥95.

- [ ] **Krok 3: Lighthouse na `/catalog/QX` i `/catalog/QS`**

Powtórz krok 2 dla `http://localhost:3000/catalog/QX` (zapisz `qx.png`) i `http://localhost:3000/catalog/QS` (zapisz `qs.png`). Wpisz wyniki w tabeli.

Oczekiwane: ≥95 na każdej stronie.

- [ ] **Krok 4: Reakcja na ewentualne zalecenia**

Jeśli Lighthouse zwróci wynik < 95 lub nieprzewidziane zalecenia, **NIE łataj na ślepo** — odnotuj listę zaleceń w sekcji A pod tabelą i przejdź do Task A6 (rejestr blokerów). Plan poprawek na poziomie kodu wykracza poza zakres tego dokumentu — wymaga osobnego planu z brainstormingiem.

- [ ] **Krok 5: Commit wyników Lighthouse**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md docs/screenshots/lighthouse/*.png
git commit -m "docs(a11y): record Lighthouse accessibility scores for /, /catalog/QX, /catalog/QS"
```

---

### Task A3: axe DevTools audit na 3 stronach

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-weryfikacja-manualna.md` (sekcja B)
- Twórz: `docs/screenshots/lighthouse/axe-home.png`, `axe-qx.png`, `axe-qs.png`

- [ ] **Krok 1: Instalacja axe DevTools (jeśli brak)**

Zainstaluj rozszerzenie [axe DevTools — Web Accessibility Testing](https://chrome.google.com/webstore/detail/axe-devtools-web-accessib/lhdoppojpmngadmnindnejefpokejbdd) z Chrome Web Store.

- [ ] **Krok 2: Skan każdej strony**

Dla każdej z trzech stron (z tym samym buildem co w Task A2):
1. Otwórz stronę w trybie incognito.
2. DevTools → zakładka **axe DevTools** → **Scan ALL of my page**.
3. Zapisz screenshot wyniku (`axe-home.png`, `axe-qx.png`, `axe-qs.png`).
4. Wpisz liczby Violations / Needs review / Best practices do tabeli w sekcji B.

Oczekiwane: 0 Violations na każdej stronie.

- [ ] **Krok 3: Reakcja na ewentualne violations**

Jak w Task A2 — odnotuj w sekcji B i wnieś do Task A6, **bez modyfikowania kodu w obrębie tego planu**.

- [ ] **Krok 4: Commit**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md docs/screenshots/lighthouse/axe-*.png
git commit -m "docs(a11y): record axe DevTools scan results"
```

---

### Task A4: Walka z klawiaturą — pełny przebieg

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-weryfikacja-manualna.md` (sekcja C)

- [ ] **Krok 1: Wykonanie 10 scenariuszy keyboard-only**

Z tym samym buildem produkcyjnym, używając WYŁĄCZNIE klawiatury (mysz odłożona / touchpad nie używany), wykonaj każdy scenariusz z tabeli w sekcji C. Zapisz dla każdego: `PASS` / `FAIL: <opis>`.

Pomocne klawisze:
- `Tab` / `Shift+Tab` — nawigacja
- `Enter` / `Space` — aktywacja
- `Escape` — zamknięcie modala
- `←` `→` `↑` `↓` — nawigacja w komponentach (slider, lightbox)

- [ ] **Krok 2: Reakcja na FAIL**

Każdy `FAIL` przenieś do Task A6 z opisem regresji (selektor / komponent / oczekiwane vs faktyczne zachowanie).

- [ ] **Krok 3: Commit**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md
git commit -m "docs(a11y): record keyboard-only navigation walkthrough"
```

---

### Task A5: Testy z czytnikiem ekranu (VoiceOver)

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-weryfikacja-manualna.md` (sekcja D)

- [ ] **Krok 1: Aktywacja VoiceOver**

Cmd+F5 (lub Touch ID 3× na MacBookach z Touch Bar). Jeśli pierwsze użycie — uruchom Quick Start Tutorial z menu VoiceOver Utility.

- [ ] **Krok 2: Wykonanie 9 scenariuszy z sekcji D**

Z buildem produkcyjnym (Safari rekomendowane dla VoiceOver, choć Chrome działa) wykonaj każdy scenariusz. Kluczowe gesty VO:
- `VO+→` — następny element
- `VO+←` — poprzedni
- `VO+U` — rotor (zmiana kategorii: Headings, Landmarks, Links, Form Controls)
- `VO+Space` — kliknij / aktywuj
- `Cmd+L` — wstaw URL (do nawigacji między stronami)

Dla każdego scenariusza zanotuj: czy VO ogłosił poprawnie + cytuj treść ogłoszenia.

- [ ] **Krok 3: Deaktywacja VoiceOver po teście**

Cmd+F5.

- [ ] **Krok 4: Commit**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md
git commit -m "docs(a11y): record VoiceOver screen reader test results"
```

---

### Task A6: Reflow 320 px i zoom 200%

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-weryfikacja-manualna.md` (sekcja E + F)

- [ ] **Krok 1: Reflow 320 px na obu katalogach**

Chrome DevTools → ikona Toggle Device Toolbar (Cmd+Shift+M) → ustaw Responsive `320 × 640`. Wejdź kolejno na `/`, `/catalog/QX`. Sprawdź:
- Brak poziomego scrollbara na całej długości strony (przewiń od góry do dołu).
- Wszystkie sekcje renderują się czytelnie.

Zapisz wynik w sekcji E.

- [ ] **Krok 2: Zoom 200%**

Wróć do widoku Desktop (1440 px). `Cmd+` (przybliż) ×2, by uzyskać 200%. Sprawdź:
- Tekst w Hero / Overview / Materials nie ucina się ani nie zachodzi.
- Brak poziomego scrolla.
- Modale (Lightbox, FinishesQX) wciąż mieszczą się w viewport.

Zapisz wynik w sekcji E.

- [ ] **Krok 3: Wypełnienie sekcji F (Wnioski)**

Na podstawie wyników A–E zaznacz checkboxy w sekcji F i wpisz ostateczny werdykt (PASS / FAIL + lista blokerów jeśli są). Wpisz datę i imię wykonawcy w nagłówku.

- [ ] **Krok 4: Commit finalizujący Fazę A**

```bash
git add docs/raport-dostepnosci-weryfikacja-manualna.md
git commit -m "docs(a11y): finalize manual verification log with reflow/zoom and verdict"
```

---

## Faza B — Korekta raportu głównego

Cel: doprowadzić `docs/raport-dostepnosci-2026-05-07.md` do zgodności z aktualnym stanem repo (`__METRO_catalogs_stage2`, branch `main`) i zlikwidować wszystkie nieaktualne odniesienia wykryte w weryfikacji 2026-05-09.

---

### Task B1: Aktualizacja metadanych raportu (nagłówek + sekcja 3)

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (linie 1–10, 41)

- [ ] **Krok 1: Wymiana metadanych w nagłówku**

W pliku [docs/raport-dostepnosci-2026-05-07.md](docs/raport-dostepnosci-2026-05-07.md) zamień blok od linii 5 do 7:

**OLD (linie 5–7):**
```
**Wykonawca raportu:** Zespół wdrożeniowy METRO Catalogs.
**Wersja oprogramowania:** branch `audyt_fix`, commit referencyjny `27d1578` (na bazie `f6508ab`).
**Standard odniesienia:** Web Content Accessibility Guidelines (WCAG) 2.1 na poziomie AA.
```

**NEW:**
```
**Wykonawca raportu:** Zespół wdrożeniowy METRO Catalogs.
**Repozytorium:** `__METRO_catalogs_stage2`, branch `main` — historia po skondensowanym imporcie ze stage 1 (oryginalna gałąź `audyt_fix` z 28 commitami została zaplaszczona; szczegółowy rejestr zmian per zadanie pozostaje dostępny w [docs/superpowers/plans/2026-05-07-accessibility-progress.md](./superpowers/plans/2026-05-07-accessibility-progress.md)).
**Standard odniesienia:** Web Content Accessibility Guidelines (WCAG) 2.1 na poziomie AA.
```

- [ ] **Krok 2: Aktualizacja sekcji 3 punkt 3 (linia 41)**

**OLD (linia 41):**
```
3. **Wdrożenie napraw:** 28 zatwierdzonych zmian (git commits) na gałęzi `audyt_fix` — pełny rejestr w [docs/superpowers/plans/2026-05-07-accessibility-progress.md](./superpowers/plans/2026-05-07-accessibility-progress.md).
```

**NEW:**
```
3. **Wdrożenie napraw:** 28 zatwierdzonych zmian wdrożonych pierwotnie na gałęzi `audyt_fix` (stage 1). Po imporcie do `__METRO_catalogs_stage2` historia git została skondensowana; tożsamość zmian na poziomie zadań (T0.1–T5.1) pozostaje wiernie udokumentowana w [docs/superpowers/plans/2026-05-07-accessibility-progress.md](./superpowers/plans/2026-05-07-accessibility-progress.md), a faktyczna obecność wszystkich mechanizmów w drzewie roboczym została potwierdzona automatyczną weryfikacją (typecheck, jest-axe, build) z 2026-05-09.
```

- [ ] **Krok 3: Aktualizacja licznika testów (linia 42)**

**OLD (linia 42):**
```
4. **Weryfikacja automatyczna:** zestaw testów jednostkowych z biblioteką `jest-axe` (axe-core) — 44 testy zaliczone, 0 niezaliczonych.
```

**NEW:**
```
4. **Weryfikacja automatyczna:** zestaw testów jednostkowych (vitest + jest-axe / axe-core) — **82 testy zaliczone, 1 pominięty** (`overview-min-size.test.ts` — pominięcie pre-existing, niezwiązane z dostępnością), **0 niezaliczonych**. Stan na 2026-05-09 (re-run `npm run test`).
```

- [ ] **Krok 4: Aktualizacja licznika stron build (linia 43)**

**OLD (linia 43):**
```
5. **Testy regresji:** typecheck (`tsc --noEmit`) — 0 błędów; build produkcyjny — 10 stron statycznych wygenerowanych pomyślnie.
```

**NEW:**
```
5. **Testy regresji:** typecheck (`tsc --noEmit`) — 0 błędów; build produkcyjny (`npm run build`) — **22 trasy wygenerowane pomyślnie** (5 prerenderowanych statycznie: `/`, `/_not-found`, `/design-system`, `/robots.txt`, `/sitemap.xml`; 2 SSG: `/catalog/QX`, `/catalog/QS`; 15 dynamicznych funkcji serwerowych). Stan na 2026-05-09.
```

- [ ] **Krok 5: Weryfikacja zmian**

```bash
grep -n "audyt_fix\|27d1578\|f6508ab\|44 testy\|10 stron statycznych" docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: tylko jedna linia z `audyt_fix` (kontekst historyczny: „pierwotnie na gałęzi `audyt_fix`"); zero wystąpień `27d1578`, `f6508ab`, `44 testy`, `10 stron statycznych`.

- [ ] **Krok 6: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): update report metadata to reflect stage2 repo state"
```

---

### Task B2: Korekta odniesień do nieistniejących CatalogPageType2/Type3

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (linie 102, 210)

- [ ] **Krok 1: Weryfikacja stanu kodu**

Wykonaj:

```bash
ls src/components/CatalogPageType*.tsx 2>&1
grep -rn "layoutType" src/types/catalog.ts src/lib/catalog-loader.ts src/app/catalog/
```

Oczekiwane: pliki `CatalogPageType2.tsx` / `CatalogPageType3.tsx` **nie istnieją**; `layoutType` przyjmuje wartości `qx | type2 | type3` w schemacie, ale tylko `qx` ma realny komponent — pozostałe trafiają do `CatalogPagePlaceholder`.

- [ ] **Krok 2: Korekta Zasady 2 (linia 102)**

**OLD (linia 102):**
```
   - Reflow przy szerokości 320 px — usunięto sztywne szerokości `lg:w-[721px]` i `max-w-xl` w komponentach `MaterialsQX`, `CatalogPageType2`, `CatalogPageType3` (WCAG 1.4.10).
```

**NEW:**
```
   - Reflow przy szerokości 320 px — usunięto sztywne szerokości `lg:w-[721px]` w komponencie [`MaterialsQX`](../src/layouts/qx/MaterialsQX.tsx#L186) (zamienione na `lg:w-full lg:max-w-[721px]`). Komponent [`CatalogPagePlaceholder`](../src/components/catalog/CatalogPagePlaceholder.tsx#L30), pełniący rolę zaślepki dla nieukończonych typów layoutu (`type2`, `type3` — przewidziane dla przyszłych kolekcji), używa `max-w-full sm:max-w-xl` (WCAG 1.4.10).
```

- [ ] **Krok 3: Korekta Zasady 7 (linia 210)**

**OLD (linia 210):**
```
   - `CatalogPageType2` i `CatalogPageType3` — `max-w-full sm:max-w-xl`.
```

**NEW:**
```
   - [`CatalogPagePlaceholder`](../src/components/catalog/CatalogPagePlaceholder.tsx#L30) (zaślepka dla layoutów `type2` / `type3` przewidzianych w schemacie [`src/types/catalog.ts`](../src/types/catalog.ts) dla przyszłych kolekcji) — `max-w-full sm:max-w-xl`.
```

- [ ] **Krok 4: Weryfikacja**

```bash
grep -n "CatalogPageType2\|CatalogPageType3" docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: 0 wystąpień.

- [ ] **Krok 5: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): replace dead CatalogPageType2/3 refs with actual placeholder path"
```

---

### Task B3: Aktualizacja numerów linii w globals.css

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (linie 104, 122)

- [ ] **Krok 1: Pobranie aktualnych numerów linii**

```bash
grep -n "^\.skip-link\|prefers-reduced-motion: reduce\|^:focus-visible" src/app/globals.css
```

Oczekiwane wyjście (na 2026-05-09):
```
438:.skip-link {
452:@media (prefers-reduced-motion: reduce) {
467::focus-visible {
```

- [ ] **Krok 2: Korekta odniesienia w Zasadzie 2 (linia 104)**

**OLD (linia 104):**
```
     - Globalna reguła CSS w [src/app/globals.css:367–379](../src/app/globals.css) skraca wszystkie animacje do 0,01 ms.
```

**NEW:**
```
     - Globalna reguła CSS w [src/app/globals.css:452-464](../src/app/globals.css#L452-L464) skraca wszystkie animacje do 0,01 ms.
```

- [ ] **Krok 3: Korekta odniesienia w Zasadzie 3 (linia 122)**

**OLD (linia 122):**
```
- **Skip link** umożliwiający pominięcie nawigacji i przeskok do treści głównej (WCAG 2.4.1 Bypass Blocks). Implementacja: [src/app/globals.css:352–360](../src/app/globals.css), użycie: [src/layouts/qx/CatalogPageQX.tsx:39–41](../src/layouts/qx/CatalogPageQX.tsx).
```

**NEW:**
```
- **Skip link** umożliwiający pominięcie nawigacji i przeskok do treści głównej (WCAG 2.4.1 Bypass Blocks). Implementacja: [src/app/globals.css:438-445](../src/app/globals.css#L438-L445), użycie: [src/layouts/qx/CatalogPageQX.tsx](../src/layouts/qx/CatalogPageQX.tsx).
```

- [ ] **Krok 4: Weryfikacja**

```bash
grep -n "globals.css:" docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: numery linii 438, 452, ewentualnie 467 dla `:focus-visible` (jeśli cytowane w innych miejscach raportu).

- [ ] **Krok 5: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): refresh globals.css line refs (skip link, prefers-reduced-motion)"
```

---

### Task B4: Aktualizacja sekcji 5 (Podsumowanie weryfikacji technicznej)

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (linie 299–306)

- [ ] **Krok 1: Wymiana całej tabeli**

**OLD (linie 299–306):**
```
| Wskaźnik | Wartość |
| --- | --- |
| Zidentyfikowanych ustaleń z audytu | 27 (5 K, 8 P, 9 U, 5 D) |
| Wdrożonych poprawek (commits) | 28 |
| Testy automatyczne (vitest + jest-axe) | 44 zaliczonych / 1 pominięty (powód niezwiązany z dostępnością) / 0 niezaliczonych |
| Sprawdzenie typów (TypeScript) | 0 błędów |
| Build produkcyjny (Next.js) | 10/10 stron statycznych wygenerowanych poprawnie |
| Pokrycie zasad uniwersalnego projektowania (10 zasad) | 10/10 spełnionych |
```

**NEW:**
```
| Wskaźnik | Wartość | Stan na |
| --- | --- | --- |
| Zidentyfikowanych ustaleń z audytu | 27 (5 K, 8 P, 9 U, 5 D) | 2026-05-07 |
| Wdrożonych poprawek (per zadanie T0.1–T5.1) | 28 | 2026-05-07 |
| Testy automatyczne (vitest + jest-axe) | 82 zaliczone / 1 pominięty (`overview-min-size.test.ts`, powód niezwiązany z a11y) / 0 niezaliczonych | 2026-05-09 |
| Sprawdzenie typów (TypeScript) | 0 błędów (`tsc --noEmit`) | 2026-05-09 |
| Build produkcyjny (Next.js 15.5.12) | 22/22 trasy wygenerowane poprawnie (5 prerendered + 2 SSG + 15 dynamicznych) | 2026-05-09 |
| Pokrycie zasad uniwersalnego projektowania (10 zasad) | 10/10 spełnionych w warstwie kodu | 2026-05-07 |
| Manualna weryfikacja a11y (Lighthouse + axe + AT + reflow) | _zob. [docs/raport-dostepnosci-weryfikacja-manualna.md](./raport-dostepnosci-weryfikacja-manualna.md)_ | _data wykonania Task A_ |
```

- [ ] **Krok 2: Weryfikacja**

```bash
grep -n "44 zaliczonych\|10/10 stron" docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: 0 wystąpień.

- [ ] **Krok 3: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): refresh summary metrics table with current build/test counts"
```

---

### Task B5: Aktualizacja sekcji 7 (Załączniki)

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (linie 322–329)

- [ ] **Krok 1: Wymiana listy załączników**

**OLD (linie 322–329):**
```
- **Załącznik A.** Audyt dostępności frontendu — [`.ui-design/audits/metro_catalogs_zasady_20260507_115012.md`](../.ui-design/audits/metro_catalogs_zasady_20260507_115012.md)
- **Załącznik B.** Plan implementacji napraw — [`docs/superpowers/plans/2026-05-07-accessibility-wcag-aa-remediation.md`](./superpowers/plans/2026-05-07-accessibility-wcag-aa-remediation.md)
- **Załącznik C.** Rejestr postępu i checklista weryfikacji manualnej — [`docs/superpowers/plans/2026-05-07-accessibility-progress.md`](./superpowers/plans/2026-05-07-accessibility-progress.md)
- **Załącznik D.** Zasady projektowe (dokument źródłowy) — [`docs/zasady.md`](./zasady.md)
- **Załącznik E.** Dokumentacja wzorców a11y na żywo — strona `/design-system#a11y-patterns` w aplikacji
- **Załącznik F.** Pełny rejestr 28 zmian (`git log f6508ab..HEAD --oneline`) na gałęzi `audyt_fix`
```

**NEW:**
```
- **Załącznik A.** Audyt dostępności frontendu — [`.ui-design/audits/metro_catalogs_zasady_20260507_115012.md`](../.ui-design/audits/metro_catalogs_zasady_20260507_115012.md)
- **Załącznik B.** Plan implementacji napraw — [`docs/superpowers/plans/2026-05-07-accessibility-wcag-aa-remediation.md`](./superpowers/plans/2026-05-07-accessibility-wcag-aa-remediation.md)
- **Załącznik C.** Rejestr postępu i mapowanie zadań T0.1–T5.1 na pierwotne SHA — [`docs/superpowers/plans/2026-05-07-accessibility-progress.md`](./superpowers/plans/2026-05-07-accessibility-progress.md)
- **Załącznik D.** Zasady projektowe (dokument źródłowy) — [`docs/zasady.md`](./zasady.md)
- **Załącznik E.** Dokumentacja wzorców a11y na żywo — strona `/design-system#a11y-patterns` w aplikacji
- **Załącznik F.** Raport z manualnej weryfikacji a11y (Lighthouse, axe DevTools, klawiatura, czytnik ekranu, reflow) — [`docs/raport-dostepnosci-weryfikacja-manualna.md`](./raport-dostepnosci-weryfikacja-manualna.md)
- **Załącznik G.** Plan poprawek doprowadzający audyt do kompletności — [`docs/plan_poprawek.md`](./plan_poprawek.md)
```

- [ ] **Krok 2: Weryfikacja**

```bash
grep -n "git log f6508ab\|audyt_fix" docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: tylko jedno wystąpienie `audyt_fix` (z Task B1, kontekst historyczny w nagłówku); zero wystąpień `git log f6508ab`.

- [ ] **Krok 3: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): update annex list — replace unreproducible git log with manual verification"
```

---

## Faza C — Finalizacja audytu

---

### Task C1: Aneks z ostatecznym werdyktem

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (dopisz po linii 339)

- [ ] **Krok 1: Dopisanie aneksu na końcu raportu**

Po linii 339 dopisz:

```markdown

---

## 8. Aneks weryfikacyjny (2026-05-09 i wykonanie manualne)

### 8.1 Re-walidacja automatyczna 2026-05-09

W ramach domknięcia audytu zespół wykonał ponowną walidację automatyczną na obecnym repo (`__METRO_catalogs_stage2`, branch `main`):

- `npm run typecheck` → **0 błędów**
- `npm run test` → **82 passed / 1 skipped / 0 failed** (20 plików testowych, czas 4.86 s)
- `npm run build` → **22/22 trasy wygenerowane**, 0 ostrzeżeń krytycznych

Wszystkie deklarowane w sekcjach 4.1–4.10 mechanizmy a11y zostały odnalezione w drzewie roboczym i działają poprawnie. Pełna lista weryfikowanych elementów:

- `useFocusTrap` używany w 3 modalach (Lightbox, FinishesQX preview, PackshotsQX lightbox)
- ARIA na komponentach (`role="dialog"`, `aria-modal="true"`, `aria-labelledby`, `aria-current="location"`, `aria-live="polite"`, `aria-pressed`)
- Skip link i `prefers-reduced-motion` w `src/app/globals.css` (linie 438–445 i 452–464)
- Touch targets ≥44 px (ColorChip, Lightbox, HeroQX, GalleryQX)
- Reflow 320 px (`MaterialsQX`, `CatalogPagePlaceholder`)
- `<html lang="en">` na poziomie root layoutu
- `<h2 class="section_ID">` na stronie startowej (3 wystąpienia)

### 8.2 Weryfikacja manualna

Manualna weryfikacja w przeglądarce (Lighthouse, axe DevTools, klawiatura, czytnik ekranu VoiceOver, reflow 320 px / zoom 200%) została wykonana zgodnie z planem [docs/plan_poprawek.md](./plan_poprawek.md) (Faza A). Pełny log wraz z screenshotami: [docs/raport-dostepnosci-weryfikacja-manualna.md](./raport-dostepnosci-weryfikacja-manualna.md).

**Werdykt manualny:** _do uzupełnienia po Task A6 z planu poprawek (PASS / FAIL z listą blokerów)._

---

**Sporządził aneks:** Zespół wdrożeniowy METRO Catalogs
**Data aneksu:** 2026-05-09
```

- [ ] **Krok 2: Weryfikacja kompletności raportu**

```bash
wc -l docs/raport-dostepnosci-2026-05-07.md
```

Oczekiwane: ~370–380 linii (oryginalnie 339 + ~30–40 linii aneksu).

- [ ] **Krok 3: Commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): add verification annex (sec. 8) with 2026-05-09 re-validation results"
```

---

### Task C2: Domknięcie werdyktu manualnego

**Pliki:**
- Modyfikuj: `docs/raport-dostepnosci-2026-05-07.md` (sekcja 8.2 — placeholder werdyktu)
- Wymaga ukończenia: Task A6

- [ ] **Krok 1: Pobranie werdyktu z logu manualnego**

Otwórz [docs/raport-dostepnosci-weryfikacja-manualna.md](docs/raport-dostepnosci-weryfikacja-manualna.md) i sprawdź sekcję F (Wnioski → ostateczny werdykt).

- [ ] **Krok 2: Wymiana placeholdera w raporcie**

W `docs/raport-dostepnosci-2026-05-07.md`, w sekcji 8.2:

**OLD:**
```
**Werdykt manualny:** _do uzupełnienia po Task A6 z planu poprawek (PASS / FAIL z listą blokerów)._
```

**NEW (wariant PASS, jeśli werdykt = PASS):**
```
**Werdykt manualny:** ✅ **PASS.** Wszystkie 3 strony reprezentatywne osiągnęły Lighthouse Accessibility ≥95 i 0 violations w axe DevTools. Klawiatura w pełni funkcjonalna we wszystkich 10 testowanych scenariuszach. VoiceOver poprawnie ogłasza nagłówki, landmarki, modale i stany interaktywne. Reflow przy 320 px i zoom 200% bez utraty treści. Audyt zamknięty zgodnością WCAG 2.1 AA.
```

**NEW (wariant FAIL, jeśli werdykt = FAIL):**
```
**Werdykt manualny:** ⚠️ **FAIL** w obecnym stanie. Zidentyfikowane blokery (do osobnego planu naprawczego):
- _lista 1_
- _lista 2_
Ostateczna deklaracja zgodności WCAG 2.1 AA wymaga ich domknięcia.
```

(Wybierz właściwy wariant na podstawie Kroku 1.)

- [ ] **Krok 3: Final commit**

```bash
git add docs/raport-dostepnosci-2026-05-07.md
git commit -m "docs(a11y): close manual verification verdict in main report"
```

---

### Task C3: Final review checklist

**Pliki:**
- Tylko odczyt — przegląd całości

- [ ] **Krok 1: Odczyt raportu od początku do końca**

Przeczytaj cały `docs/raport-dostepnosci-2026-05-07.md`. Sprawdź:
- Wszystkie linki działają (Cmd+kliknięcie w VS Code).
- Brak wystąpień `27d1578`, `f6508ab`, `CatalogPageType2`, `CatalogPageType3`.
- Numery linii w cytatach `globals.css` zgodne z faktycznym stanem.
- Liczby w tabeli sekcji 5 zgodne z aneksem 8.1.

- [ ] **Krok 2: Odczyt logu manualnego**

Przeczytaj `docs/raport-dostepnosci-weryfikacja-manualna.md`. Sprawdź:
- Wszystkie sekcje A–F wypełnione.
- Screenshoty w `docs/screenshots/lighthouse/` istnieją i są referencjonowane.
- Werdykt zgodny z tym, co trafiło do sekcji 8.2 raportu głównego.

- [ ] **Krok 3: Sanity check automatyczny**

```bash
npm run typecheck && npm run test && npm run build 2>&1 | tail -5
```

Oczekiwane: typecheck 0 errors, test 82 passed, build 22/22 routes.

- [ ] **Krok 4: Commit „audyt zamknięty"**

```bash
git add -A docs/
git commit --allow-empty -m "docs(a11y): audit complete — main report + manual verification + closure" || echo "nothing to commit, audit already finalized"
```

---

## Self-review (wykonana przez autora planu)

**Pokrycie spec:**
- ✅ Manualna weryfikacja a11y (T5.2 z `2026-05-07-accessibility-progress.md`) → Faza A
- ✅ Korekta git refs (audyt_fix, SHA) → Task B1
- ✅ Korekta CatalogPageType2/3 → Task B2
- ✅ Korekta numerów linii globals.css → Task B3
- ✅ Korekta liczników 44/10 → Task B1 + Task B4
- ✅ Aneks weryfikacyjny → Task C1
- ✅ Werdykt finalny → Task C2
- ✅ Final review → Task C3

**Skan placeholderów:** Każdy placeholder w `_TBD_` (sekcja A1) jest świadomie pozostawiony jako pole do wypełnienia podczas wykonania manualnego — to jedyny dopuszczalny placeholder, ponieważ wartości zależą od pomiarów wykonywanych w przeglądarce. Wszystkie kroki implementacyjne mają konkretną treść (kod, komendę, dokładną zamianę OLD/NEW).

**Spójność typów / nazw:**
- Plik weryfikacyjny zawsze cytowany jako `docs/raport-dostepnosci-weryfikacja-manualna.md` (Task A1, B5, C1, C2, C3 — spójnie).
- Plan poprawek cytowany jako `docs/plan_poprawek.md` (Task B5, C1 — spójnie).
- Numery linii globals.css: 438–445 (skip link), 452–464 (reduced-motion) — spójnie w Task B3, C1.
- Liczniki: 82 testów, 22 trasy — spójnie w Task B1, B4, C1.

---

**Plan kompletny i zapisany do `docs/plan_poprawek.md`. Dwie opcje wykonania:**

**1. Subagent-Driven (rekomendowane)** — dispatching świeży subagent per zadanie, review między zadaniami, szybka iteracja.

**2. Inline Execution** — wykonanie w obecnej sesji z `superpowers:executing-plans`, batch z checkpointami.

**Które podejście?**
