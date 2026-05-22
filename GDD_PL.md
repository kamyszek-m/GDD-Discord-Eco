# Dokument Projektowy Gry (GDD) - Wersja Polska
## Projekt: HearthGuild Village (nazwa robocza)
## Gatunek: Przytulna symulacja ekonomii wioski (Discord-native)
## Odbiorcy: Społeczności furry fandom na Discordzie
## Rewizja: v2.0 (PL)

---

## 1. Podsumowanie wykonawcze

Społecznościowa symulacja ekonomiczna działająca natywnie na Discordzie. Gracz nie "farmi pieniędzy" - staje się ważnym członkiem żywej wioski. System unika typowych problemów botów ekonomicznych (spam komend, nieskończone źródła waluty, dominacja najbogatszych, ekonomia kasynowa).

Kryteria sukcesu:

- Gracze wracają dla poczucia przynależności i współtworzenia.
- Waluta jest stabilna, ograniczona i służy użyteczności.
- Profesje są współzależne i społecznie istotne.
- Rynek reaguje na podaż, popyt, sezony i wydarzenia.
- Weterani utrzymują zaangażowanie przez mistrzostwo i opiekę nad społecznością, nie przez akumulację majątku.

---

## 2. Filozofia projektu

### 2.1 North Star

"Nie grasz o monety. Pomagasz swojej wiosce prosperować."

### 2.2 Filary

1. Współzależność społeczna
- Żadna profesja nie jest samowystarczalna.
- Każdy zawód tworzy dobra potrzebne innym.
- Kontrakty i projekty wspólne budują regularne interakcje.

2. Przytulność ponad przymus
- Postęp jest spokojny, planowany i bez presji spamu.
- Mechaniki idle/asynchroniczne respektują codzienny rytm graczy.
- Brak "cooldown-clickera" jako głównej pętli.

3. Ekonomia jako symulacja
- Pieniądz pojawia się przez realną kreację wartości.
- Niedobór, sezonowość i popyt wpływają na ceny.
- Inflacja jest monitorowana i automatycznie korygowana.

4. Reputacja ponad bogactwo
- Zaufanie i wkład społeczny otwierają możliwości.
- Sam majątek nie daje trwałej przewagi politycznej ani społecznej.

5. Żywa, ewoluująca wioska
- Infrastruktura, festiwale i zdarzenia zmieniają priorytety.
- Świat ma odczuwalny rytm i konsekwencje działań graczy.

### 2.3 Antywzorce (świadomie odrzucone)

- Nieskończone "faucety" waluty z jednej komendy.
- Model kasynowy jako główne źródło retencji.
- Statyczne ceny i martwy rynek.
- Trwałe monopole starych graczy.
- Postęp oparty wyłącznie na rosnącym saldzie.

---

## 3. Fantazja gracza i cele emocjonalne

### 3.1 Docelowe emocje

- Ciepło: wioska ma być bezpieczna i serdeczna.
- Duma: praca gracza realnie pomaga innym.
- Uznanie: wiarygodność i reputacja są widoczne.
- Ciekawość: rynek i sezony stale otwierają nowe możliwości.
- Przynależność: wydarzenia wspólne tworzą poczucie "naszej" wioski.

### 3.2 Archetypy graczy

1. Rzemieślnik: kocha produkcję, jakość i łańcuchy wartości.
2. Gospodarz: rozwija dom, organizuje spotkania i życie społeczne.
3. Strateg: analizuje ceny, logistykę i opłacalne specjalizacje.
4. Opiekun: wzmacnia projekty wspólnoty i reaguje na kryzysy.
5. Opowiadacz: buduje klimat RP, kulturę i narrację społeczności.

---

## 4. Główna pętla rozgrywki

1. Obserwacja stanu wioski
- Tablica rynku, kontrakty, projekty społeczne, sezon.

2. Plan pracy
- Wybór aktywności profesji wg popytu, zasobów i celów.

3. Produkcja i handel
- Zbieranie/przetwarzanie/wytwarzanie dóbr.
- Realizacja kontraktów graczy i NPC.
- Sprzedaż na rynku lub handel bezpośredni.

4. Reinvestycja i wkład
- Ulepszenia narzędzi, domu i warsztatu.
- Wsparcie projektów wspólnych materiałami i pracą.

5. Wzrost zaufania i reputacji
- Terminowość i jakość budują długofalową wiarygodność.

6. Reakcja na zmiany świata
- Sezony, festiwale i zdarzenia zmieniają potrzeby rynku.

Rezultat pętli: tożsamość gracza oraz rozwój społeczności, nie tylko wzrost pieniędzy.

---

## 5. Pętle poboczne

### 5.1 Pętla mistrzostwa profesji
Praca -> doświadczenie -> specjalizacja -> wyższa jakość -> większy popyt -> wzrost reputacji.

### 5.2 Pętla domostwa
Ulepszanie domu -> bonusy użytkowe i społeczne -> więcej odwiedzin/kontraktów -> dalsza personalizacja.

### 5.3 Pętla zaufania
Rzetelne realizacje -> rekomendacje -> niższe koszty i lepsze kontrakty -> większa odpowiedzialność.

### 5.4 Pętla wspólnotowa
Wkład w projekty -> odblokowane premie globalne -> lepsza produktywność całej wioski.

---

## 6. Model progresji

### 6.1 Onboarding nowego gracza (NPE)

Faza A: Przyjazd (Dzień 0-1)
- Wybór tożsamości i ścieżki startowej profesji.
- Łańcuch samouczka: produkcja, handel, zaufanie.
- Pierwszy mini-projekt kooperacyjny.

Faza B: Pierwszy wkład (Dzień 2-4)
- Pierwszy kontrakt z realnym odbiorcą.
- Podstawowa personalizacja domu.
- Pierwszy tytuł reputacyjny: Pomocny Mieszkaniec.

Faza C: Integracja (Tydzień 1)
- Wybór specjalizacji.
- Udział w wydarzeniu sezonowym.
- Pierwsze stałe relacje handlowe/społeczne.

### 6.2 Mid-game
- Złożone zależności między profesjami.
- Optymalizacja wokół roli i jakości, nie wokół samego kapitału.
- Kontrakty i projekty społeczne jako główny motor postępu.

### 6.3 Retencja weteranów

Weterani rozwijają się przez:

- poziomy "masterwork" jakości,
- mentoring nowych graczy,
- role organizacyjne (festiwale, kryzysy),
- dziedzictwo (trwałe wkłady infrastrukturalne/kulturowe).

Brak trwałej dominacji:
- Część przywilejów zależy od bieżącej wiarygodności (okna 30/60/90 dni), a nie tylko historii bogactwa.

### 6.4 Retencja emocjonalna

Dlaczego gracze wracają:

- Ich rola społeczna ma znaczenie.
- Stan wioski zmienił się od ostatniej wizyty.
- Trwają wspólne cele wymagające ich profesji.
- Sezony i wydarzenia zmieniają opłacalne decyzje.
- Dom i relacje tworzą osobisty kontekst gry.

---

## 7. Profesje i ekonomia pracy

### 7.1 Lista profesji (startowa)

- Rolnik
- Zielarz
- Piekarz
- Krawiec
- Rybak
- Cieśla
- Karczmarz
- Kupiec
- Artysta
- Kurier
- Kowal
- Opowiadacz

### 7.2 Przykłady łańcuchów produkcyjnych

Łańcuch żywności i gościnności:
- Rolnik -> Pszenica
- Młynarz (gałąź piekarza) -> Mąka
- Piekarz -> Chleb
- Karczmarz -> Posiłki/Buffy gościnności

Łańcuch odzieżowy:
- Rolnik/Zielarz -> Włókna i barwniki
- Krawiec -> Ubiory
- Kupiec -> Pakiety festiwalowe i wymiana regionalna

Łańcuch narzędzi:
- Dostawy surowców -> Kowal/Cieśla -> Narzędzia i stanowiska
- Pozostałe profesje -> wzrost wydajności i jakości

### 7.3 Progresja profesji

Każda profesja zawiera:

- Rangi: Nowicjusz -> Wykwalifikowany -> Ekspert -> Mistrz -> Renomowany
- Rozgałęzienie specjalizacji od poziomu "Wykwalifikowany".
- Ścieżki mistrzostwa: Wydajność, Jakość, Służba Społeczna.

### 7.4 Model czasu pracy (Labor Points)

Akcje zużywają punkty pracy (LP), regenerowane pasywnie.

Cele modelu LP:
- ograniczenie spamu bez frustrujących cooldownów,
- promowanie planowania i współpracy,
- wspieranie stylu idle/asynchronicznego.

---

## 8. Projekt ekonomii

### 8.1 Zasady waluty

- Jedna główna waluta lokalna (czytelność i balans).
- Ograniczone wejścia pieniądza, powiązane z wartością.
- Silne ujścia (sinki) związane z użytkowością i utrzymaniem.

### 8.2 Dozwolone źródła napływu waluty

1. Kontrakty NPC reprezentujące popyt zewnętrzny.
2. Nagrody za ukończone projekty wspólnotowe.
3. Budżetowane stypendia/event payouts (z limitem).

Brak komend generujących nieskończoną gotówkę.

### 8.3 Ujścia waluty (money sinks)

- Opłaty rynkowe i podatki transakcyjne.
- Utrzymanie domu i narzędzi.
- Koszty logistyczne i premium transport.
- Wydatki luksusowe/kosmetyczne.
- Licencje profesyjne i certyfikacje specjalizacji.

### 8.4 Guardrails podaży pieniądza

Po przekroczeniu bezpiecznych progów aktywuje się dynamiczny mnożnik sinków:
- wzrost opłat w przegrzanych segmentach,
- korekta wypłat NPC dla nadpodaży,
- pojawianie się atrakcyjnych wydatków użytkowych/luksusowych.

---

## 9. Dynamiczny system rynku

### 9.1 Komponenty rynku

1. Lokalny rynek graczy
- Oferty stałe i aukcyjne.

2. Tablica kontraktów
- Zlecenia NPC i graczy z terminami i wymaganiami jakości.

3. Wymiana sezonowa
- Czasowe niedobory i skoki popytu.

4. Przytulny Nocny Bazar (opcjonalny "black market")
- RP-friendly, dostęp reputacyjny, wyższe ryzyko/opłaty.

### 9.2 Czynniki ceny

Cena odniesienia uwzględnia:
- koszt bazowy produkcji,
- średnią ważoną historycznych transakcji,
- wskaźniki podaży i popytu,
- sezon i wydarzenia,
- mnożnik niedoboru,
- tłumienie zmienności.

### 9.3 Stabilizacja cen

- EWMA na oknach historycznych.
- Circuit breakers (limity skoku ceny per cykl).
- Minimalne/maksymalne pułapy kategorii.
- Filtry anomalii (m.in. self-trade ringi).

### 9.4 Ruchy podaży i popytu

Niedobór rośnie przez:
- niskie stany magazynowe,
- przerwane łańcuchy dostaw,
- ograniczenia sezonowe,
- nadzwyczajną konsumpcję eventową.

Popyt rośnie przez:
- festiwale,
- sytuacje kryzysowe,
- nowe receptury/ulepszenia,
- trendy społeczne i reputacyjne.

---

## 10. Prewencja inflacji i samokorekta

### 10.1 Kluczowe wskaźniki

1. Money Supply Ratio (MSR)
MSR = całkowita płynna waluta / wartość aktywnych dóbr

2. Inflation Index (II)
II = (CPI_bieżące - CPI_bazowe) / CPI_bazowe

3. Scarcity Score (SS)
SS_item = stock_docelowy / max(stock_obserwowany, 1)

4. Economic Health Index (EHI)
Złożony wynik z inflacji, zmienności, nierówności i pokrycia zapasów.

### 10.2 Automatyczna logika stabilizacji

Gdy II i MSR przekroczą progi:
- stopniowy wzrost opłat,
- obniżanie payoutów NPC dla nadpodaży,
- aktywacja dodatkowych sinków,
- uruchamianie projektów pochłaniających nadmiar waluty.

Gdy gospodarka jest zbyt "ciasna" (ryzyko deflacji):
- obniżanie opłat,
- wzrost kontraktów popytowych,
- subsydia dla profesji wąskich gardeł,
- ulgi utrzymaniowe dla nowych graczy.

### 10.3 Wygładzanie antyszokowe

- Zmiany parametrów są małe i limitowane per cykl.
- Oparte na trendach 7d/30d, nie jednorazowych pikach.
- Twarde limity tempa zmian chronią przed "oscylacją" systemu.

---

## 11. Reputacja i zaufanie

### 11.1 Ścieżki reputacji

- Pomocny Mieszkaniec
- Zaufany Handlarz
- Przytulny Gospodarz
- Wykwalifikowany Rzemieślnik
- Organizator Festiwali

Wpływ reputacji:
- dostęp do kontraktów,
- zniżki/opłaty,
- specjalne interakcje NPC,
- możliwości społeczne.

### 11.2 Mechanika zaufania

Zaufanie rośnie przez:
- terminową realizację,
- stabilną jakość,
- rekomendacje,
- niski poziom sporów.

Zaufanie maleje przez:
- niedostarczanie zleceń,
- powtarzalne anulacje,
- oszustwa i nadużycia.

Wpływ zaufania:
- progi podatkowe,
- wymogi escrow,
- limity wielkości transakcji,
- dostęp do rzadkich okazji.

### 11.3 Integralność systemu

- Waga ocen zależy od wiarygodności oceniającego.
- Starsze opinie naturalnie tracą siłę.
- Ochrona przed brigadingiem i ringami rekomendacji.

---

## 12. Domostwa i personalizacja

### 12.1 Typy domów

- Nora
- Borsucza jama
- Domek na drzewie
- Chata
- Loft warsztatowy

### 12.2 Funkcja gameplayowa

Dom zapewnia:
- większy magazyn,
- sloty stanowisk profesyjnych,
- bonusy komfortu,
- pojemność gościnną.

### 12.3 Hosting społeczny

Możliwe aktywności:
- herbatki i wieczory rozmów,
- kręgi rzemieślnicze,
- noce opowieści,
- mini-jarmarki.

Korzyści:
- reputacja społeczna,
- punkty współpracy eventowej,
- okazjonalne nowe kontrakty.

### 12.4 Utrzymanie i anty-hoarding

- Większe domy mają wyższe koszty utrzymania.
- Nieużywane luksusy dają malejące korzyści.
- System premiuje aktywne wykorzystanie, nie bierne gromadzenie.

---

## 13. Systemy społecznościowe i wydarzenia

### 13.1 Infrastruktura wspólna

- Szklarnia
- Karczma
- Ogrody publiczne
- Hala warsztatowa
- Poczta kurierska

Każdy obiekt ma poziomy rozwoju wymagające współpracy.

### 13.2 Przykładowe wydarzenia flagowe

1. Święto Zbiorów
- wzrost popytu na żywność,
- przygotowania wspólnotowe,
- konkursy dekoracyjne.

2. Zimowy Festiwal Latarni
- większy popyt na rzemiosło i sztukę,
- mechaniki "ciepła" społecznego.

3. Wizyta Wędrownego Kupca
- okazje importowo-eksportowe,
- strategiczne zarządzanie zapasami.

4. Nagły Kryzys Wioski
- szok podaży wymagający współdziałania profesji.

5. Przytulne Noce Opowieści
- wydarzenia RP, rola opowiadacza i gospodarzy.

### 13.3 Zasady projektowania eventów

- Każdy event zawiera cel kooperacyjny.
- Każdy event daje co najmniej jedną nagrodę niematerialną.
- Eventy nie mogą być "drukarką" surowej waluty.

### 13.4 Sklep benefitow serwerowych (role i perki)

Cel projektowy:
- Umozliwic graczom zamiane wkladu w zycie wioski na benefity Discorda bez psucia balansu ekonomii.

Kategorie benefitow:
- Role kosmetyczne (kolor/nazwa/tytul, bez przewagi mocy).
- Perki spoleczne (odznaki profilu, priorytet wyroznienia gospodarza wydarzen).
- Perki dostepu (kanaly lounge festiwalowe, strefa prezentacji tworczosci).
- Czasowe perki wygody (wiekszy limit aktywnych ofert, dodatkowy slot dekoracji domu).

Zasady balansu:
- Brak pay-to-win: sklep nie daje bezposrednich mnoznikow produkcji.
- Mocniejsze perki sa czasowe (np. 14/30 dni), nie permanentne.
- Zaawansowane benefity wymagaja progow zaufania/reputacji.
- Miesieczne soft capy na najlepsze perki.

Rola ekonomiczna:
- Zakupy rol/perkow stanowia istotny money sink.
- Ceny perkow moga rosnac dynamicznie przy presji inflacyjnej.
- Opcjonalny podzial wplywow: czesc trafia do skarbca projektow wspolnotowych.

Model pozyskania:
- Zakup bezposredni: oferta stala.
- Odblokowanie za zaslugi: prog reputacji + obnizona cena.
- Oferty sezonowe: limitowane kosmetyki i motywy wydarzen.

Integracja tozsamosci serwera:
- Benefity wzmacniaja klimat furry/community (np. Cozy Host, Caravan Guide, Lantern Curator).
- Asortyment sklepu konfigurowany przez administracje.

Ochrona przed dominacja:
- Benefity rozwijaja ekspresje i organizacje, nie surowa sile ekonomiczna.
- Nowi gracze maja szybki dostep do kosmetykow startowych.

Zarzadzanie administracyjne:
- Admin konfiguruje mapowanie ofert -> role Discord i limity posiadaczy.
- Przelacznik awaryjny do natychmiastowego wylaczenia sklepu.

---

## 14. Systemy anty-dominacji i anty-bogacz

1. Progresywne podatki od wysokiego miesięcznego obrotu.
2. Malejące zwroty z powtarzalnych pętli arbitrażu.
3. Dostęp oparty o reputację/zaufanie, nie wyłącznie majątek.
4. Rotacja top-kontraktów (antymonopol).
5. Koszt utrzymania nadmiernych, nieaktywnych zapasów.
6. Premie mentoringowe dla wspierania nowych graczy.
7. Miękki cap użyteczności bogactwa (nadwyżki -> prestiż/kosmetyka).

---

## 15. Reguły balansu

1. Reguła użyteczności
Nowe elementy muszą mieć wartość funkcjonalną lub społeczną.

2. Reguła współzależności
Żadna profesja nie domyka samodzielnie pełnego łańcucha.

3. Reguła źródło-ujście
Każde nowe źródło waluty wymaga zaprojektowanego ujścia.

4. Reguła ochrony nowych graczy
Wczesny postęp nie może być blokowany przez ceny rynkowe.

5. Reguła skończonych pętli
Każda dochodowa pętla musi mieć koszt czasu/zasobu/popytu.

6. Reguła stabilności eventowej
Event może wzmacniać popyt tymczasowo, nie permanentnie.

---

## 16. Anty-exploit (warstwa gameplay)

- Wykrywanie nienaturalnych transferów i zachowań alt-kont.
- Limity częstotliwości listowania/cancel-relist.
- Escrow dla niskiego zaufania.
- Analiza anomalii cenowych i wolumenu.
- Wykrywanie ringów rekomendacji/review bombingu.

---

## 17. Przykładowe ścieżki gracza

### 17.1 Nowy gracz: "Pine" (Rolnik)

- Dołącza i wybiera rolnika.
- Realizuje pierwszy kontrakt pszenicy.
- Sprzedaje plony piekarzowi.
- Otrzymuje pozytywną ocenę i wzrost zaufania.
- Wspiera szklarnię społecznościową.
- Odblokowuje tytuł Pomocny Mieszkaniec.

### 17.2 Gracz średniozaawansowany: "Moss" (Krawiec + Gospodarz)

- Szyje odzież na Festiwal Latarni.
- Organizuje spotkanie w chacie.
- Buduje reputację Gospodarza i Rzemieślnika.
- Otrzymuje dostęp do wartościowych kontraktów tekstylnych.

### 17.3 Weteran: "Rowan" (Karczmarz-opiekun)

- Koordynuje reakcję na kryzys zaopatrzenia.
- Mentoruje nowych piekarzy.
- Zyskuje status Organizatora Festiwali.
- Utrzymuje wpływ dzięki niezawodności, nie zasobności konta.

---

## 18. Zakres MVP i roadmap (perspektywa designu)

### 18.1 MVP (Faza 1)

- 6 profesji (Rolnik, Piekarz, Rybak, Krawiec, Cieśla, Karczmarz)
- Podstawowe łańcuchy produkcyjne
- Tablica kontraktów (NPC + gracz)
- Dynamiczne ceny v1
- Reputacja/zaufanie v1
- Domostwa v1 (2 typy)
- 1 event sezonowy (Święto Zbiorów)

### 18.2 Faza 2

- Pozostałe profesje
- Infrastruktura wspólnotowa
- System kryzysów i zdarzeń awaryjnych
- Rozszerzona arbitraż/reklamacje zaufania
- Analityka ekonomiczna dla społeczności i administracji

### 18.3 Faza 3

- Handel regionalny
- Przytulny Nocny Bazar
- Łańcuchy fabularne i narracyjne wydarzenia
- Kooperatywy/gildie rodzinne

---

## 19. Pomysły na dalszy rozwój

- Dzielnice biome (las, rzeka, wzgórza) z lokalnymi surowcami.
- Karawany między serwerami partnerskimi.
- Sygnowane wyroby mistrzowskie (unikalne, ale użyteczne).
- Kalendarz świąt kulturowych i sezonowe receptury.
- Głosowania społeczności nad priorytetami podatków i projektów.

---

## 20. Metryki sukcesu (KPI)

- Retencja 7/30 dni powiązana z aktywnością społeczną.
- Odsetek graczy biorących udział w projektach wspólnych.
- Mediana wzrostu zaufania/reputacji nowych graczy.
- Zmienność cen utrzymana w docelowym paśmie.
- Indeks różnorodności profesji (brak dominującej mety).
- Udział akcji społecznych względem czysto rynkowych.

Jeżeli KPI odbiegają od celu:
- korekta kontraktów,
- korekta sink/source,
- tuning produktywności profesji,
- zmiana częstotliwości i skali eventów.
