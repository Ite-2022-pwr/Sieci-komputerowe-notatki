# Przełączanie

**Przydatne Linki**
- [NetworkChuck - What is a SWITCH? // FREE CCNA // Day 1](https://www.youtube.com/watch?v=9eH16Fxeb9o)
- [dr inż. Maciej Sobieraj - Przełączniki](http://maciej.sobieraj.pracownik.put.poznan.pl/W03_TAPS.pdf)
- [dr inż. Maciej Sobieraj - Sieci komputerowe Wykład 2](http://maciej.sobieraj.pracownik.put.poznan.pl/PWSZ/W02.pdf)

**Spis treści:**
- [Przełączanie](#przełączanie)
  - [Jak przełącznik przekazuje ramki?](#jak-przełącznik-przekazuje-ramki)
    - [Tablica adresów MAC przełącznika](#tablica-adresów-mac-przełącznika)
    - [Uczenie i przekazywanie](#uczenie-i-przekazywanie)
      - [Uczenie - badanie źródłowego adresu MAC](#uczenie---badanie-źródłowego-adresu-mac)
      - [Przekazywanie - badanie docelowego adresu MAC](#przekazywanie---badanie-docelowego-adresu-mac)
  - [Metody przekazywania](#metody-przekazywania)
    - [store and forward](#store-and-forward)
    - [cut-through](#cut-through)
    - [fragment free](#fragment-free)
  - [Domeny przełączania](#domeny-przełączania)
    - [Domeny kolizyjne](#domeny-kolizyjne)
    - [Domeny rozgłoszeniowe](#domeny-rozgłoszeniowe)

## Jak przełącznik przekazuje ramki?

Przełącznik to urządzenie warstwy drugiej, co oznacza, że decyzje o przekazaniu ramki podejmuje jedynie w oparciu o adresy MAC. Z przekazywaniem ramek wiążą się dwa terminy:

- **Ingress** - używane w odniesieniu do portu, przez który ramki wchodzą do przełącznika
- **Egress** używane w odniesieniu do portu, przez który ramki opuszczą przełącznik

Przełącznik nigdy nie przekaże ramki Ethernet do tego samego portu, którym ją odebrał

### Tablica adresów MAC przełącznika

Przełącznik utrzymuje tablicę, która opisuje relacje adresów fizycznych podłączonych do niego urządzeń i portów przełącznika. Przechowuje ją w pamięci CAM (*content addressable memory*).

Przełącznik wykorzystuje tablicę adresów MAC do przekazywania ramek do konkretnego urządzenia przez port, który został do tego urządzenia przypisany

### Uczenie i przekazywanie

#### Uczenie - badanie źródłowego adresu MAC

Przełącznik każdą wchodzącą ramkę bada pod kątem nowych informacji sprawdzając **źródłowy** adres MAC i numer portu, na którym odebrał ramkę

- jeżeli nie ma źródłowego adresu MAC ramki, przełącznik dodaje nowy wpis do tablicy z tym adresem i numerem portu, na którym ramka została odebrana
- jeżeli tablica adresów MAC przełącznika zawiera źródłowy adres MAC, zostaje zaktualizowany licznik czasu odświeżania dla tego wpisu. Domyślnie większość przełączników utrzymuje wpis w tabeli przez 5 minut
- jeśli tablica zawiera źródłowy adres MAC, ale jest on skojarzony z innym portem, przełącznik traktuje to jako nowy wpis, zastępując poprzedni z użyciem tego samego adresu, ale z nowym numerem portu

#### Przekazywanie - badanie docelowego adresu MAC

Decyzję o przekazaniu ramki przełącznik podejmuje na podstawie docelowego adresu MAC odebranej ramki

- jeśli przełącznik w swojej tablicy adresów MAC posiada wpis pasujący do adresu docelowego ramki, przekazuje ją przez skojarzony z tym adresem port
- jeżeli docelowy adres MAC nie figuruje w tablicy adresów, przełącznik wysyła ramkę przez wszystkie porty oprócz tego, z którego została odebrana (jest to nazywane *nieznanym unicastem*)
- jeżeli docelowy adres MAC jest *broadcastem* lub *multicastem*, przełąncznik również wysyła ramkę przez wszystkie porty z wyjątkiem tego, którym weszła

## Metody przekazywania

### store and forward

W tej metodzie, przełącznik, zanim ją przekaże, buforuje całą ramkę i sprawdza pod kątem błędów korzystając z matematycznego mechanizmu sprawdzania błędów znanego jako *cykliczna kontrola nadmiarowości* (CRC).



Po odebraniu całej ramki przełącznik porównuje wartość sekwencji kontroli ramki (FCS) z własnymi obliczeniami FCS. Ten proces ma na celu sprawdzenie, czy ramka jest prawidłowa. Jeżeli ramka jest wolna od błędów, zostaje przekazana dalej, w przeciwnym razie - przełącznik ją porzuca.

Proces buforowania portu wejściowego wykorzystywany przez metodę *store-and-forward* zapewnia elastyczną obsługę dowolnej kombinacji prędkości Ethernet. Jeśli występuje różnica prędkości transmisji na portach wejściowym i wyjściowym, przykładowo obsługa ramek przychodzących z prędkością 100Mb/s, które mają zostać wysłane z prędkością 1Gb/s, konieczne jest użycie metody store-and-forward. Przełącznik zachowa całą ramkę w buforze, obliczy FCS i ewentualnie przekaże do bufora portu wyjściowego i wyśle dalej

### cut-through

Metoda *cut-through* jest szybsza od *store-and-forward*, ponieważ przekazuje ramkę gdy tylko sprawdzi docelowy adres MAC. Oznacza to, że ta metoda ma możliwość szybkiego przekazywania ramek, ale może przekazywać ramki niepoprawne, ponieważ nie sprawdza ich pod kątem błędów (nie sprawdza wartości FCS) i wpłynąć negatywnie na wydajność sieci, ponieważ pasmo jest zużywane na uszkodzone lub niepoprawne ramki.

Jednak ta metoda wprowadza małe opóźnienia, co czyni ją odpowiednią dla bardzo wymagających obliczeniowo aplikacji wysokich wydajności (*high-performance computing*, HPC), które wymagają opóźnień na poziomie nie większych niż 10 mikrosekund

### fragment free

Ta metoda jest zmodyfikowaną formą *cut-through*, w której przełącznik zaczyna przekazywać ramkę dopiero po przeczytaniu pola *Typ*, dzięki czemu zapewnia lepsze sprawdzenie błędów, praktycznie nie wprowadzając dodatkowych opóźnień

## Domeny przełączania

### Domeny kolizyjne

W starszych sieciach Ethernet, w segmentach opartych na *koncentratorze*, urządzenia sieciowe rywalizowały ze sobą o dostęp do medium transmisyjnego. Fragmenty sieci, które współdzielą pasmo między urządzeniami, to tzw. *domeny kolizji*. Kolizja występuje wtedy, gdy dwa lub więcej urządzeń w tej samej domenie kolizji spróbuje się komunikować w tym samym czasie.

Nie ma domen kolizji, jeśli porty przełącznika działają w pełnym dupleksie. Jednak domena kolizyjna może istnieć, jeśli port przełącznika działa w trybie półdupleksu - w tym przypadku port przełącznika będzie częścią domeny kolizyjnej.

Domyślnie porty przełącznika Ethernet będą automatycznie negocjować pełny dupleks, jeśli sąsiednie urządzenie też może działać w trybie pełnego dupleksu

### Domeny rozgłoszeniowe

Zbiór połączonych ze sobą przełączników tworzy jedną domenę rozgłoszeniową. Domena rozgłoszeniowa warstwy drugiej jest nazywana domeną rozgłoszeniową MAC. Zawiera ona wszystkie urządzenia w sieci LAN, które otrzymają ramki rozgłoszeniowe wysłane przez hosta.

Przełącznik odbierając ramkę rozgłoszeniową, przekazuje ją na wszystkie porty oprócz wejściowego. Szerokość pasma jest zużywana na ruch rozgłoszeniowy, który zmniejsza wydajność sieci. Zbyt wiele transmisji i duże obciążenie ruchem może powodować przeciążenie sieci, co spowalnia jej działanie.

Tylko urządzenia warstwy trzeciej , takie jak routery, mogą podzielić domenę rozgłoszeniową warstwy drugiej. Routery segmentują również domenę kolizyjną.

Zmniejszanie przeciążenia obejmuje takie cechy przełącznika jak: szybkie prędkości portów, szybkie przełączanie wewnętrzne, duże bufory ramek i wysoka gęstość portów