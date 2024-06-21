# Adresy IPv4

[Politechnika Poznańska: Sieci Komputerowe I - Podstawy (adresacja)](https://www.cs.put.poznan.pl/ddwornikowski/sieci/sieci1/adresacja.html)

[Pasja informatyki: Adresowanie IP v4. Budowa adresów, obliczenia, podział na podsieci](https://www.youtube.com/watch?v=t3IceGlTjig)

Adres IP to adres _logiczny_ komputera. Informuje urządzenia z tej samej lub odległej sieci, gdzie należy dostarczyć dany pakiet. Podobnie jak adres naszego domu mówi kurierowi, gdzie ma dostarczyć paczkę

Adres IPv4 to 32-bitowy adres hierarchiczny, który składa się z **części sieci** i **części hosta**

**Spis treści**

- [Adresy IPv4](#adresy-ipv4)
  - [Reprezentacja adresów IPv4](#reprezentacja-adresów-ipv4)
  - [Rodzaje adresów IPv4](#rodzaje-adresów-ipv4)
    - [unicast](#unicast)
      - [adresy prywatne](#adresy-prywatne)
      - [adresy publiczne](#adresy-publiczne)
    - [multicast](#multicast)
    - [broadcast](#broadcast)
    - [pętla zwrotna](#pętla-zwrotna)
  - [Podsieci](#podsieci)
    - [Maska podsieci](#maska-podsieci)
    - [Klasy adresów IPv4](#klasy-adresów-ipv4)
    - [CIDR](#cidr)
    - [Adresy w podsieci](#adresy-w-podsieci)
    - [Obliczanie adresu podsieci](#obliczanie-adresu-podsieci)
    - [Obliczanie adresu broadcast podsieci](#obliczanie-adresu-broadcast-podsieci)
    - [Obliczanie ilości hostów w podsieci](#obliczanie-ilości-hostów-w-podsieci)
    - [Tworzenie podsieci](#tworzenie-podsieci)
  - [Problemy IPv4](#problemy-ipv4)

## Reprezentacja adresów IPv4

Adresy IPv4 przedstawiane są jako 4 liczby dziesiętne z zakresu od 0 do 255 oddzielone kropkami na przykład:

- 192.168.0.1
- 10.1.43.5
- 127.0.0.1

 Binarnie są to 4 _oktety_. Oktet ma długość 8 bitów. System binarny jest bardzo ważny przy omawianiu IPv4, dlatego koniecznie należy się z nim zapoznać

 [Pasja informatyki: Zamiana liczb - system dwójkowy, szesnastkowy, ósemkowy, dziesiętny](https://www.youtube.com/watch?v=VUHwfugYFEA)

## Rodzaje adresów IPv4

### unicast

Adresy _unicast_ są przypisywane do jednego konkretnego urządzenia w sieci jednoznacznie je identyfikując w sieci

#### adresy prywatne

Część adresów z puli możliwych adresów IPv4 to tzw. _adresy prywatne_. Są stosowane wyłącznie w sieciach lokalnych i nie mogą one być routowane przez Internet. Spowolniło to wyczerpanie adresów IP, ponieważ dziesiątki czy setki urządzeń mogą współdzielić jeden lub kilka _adresów publicznych_ (dzięki _NAT_, który tłumaczy adresy prywatne na adresy publiczne)

Grupy prywatnych adresów IPv4:

- 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
- 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
- 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)

#### adresy publiczne

Adresy publiczne pozwalają hostom na dostęp do Internetu i jednoznacznie identyfikują dany węzeł w Internecie

### multicast

Adresy multicast są używane, gdy pakiet jest wysyłany do więcej niż jednego hosta w sieci. O tego typu adresach mówi dokument RFC 3171

Adresy multicast to adresy od od **224.0.0.0** do **239.255.255.255**

### broadcast

Adres broadcast (rozgłoszeniowy) jest wykorzystywany, gdy pakiet jest wysyłany do wszystkich hostów w podsieci

### pętla zwrotna

Pętla zwrotna (ang. _local loopback_) jest wykorzystywana do testowania systemów. Pakiety wysyłane na pętlę zwrotną nie opuszczają komputera. Adres pętli zwrotnej to **127.0.0.1** (_localhost_)

## Podsieci

### Maska podsieci

Maska podsieci mówi do jakiej podsieci należy adres IP. Maska podsieci ma długość 32 bitów i składa się z dwóch części: części sieci i części hosta, podobnie jak adres IP i również zapisujemy ją jako cztery liczby (_oktety_) dziesiętne odzielone kropkami, np. 

- **255.255.248.0**
- **255.255.255.0**
- **255.255.255.240**

W binarnej postaci maska _część sieci_ składa się z samych jedynek, natomiast _część hosta_ z samych zer, przykładowo maskę **255.255.248.0** binarnie zapiszemy **11111111.11111111.11111000.00000000**

część sieci | część hosta
:---: | :---:
11111111.11111111.11111 | 000.00000000

Część sieci mówi nam, ile _bitów_ w adresie należy do adresu (prefiksu) podsieci (ilość "jedynek"), a ile bitów należy do hosta (ilość "zer"), dlatego przy rozważaniu podsieci IPv4 bardzo ważna jest postać binarna adresu i maski podsieci

### Klasy adresów IPv4

Dawniej używano _klas_ adresów IPv4

- klasa **A**
  - pierwszy bit - 0
  - pierwszy oktet należy do sieci, pozostałe do hosta
  - odpowiada masce 255.0.0.0
- klasa **B**
  - pierwsze bity - 10
  - dwa pierwsze oktety należą do sieci, pozostałe do hosta
  - odpowiada masce 255.255.0.0
- klasa **C**
  - pierwsze bity - 110
  - trzy pierwsze oktety należą do sieci, czwarty do hosta
  - odpowiada masce 255.255.255.0
- klasa **D**
  - pierwsze bity - 1110
  - multicast
  - adresy z zakresu 224.0.0.0 – 239.0.0.0
- klasa **E**
  - pierwsze bity - 1111
  - eksperymentalne
  - adresy z zakresu 240.0.0.0 – 254.0.0.0

Obecnie zamiast klas używa się **CIDR**, długości _prefiksu_ i **VLMS**

### CIDR

CIDR (_Classless Inter-Domain Routing_) to bezklasowa metoda przydzielania adresów IP, w której długość maski podsieci (liczba "jedynek") zależy od potrzeb danej podsieci (**VLMS** - _Variable Length Subnet Mask_)

W notacji CIDR maskę podsieci zapiszemy w taki sposób: `/n`, gdzie **`n`** jest długością prefiksu (ilością "jedynek" w zapisie binarnym maski), przykładowo:

- /24 odpowiada masce 255.255.255.0
- /27 odpowiada masce 255.255.255.224
- /21 odpowiada masce 255.255.248.0
- /15 odpowiada masce 255.254.0.0

Tak więc np. adres **192.168.8.15** z maską **255.255.255.0** zapiszemy w skrócie **192.168.8.15/24**

### Adresy w podsieci

Dla danej podsieci możemy wyróżnić trzy rodzaje adresów:

- **adres podsieci** - pierwszy adres należący do danej podsieci
- **adres rozgłoszeniowy** (_broadcast_) - ostatni adres należący do danej podsieci
- **adresy hostów** - adresy, które możemy przypisać konkretnym hostom

### Obliczanie adresu podsieci

Aby wyznaczyć adres podsieci, do jakiej należy dany adres IPv4, potrzebujemy jego maskę. Dla przykładu:

- adres: 192.168.0.1
- maska: 255.255.248.0 (/21)

Teraz należy zamienić adres IP i maskę na postać binarną. Zapiszmy to jedno pod drugim

dana | postać dziesiętna | postać binarna
:--- | :---: | :---:
Adres | 192.168.0.1 | 11000000101010000000000000000001
Maska | 255.255.248.0 | 11111111111111111111100000000000

Teraz dla odpowiadających sobie bitów w adresie i masce należy wykonać operację logiczną ***AND***

otrzymamy wynik: **11000000101010000000000000000000**, co odpowiada wartości dziesiętnej z kropkami: **192.168.0.0**

Tak więc nasz adres sieci to **192.168.0.0/21**

### Obliczanie adresu broadcast podsieci

Najpierw, mając postać binarną maski, wykonujemy na niej operację logiczną ***NOT*** (negujemy maskę). Teraz zapiszmy postać binarną adresu IP i zanegowaną maskę jedno pod drugim

dana | postać binarna
:--- | :---:
Adres | 11000000101010000000000000000001
NOT Maska | 00000000000000000000011111111111

Teraz dla odpowiadających sobie bitów w adresie i zanegowanej masce należy wykonać operację logiczną ***OR***

otrzymamy wynik: **11000000101010000000011111111111**, co odpowiada wartości dziesiętnej z kropkami: **192.168.7.255**

Tak więc nasz adres sieci to **192.168.7.255/21**

### Obliczanie ilości hostów w podsieci

Aby obliczyć ilość użytecznych adresów dla hostów w podsieci wystarczy podnieść 2 do potęgi będącej długością części hosta i odjąć 2 - adres podsieci i adres broadcast

Otrzymaliśmy wzór: 

**2<sup>n</sup> - 2**

gdzie ***n*** jest długością części hosta, które możemy obliczyć odejmując długość prefiksu od 32, np. dla maski /21, część hosta ma długość **n = 32 - 21** czyli **11**

Dla naszego przykładu będzie to w takim razie:

**2<sup>11</sup> - 2 = 2046**

Pierwszy użyteczny adres z tej puli 2046 użytecznych adresów to **192.168.0.1** (o 1 większy od adresu podsieci), a ostatni **192.168.7.254** (o 1 mniejszy od adresu broadcast)

Tak więc mamy obliczone:
```
Adres IPv4: 192.168.0.1
Maska podsieci: /21

Adres podsieci: 192.168.0.0
Pierwszy użyteczny adres: 192.168.0.1
Ostatni użyteczny adres: 192.168.7.254
Adres broadcast: 192.168.7.255
Maska: 255.255.248.0
Liczba hostów: 2046
```

### Tworzenie podsieci

Podsieci IPv4 są tworzone poprzez użycie jednego lub większej ilości bitów z części hosta jako bitów części sieci

Przykładowo, chcemy podzielić sieć **192.168.0.0/24** na dwie mniejsze. Zabierzemy tym samym 1 bit części hosta na rzecz części sieci otrzymując maskę /25. W ten sposób otrzymaliśmy dwie mniejsze sieci:

- **192.168.0.0/25**
- **192.168.0.128/25**

Jeśli chcielibyśmy uzyskać 4 mniejsze podsieci, należałoby zabrać 2 bity:

- **192.168.0.0/26**
- **192.168.0.64/26**
- **192.168.0.128/26**
- **192.168.0.192/26**

Dzięki temu możemy tworzyć podsieci dla odpowiedniej liczby hostów, aby marnować jak najmniej adresów

Dla przykładu, mamy do dyspozycji sieć **192.168.8.0/24** i chcemy ją podzielić na podsieci, w których będzie po 10 komputerów oraz chcemy zmarnować jak najmniej adresów

Sprawdźmy, jakiej maski potrzebujemy:

- dla /25: 2<sup>7</sup> - 2 = 126 hostów
- dla /26: 2<sup>6</sup> - 2 = 62 hosty
- dla /27: 2<sup>5</sup> - 2 = 30 hostów
- dla /28: 2<sup>4</sup> - 2 = 14 hostów
- dla /29: 2<sup>3</sup> - 2 = 6 hostów - <span style="color:red">za mała!</span>

Podsieć z maską /29 jest już za mała, dlatego bierzemy pierwszą większą od niej podsieć - także naszą odpowiedzią jest maska /28 (255.255.255.240)

## Problemy IPv4

Do głównych problemów IPv4 należy wyczerpanie się **puli dostępnych adresów**. IPv4 ma teoretycznie 4.3 mld adresów, co okazało się za małą ilością, aby zaspokoić potrzeby dzisiejszego świata. W 2019 roku pula dostępnych adresów IPv4 wyczerpała się

Adresy prywatne i ***NAT*** (_Network Address Translation_) spowolniły proces kończenia się adresów IPv4, jednak ostatecznie koniecznym będzie przejść w całości na **IPv6**. Dodatkowo NAT może powodować problemy z połączeniami _end-to-end_

**Techniki migracji** z IPv4 na IPv6 można podzielić na 3 kategorie:

- podwójny stos
- tunelowanie
- translacja

Liczba możliwych adresów IPv4 to 2<sup>32</sup>, natomiast długość adresu IPv6 wynosi 128, dlatego liczba możliwych adresów IPv6 to 2<sup>128</sup>