# Prompt startowy — praca magisterska

Ten dokument zawiera szczegółowy, roboczy kontekst pracy i może służyć jako prompt rozpoczynający nową rozmowę. Krótkie, stałe zasady dla Claude Code są w głównym pliku [`CLAUDE.md`](../CLAUDE.md). W razie rozbieżności pierwszeństwo mają najnowsze ustalenia użytkownika i promotora. To kontekst, nie polecenie do samodzielnego realizowania całego proponowanego planu — wykonuj zadanie określone w bieżącej prośbie.

Na dzień przygotowania tego kontekstu repozytorium nie zawiera jeszcze kodu prototypu. Język, stos technologiczny, dokładne zasady przydziału i mechanizm referencyjny nie są ustalone. Nie traktuj wymienionych dalej propozycji jako zatwierdzonego zakresu.

## 1. Temat pracy

**„Projekt i implementacja prototypu systemu elektronicznej rekrutacji do szkół średnich”**

Jest to temat narzucony przez promotora na Uniwersytecie Rzeszowskim. Opis tematu wskazuje na problem występujący w elektronicznych systemach rekrutacyjnych do szkół ponadpodstawowych, np. w Rzeszowie: zdarzają się sytuacje, w których uczeń nie dostaje się do preferowanej szkoły lub klasy, podczas gdy inny uczeń z niższymi wynikami rekrutacyjnymi otrzymuje miejsce w bardziej pożądanej placówce. Celem pracy jest zaprojektowanie, zaimplementowanie i przetestowanie prototypu rozwiązania ograniczającego takie przypadki, z uwzględnieniem obowiązujących zasad rekrutacji.

Temat będzie bardziej wyszczególniony i zawężony po rozmowie z promotorem. Jak dowiem się jak będzie dokładnie brzmiał dam znać.

## 2. Preferowany kierunek

Chcę podejść do tematu przede wszystkim **implementacyjnie, algorytmicznie i z naciskiem na skalowalność**, a nie teoretycznie czy projektowo od strony interfejsu.

Nie chcę tworzyć klona systemu VULCAN ani pełnego systemu produkcyjnego. Interesuje mnie prototyp silnika przydziału uczniów do szkół, który można porównać z mechanizmem bazowym i wykorzystać do sprawdzenia, czy inne algorytmy lub ich modyfikacje pozwalają uzyskać sprawiedliwsze wyniki przy zachowaniu porównywalnej lub lepszej efektywności.

Głównym celem jest znalezienie i sprawdzenie rozwiązania, które poprawia sprawiedliwość przydziału, a nie tylko stworzenie działającej aplikacji.

## 3. Założenia i ograniczenia

- Prototyp ma implementować algorytm przydziału, a nie pełny system rekrutacyjny.
- Interfejs graficzny (UI) nie jest potrzebny, chyba że promotor zdecyduje inaczej.
- Komunikacja może odbywać się przez API albo pliki wejściowe i wyjściowe.
- Dane będą syntetyczne, generowane na potrzeby eksperymentów.
- Zakres ma koncentrować się na algorytmach, ich poprawności, sprawiedliwości, efektywności i skalowalności.
- Funkcjonalności użytkownika i administratora, szczegółowe wymagania interfejsu oraz kwestie wdrożenia produkcyjnego nie są obecnie priorytetem.
- Ograniczenia organizacyjne i użytkowe wynikają w dużej mierze z istniejącego procesu rekrutacji oraz obowiązujących przepisów. Należy je uwzględnić w zakresie koniecznym do prawidłowego modelowania przydziału, ale nie powinny dominować pracy.
- Nie jestem matematykiem, dlatego modele formalne i algorytmy należy wyjaśniać przystępnie, stopniowo i na przykładach. W razie potrzeby można stosować bardziej formalne modele, po uzasadnieniu ich literaturą.
- Nie należy z góry zakładać, że rozwiązanie autorskie będzie lepsze. Jego przewaga musi wynikać z eksperymentów.

## 4. Obecne rozumienie systemu referencyjnego

Z mojej dotychczasowej wiedzy wynika, że w systemie takim jak VULCAN uczeń:

- wybiera szkoły i klasy, ustawiając je według własnych preferencji;
- uzyskuje punkty za wyniki egzaminu ósmoklasisty;
- uzyskuje punkty za oceny na świadectwie;
- może otrzymać dodatkowe punkty za świadectwo z wyróżnieniem;
- może otrzymać dodatkowe punkty za osiągnięcia w konkursach, w tym tytuły laureata lub finalisty, zgodnie z obowiązującymi zasadami;
- może uzyskać punkty za wolontariat, jeśli spełnia on wymagania określone w zasadach rekrutacji.

Powyższe elementy wpływają na wynik lub uprawnienia kandydata zgodnie z aktualnymi przepisami i zasadami rekrutacji. Dokładne wartości punktowe, warunki uznawania osiągnięć oraz ich wpływ na pierwszeństwo kandydatów należy zweryfikować w oficjalnych źródłach.

W prototypie należy rozważyć modelowanie tych elementów jako syntetycznych danych wejściowych, tak aby można było odtworzyć różne wyniki rekrutacyjne i sprawdzić, jak poszczególne algorytmy przydziału zachowują się w różnych scenariuszach.

Podejrzewam, że określony sposób przetwarzania preferencji i przydziałów może prowadzić do sytuacji, w których uczeń z lepszym wynikiem rekrutacyjnym otrzymuje mniej preferowaną szkołę niż uczeń z gorszym wynikiem. Nie znam jednak dokładnego mechanizmu wewnętrznego VULCAN-a ani nie wiem, czy wykorzystuje on algorytm Deferred Acceptance (DA).

**Nie traktuj tych przypuszczeń jako potwierdzonych faktów.** Należy najpierw zbadać publicznie dostępne zasady polskiej rekrutacji oraz dokumentację systemu referencyjnego. Trzeba odróżnić błędne działanie algorytmu od sytuacji, która jest prawidłowa według obowiązujących reguł, choć może wydawać się niesprawiedliwa.

## 5. Główne pojęcia i kierunki algorytmiczne

Problem należy do obszaru:
- School Choice;
- School Assignment;
- Matching Problem;
- przydziału zasobów przy ograniczonej pojemności.

Kluczowe jest pogodzenie sprawiedliwości i efektywności przydziału oraz mierzenie "sprawiedliwości" przydziału i stopnia uzasadnionej zazdrości.

Potencjalne kryteria oceny:
- **respektowanie priorytetów** — uwzględnianie pierwszeństwa kandydatów według zasad rekrutacji;
- **efektywność Pareto** — brak możliwości poprawienia przydziału jednego ucznia bez pogorszenia sytuacji innego;
- **spełnianie preferencji** — jak wysoko na liście preferencji znajduje się przydzielona szkoła;
- **brak uzasadnionej zazdrości (justified envy)** — czy istnieje uczeń, który wolałby miejsce innego ucznia i ma wyższy priorytet do tej szkoły;
- liczba przydzielonych miejsc, liczba nieprzydzielonych uczniów i rozkład wyników w różnych grupach kandydatów.

Nie należy utożsamiać sprawiedliwości z prostą zasadą „uczeń z lepszymi ocenami zawsze powinien dostać lepszą szkołę”. Trzeba odróżnić wynik punktowy, formalny priorytet rekrutacyjny, preferencje ucznia i ewentualny prestiż szkoły oraz pozostałe parametry rekrutacyjne (jeżeli obowiązują - laureaci, wolontariat itd. wymienione w pkt. 4).

### Algorytmy do rozważenia

1. **Deferred Acceptance (DA)** — klasyczny algorytm przydziału, który może posłużyć jako punkt odniesienia.
2. **Top Trading Cycles (TTC)** — alternatywny mechanizm, który warto porównać pod względem efektywności i respektowania priorytetów.
3. **Efficiency-Adjusted Deferred Acceptance (EADA)** — potencjalny kierunek poprawy efektywności przydziału przy ograniczaniu kompromisów dotyczących sprawiedliwości.
4. **Optymalizacja całego przydziału** — np. programowanie całkowitoliczbowe z funkcją celu minimalizującą koszt niespełnionych preferencji i ograniczeniami określającymi pojemność szkół oraz reguły priorytetów.
5. **Autorska modyfikacja lub hybryda** — dopiero po zrozumieniu wyników i ograniczeń istniejących metod.

To są propozycje do zbadania, a nie z góry wybrana lista algorytmów do implementacji. Należy wybrać rozsądny zakres, który da się zrealizować i obronić w pracy magisterskiej.

Ważne: metody mogą optymalizować różne kryteria i nie muszą spełniać wszystkich własności jednocześnie. Każde rozwiązanie trzeba oceniać w odniesieniu do jawnie określonych reguł. Reguły należy zdefiniować zawężając temat pracy do akceptowalnych przez promotora i mnie.

## 6. Proponowana architektura prototypu

Prototyp może być napisany w dowolnym języku wybranym przeze mnie - do rozważenia.

Proponowane moduły:

1. Generator danych syntetycznych — uczniowie, wyniki, preferencje, priorytety, szkoły i pojemności.
2. Model danych i walidacja poprawności wejścia.
3. Implementacja mechanizmu bazowego odzwierciedlającego ustalone zasady referencyjne.
4. Implementacje wybranych algorytmów przydziału.
5. Moduł oceny wyników: sprawiedliwość, efektywność, spełnienie preferencji i liczba przydzielonych miejsc.
6. Moduł pomiaru wydajności: czas wykonania, zużycie pamięci i zachowanie przy wzroście liczby uczniów i szkół.
7. Eksperymenty i raportowanie: wyniki CSV/JSON, tabele oraz wykresy.

UI nie jest wymagany. Wystarczą pliki wejściowe i wyjściowe albo opcjonalne proste API. Najważniejszy jest powtarzalny eksperyment, nie forma aplikacji.

## 7. Proponowany plan prac

1. Przegląd literatury o School Choice, School Assignment, DA, TTC, EADA, sprawiedliwości i efektywności przydziału.
2. Ustalenie zasad polskiej rekrutacji i określenie, jaki mechanizm można wiarygodnie przyjąć jako punkt odniesienia.
3. Zdefiniowanie mierzalnych kryteriów sprawiedliwości i efektywności.
4. Zaprojektowanie modelu danych i generatora syntetycznych scenariuszy.
5. Implementacja algorytmu bazowego i wybranych metod porównawczych.
6. Przygotowanie testów poprawności na małych, ręcznie sprawdzanych przykładach.
7. Przeprowadzenie eksperymentów na różnych scenariuszach i skalach danych.
8. Porównanie wyników, kosztu obliczeniowego i kompromisów pomiędzy kryteriami.
9. Sformułowanie wniosków: w jakich warunkach poszczególne metody działają najlepiej i czy proponowana modyfikacja daje mierzalną korzyść.

## 8. Wstępne źródła do weryfikacji

Czekam na info od promotora - można czegoś poszukać i zaproponować w międzyczasie.

Należy wyszukać także oryginalne publikacje o Deferred Acceptance, Top Trading Cycles, EADA oraz literaturę dotyczącą polskich zasad rekrutacji do szkół ponadpodstawowych. Nie należy przypisywać publikacjom wyników bez sprawdzenia ich pełnej treści.

## 9. Sposób dalszej współpracy

Na obecnym etapie chcę przede wszystkim poznać możliwe rozwiązania, zrozumieć literaturę i wybrać wykonalny kierunek implementacji.

Na razie nie przygotowuj:
- osobnej sekcji problemu badawczego;
- pytań badawczych;
- hipotezy badawczej;
- szczegółowego podziału wymagań na użytkownika i administratora;
- projektu UI;
- pełnego projektu systemu produkcyjnego.

Zamiast tego:
- wyjaśniaj pojęcia i algorytmy przystępnie, na przykładach;
- prowadź research oparty na publikacjach naukowych i wiarygodnych źródłach;
- oddzielaj potwierdzone informacje od hipotez i przypuszczeń;
- wskazuj ograniczenia każdego podejścia;
- pomagaj wybrać wykonalny zakres pracy magisterskiej;
- traktuj implementację, powtarzalne testy i mierzalne porównanie jako główny wkład pracy.

**Główna idea:** stworzyć prototyp silnika przydziału uczniów do szkół, porównać mechanizm referencyjny z wybranymi algorytmami oraz sprawdzić, czy można uzyskać sprawiedliwsze wyniki bez nieakceptowalnego pogorszenia efektywności i skalowalności.
