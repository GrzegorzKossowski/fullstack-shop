# Decyzje projektowe (log)

Źródło prawdy o **dlaczego** projekt wygląda tak jak wygląda. Aktualizuj
przy każdej nowej znaczącej decyzji — zwłaszcza gdy pracujesz z innym
narzędziem/kontem Claude albo wracasz po przerwie. Najnowsze na górze.

---

## Stack

- Next.js (App Router) + TypeScript + Tailwind CSS
- Drizzle ORM + Neon (Postgres serverless)
- Zod, schematy walidacji generowane z tabel Drizzle przez `drizzle-zod` —
  baza jest jedynym źródłem prawdy o kształcie danych
- Warstwa wizualna (Tailwind/shadcn) świadomie na dalszym planie —
  priorytetem jest działająca ścieżka end-to-end, nie UI

## Tryb pracy z Claude: mentoring, nie auto-implementacja

Cel projektu: nauka fullstacka w stronę roli senior developera / architekta
SaaS. Claude (w każdym narzędziu — czat, Claude Code) ma dyskutować,
tłumaczyć koncepcje i mówić **co** zrobić, nie pisać kodu za użytkownika,
dopóki ten wyraźnie nie utknie i nie poprosi o pomoc wprost. Szczegóły
reguł: `CLAUDE.md`.

## Kolejność budowy: cienki fundament + wertykalne slice'y

Odrzucone: czysto horyzontalne (cały model na start — ryzyko projektowania
rzeczy, których jeszcze nie potrzeba) i czysto wertykalne bez żadnego
fundamentu (przy znanej domenie sklepu to niepotrzebne ryzyko przebudowy
schematu w trakcie). Wybrano hybrydę: fundament = schemat core + połączenie
z DB, każda kolejna funkcja budowana pionowo (UI → API/server action → DB),
tak żeby po każdym kroku coś realnie działało.

Roadmapa: Fundament → Katalog produktów → Koszyk → Autoryzacja → Checkout
→ Płatność → Quasi-faktura PDF → (opcjonalnie) Panel admina.

## Produkty bez wariantów

Kategoria przykładowa: książki/gadżety. Bez wariantów (rozmiar/kolor) —
jeden wiersz `products` = jeden kupowalny item. Upraszcza model koszyka i
zamówień na czas budowy głównej ścieżki.

## Płatność: Stripe (test mode), nie Przelewy24

P24 wymaga konta produkcyjnego + weryfikacji dokumentów (1–3 dni robocze)
zanim powstanie środowisko sandbox — zbędne tarcie dla projektu demo.
Stripe: rejestracja e-mailem, klucze testowe od razu, ten sam realny
przepływ (Checkout Session + webhook). P24 zostaje jako możliwy krok
"integracja z lokalnym dostawcą" po tym, jak reszta już działa.

## Repo: prywatne

W przeciwieństwie do wcześniejszego rozważania (repo publiczne jako
portfolio) — ostatecznie: **prywatne**, żeby mieć dostęp z dwóch miejsc
(praca/dom) bez rozstrzygania na razie kwestii publikacji. Można
odpublicznić później, gdy projekt osiągnie stan, który chcesz pokazywać.