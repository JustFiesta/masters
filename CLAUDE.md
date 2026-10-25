# Projekt i wskazówki dla Claude Code

@.claude/starting-prompt.md

## Kontekst

Repozytorium służy pracy magisterskiej na temat prototypu silnika przydziału kandydatów do szkół ponadpodstawowych. Powyższy import dołącza szczegółowy, roboczy kontekst z pliku [`.claude/starting-prompt.md`](.claude/starting-prompt.md) do instrukcji ładowanych na starcie sesji Claude Code.

Na obecnym etapie repozytorium nie zawiera jeszcze implementacji ani ustalonego stosu technologicznego. Zakres pracy, formalne zasady przydziału i mechanizm referencyjny pozostają do uzgodnienia. Nie zakładaj ich samodzielnie.

## Zakres i priorytety

- Skupiaj się na algorytmach przydziału, ich poprawności, mierzalnych kryteriach sprawiedliwości, efektywności oraz skalowalności.
- Traktuj prototyp jako narzędzie do powtarzalnych eksperymentów na danych syntetycznych; UI i pełny system produkcyjny nie są celem, chyba że użytkownik wyraźnie zmieni zakres.
- Proponowane algorytmy (np. DA, TTC, EADA lub optymalizacja) są kierunkami do oceny, nie wymaganiami implementacyjnymi ani ustalonym wyborem.
- Nie zakładaj, że mechanizm systemu referencyjnego to Deferred Acceptance ani że obserwowany niekorzystny wynik oznacza błąd systemu.
- Nie utożsamiaj sprawiedliwości z samym wynikiem punktowym. Rozdzielaj preferencje kandydatów, formalne priorytety, reguły kwalifikacji i wybrane metryki.
- Nie przedstawiaj metody jako lepszej bez wyników eksperymentów i jawnie opisanych ograniczeń.

## Sposób pracy

- Pisz po polsku, chyba że użytkownik poprosi inaczej. Wyjaśniaj pojęcia techniczne przystępnie i na przykładach.
- Oddzielaj fakty potwierdzone od założeń, hipotez i niewiadomych. Przy researchu korzystaj z publikacji pierwotnych oraz oficjalnych, aktualnych źródeł; podawaj odniesienia.
- Przed zmianami sprawdzaj aktualny stan repozytorium i istniejące wzorce. Nie wymyślaj języka, architektury, reguł rekrutacji ani poleceń build/test.
- Gdy rozpocznie się implementacja, preferuj proste, powtarzalne rozwiązania, testy na małych ręcznie weryfikowalnych przykładach oraz pomiary z podanym sposobem odtworzenia.
- Nie twórz jeszcze pytań ani hipotez badawczych, projektu UI lub pełnego projektu systemu produkcyjnego, chyba że użytkownik o to poprosi.
- Przy nieustalonych decyzjach merytorycznych przedstawiaj konsekwencje i pytaj, zamiast utrwalać arbitralne założenia.
