# Adresy IPv6 (ściągawka)

**Przydatne linki**
- [dr inż. Maciej Sobieraj - Sieci komputerowe, Wykład 6-1](http://maciej.sobieraj.pracownik.put.poznan.pl/PWSZ/W06-1_2018.pdf)
- [Pasja informatyki - Sieci komputerowe odc. 9 - Wprowadzenie do IPv6](https://www.youtube.com/watch?v=hRl2vX8CGpk)
- [Oracle System Administration Guide: IP Services - IPv6 Addressing Overview](https://docs.oracle.com/cd/E18752_01/html/816-4554/ipv6-overview-10.html)
- [IBM - Protokół IPv6](https://www.ibm.com/docs/pl/i/7.1?topic=setup-internet-protocol-version-6)
- [Cisco Press - IPv6 Address Representation and Address Types](https://www.ciscopress.com/articles/article.asp?p=2803866)

**Spis treści**
- [Adresy IPv6 (ściągawka)](#adresy-ipv6-ściągawka)
  - [Potrzeba następcy - problemy IPv4](#potrzeba-następcy---problemy-ipv4)
  - [Gdzie jest IPv5??](#gdzie-jest-ipv5)
  - [Reprezentacja adresów IPv6](#reprezentacja-adresów-ipv6)
  - [Rodzaje adresów IPv6](#rodzaje-adresów-ipv6)
  - [unicast](#unicast)
    - [globalny](#globalny)
    - [link-local](#link-local)
    - [unique local address](#unique-local-address)
    - [adres wbudowany IPv4](#adres-wbudowany-ipv4)
    - [pętla zwrotna](#pętla-zwrotna)
  - [multicast](#multicast)
    - [dobrze znane adresy multicast](#dobrze-znane-adresy-multicast)
    - [solicited-node](#solicited-node)
  - [anycast](#anycast)
  - [Współistnienie IPv4 i IPv6](#współistnienie-ipv4-i-ipv6)
    - [podwójny stos](#podwójny-stos)
    - [tunelowanie](#tunelowanie)
    - [translacja](#translacja)
  - [Sposoby dynamicznej konfiguracji IPv6](#sposoby-dynamicznej-konfiguracji-ipv6)
  - [Odwzorowanie adresów IPv6](#odwzorowanie-adresów-ipv6)

## Potrzeba następcy - problemy IPv4

Z adresami IPv4 jest taki problem, że... już ich nie ma, skończyły się. Pula dostępnych adresów IPv4 (2<sup>32</sup>, niecałe 4.3 miliarda adresów) została wyczerpana w 2019 roku. Dlaczego więc mamy Internet, a świata nie spotkała apokalipsa rodem z Pisma Świętego? Wprowadzenie *NAT* (*Network Address Translation*) i pul adresów prywatnych spowolniło proces wyczerpywania się adresów IPv4, jednak nie jest to dobre rozwiązanie na daleki dystans, zwłaszcza, że może to stwarzać problemy z połączeniami *end-to-end*. Potrzeba nam następcy, nowych adresów IP. Na ratunek, niczym rycerz na białym koniu, który oferuje 2<sup>128</sup> adresów.

## Gdzie jest IPv5??

Cóż, IPv5 istnieje, ale nie ma na celu zastąpienia IPv4, a działać razem z nim. Jest nazwą dla zestawu protokołów [*The Internet Stream Protocol* (ST)](https://en.wikipedia.org/wiki/Internet_Stream_Protocol) i korzysta z tego samego schematu adresowania co IPv4 oraz enkapsulacji warstwy danych

## Reprezentacja adresów IPv6

Adresy IPv6 mają długość 128 bitów (dla porównania adresy IPv4 mają długość 32 bitów) i są zapisywane za pomocą cyfr szesnastkowych (0 - 9 i A - F) grupowane po 2 bajty (4 cyfry szesnastkowe) odzielone dwukropkiem, np. `2001:0db8:acad:0001:0000:0000:0000:0002`. Żeby skrócić zapis adresu IPv6, nie trzeba zapisywać *zer* występujących **na początku** danego *hextetu*, czyli powyższy adres będzie wyglądał tak: `2001:db8:acad:1:0:0:0:2`. Jeśli w adresie IPv6 mamy grupę hextetów składających się z samych *zer*, możemy ten fragment zastąpić podwójnym dwukropkiem: `2001:db8:acad:1::2`.

W przypadku IPv6 nie używa się maski sieciowej w formacie hextetów oddzielonych dwukropkami, a informuje się jedynie o długości prefixu, przykładowo `2001:db8:acad:1::2/64`

## Rodzaje adresów IPv6

## unicast

Tak jak w przypadku adresów IPv4, adres unicast IPv6 adresuje jeden interfejs sieciowy hosta.

### globalny

Globalne adresy unicast (GUA) IPv6 zaczynają się od prefiksu `2000::/3` i pełnią podobną funkcję co publiczne adresy IPv4 - są globalnie routowane.

IPv6 został zaprojektowany z myślą o tworzeniu podsieci i składa się z trzech części:

- **prefiks routingu globalnego** - jest przydzielany klientom przez dostawcę usług internetowych; obecnie rejestry RIR przydzielają prefiksy `/48`
- **identyfikator podsieci** - jest używany do rozróżniania podsieci w swojej sieci
- **identyfikatora interfejsu** - odpowiednik części hosta w IPv4

### link-local

Adresy *link-local* (LLA) **nie są** routowalne. Są wykorzystywane w komunikacji z innymi urządzeniami w ramach jednej podsieci i **tylko tej** podsieci. Wszystkie interfejsy mające włączony IPv6 **muszą** mieć skonfigurowany adres link-local. Zakres adresów link-local to `fe80::/10`

### unique local address

Unikalne adresy lokalne (ULA) nie są globalnie routowane. Routowane są jedynie w ramach jednej domeny routingu - odpowiednik adresów prywatnych IPv4. Adresu ULA mieszczą się w zakresie `ffc00::/7` - `ffdff:/7`

### adres wbudowany IPv4

Są stosowane w celu ułatwienia przejścia z IPv4 i IPv6. Do 96 bitów wartości szesnastkowych dodaje się z prawej 32 bity adresu IPv4 w formacie dziesiętnym, np. `::ffff:192.168.8.2/96`

### pętla zwrotna

Służy do wysyłania przez hosta pakietów do *samego siebie* - analogicznie do IPv4. Adres *loopback* IPv6 to `::1/128` (lub po prostu `::1`)

## multicast

Adresy multicast należą do zakresu `ff00::/8`

### dobrze znane adresy multicast

Dwa przykłady dobrze znanych adresów multicast:

- `ff02::1` - wszystkie urządzenia w sieci (IPv6 nie ma adresu broadcast)
- `ff02::2` - wszystkie routery

### solicited-node

[CBT Nuggets - What is a Solicited Nodes Multicast Group in IPv6?](https://www.cbtnuggets.com/blog/technology/networking/what-is-a-solicited-nodes-multicast-group-in-ipv6)

To specjalne adresy multicast odgrywające szczególną rolę w protokole ***Neighbor Discovery Protocol***. Jest tworzony automatycznie po skonfigurowaniu adresu unicast. Ma postać `ff02::1:ffxx:xxxx`, gdzie *x* zastępujemy 24 najmniej znaczącymi bitami adresu unicast. Przykładowo mając adres IPv6 `2001:db8::12:3456`, adres multicast solicited-node będzie miał postać `ff02::1:ff12:3456`.

Adresy multicast są mapowane na specjalne adresy multicastowe Ethernet

## anycast

Jeden adres anycast może zostać przypisany do wielu interfejsów, zazwyczaj należących do różnych urządzeń. Adresy anycast składniowo nie różnią się od adresów unicast, ale przy konfiguracji trzeba jawnie określić, że dany adres jest adresem typu anycast. Podczas wysyłania pakietu na adres anycast, zostaje on dostarczony do najbliższego interfejsu (zgodnie z używanym protokołem routingu) korzystającego z tego adresu

## Współistnienie IPv4 i IPv6

### podwójny stos

Urządzenia z podwójnym stosem obsługują jednocześnie IPv4 i IPv6, co pozwala na współistnienie tych protokołów w ramach tej samej sieci

### tunelowanie

Pakiet IPv6 jest enkapsulowany w pakiecie IPv4, dzięki czemu ruch IPv6 może być transportowany przez sieć IPv4

### translacja

NAT64 pozwala na tranlsacje adresów IPv4 na adresy IPv6 i na odwrót

## Sposoby dynamicznej konfiguracji IPv6

- **SLAAC** - urządzenie samo konfiguruje adres IPv6 oraz inne informacje na podstawie danych otrzymanych od routera
- **bezstanowy DHCPv6** - urządzenie samo konfiguruje adres IPv6 na podstawie danych otrzymanych od routera oraz pyta wskazany przez niego serwer DHCPv6 o dodatkowe potrzebne informacje
- **stanowy DHCPv6** - urządzenie pobiera adres IPv6 oraz inne potrzebne informacje od serwera DHCPv6

## Odwzorowanie adresów IPv6

Do mapowania adresów IPv4 na adresy fizyczne służy protokół ARP, natomiast w przypadku adresów IPv6 wykorzystywany jest protokół [*Neighbor Discovery Protocol* (NDP lub ND)](https://www.juniper.net/documentation/us/en/software/junos/neighbor-discovery/topics/topic-map/ipv6-neighbor-discovery.html), który wykorzystuje wiadomości ICMPv6.

W celu ustalenia adresu fizycznego docelowego urządzenia, węzeł wysyła wiadomość *neighbor solicitation* na odpowiedni adres *solicited-node*, na którą owe urządzenie odpowiada wiadomością *neighnor advertisement* zawierającą jego adres fizyczny