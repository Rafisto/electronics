+++
date = '2024-11-19T10:00:00+01:00'

title = '3.6 Kryptografia'
+++

- [Kryptografia](#kryptografia)
  - [Ciekawe Źródła](#ciekawe-źródła)
  - [Kodowanie](#kodowanie)
    - [Kodowanie ASCII](#kodowanie-ascii)
    - [Kodowanie UTF-8](#kodowanie-utf-8)
    - [Kodowanie Base64](#kodowanie-base64)
      - [Przykład w technologiach webowych](#przykład-w-technologiach-webowych)
      - [Przykład w technologiach cloud](#przykład-w-technologiach-cloud)
      - [Security through obscurity](#security-through-obscurity)
  - [Szyfrowanie](#szyfrowanie)
    - [Szyfrowanie Symetryczne](#szyfrowanie-symetryczne)
      - [Szyfr XOR](#szyfr-xor)
      - [Szyfr Cezara](#szyfr-cezara)
      - [Szyfr AES](#szyfr-aes)
    - [Szyfrowanie Asymetryczne](#szyfrowanie-asymetryczne)
      - [RSA](#rsa)
        - [Algorytm RSA](#algorytm-rsa)
        - [Minimum Teorii Grup](#minimum-teorii-grup)
        - [Funkcja Phi Eulera](#funkcja-phi-eulera)
        - [Odwrotność modulo](#odwrotność-modulo)
        - [Dowód](#dowód)
        - [Siła algorytmu RSA](#siła-algorytmu-rsa)
  - [Protokoły wymiany klucza](#protokoły-wymiany-klucza)
    - [Diffie Hellman](#diffie-hellman)
      - [Przebieg ustalania klucza](#przebieg-ustalania-klucza)
      - [Dowód poprawności](#dowód-poprawności)
      - [Zastosowanie](#zastosowanie)
  - [Funkcje hashujące](#funkcje-hashujące)
    - [SHA256](#sha256)
    - [Dlaczego HashMap jest tak wydajny?](#dlaczego-hashmap-jest-tak-wydajny)
    - [Przykłady](#przykłady)
  - [Deniable Encryption](#deniable-encryption)


# Kryptografia

## Ciekawe Źródła

- [cryptohack.org](https://cryptohack.org/challenges/) - strona z zadaniami z kryptografii, która porusza tematykę kodowania, szyfrów symetrycznych, asymetrycznych, aspektów matematycznych - teorii grup szeroko wykorzystywanej w kryptografii, oraz wiele innych niezwykle ciekawych zagadnień.

## Kodowanie

Kodowanie to proces przekształcania danych z jednej postaci na inną. Nie ma na celu ukrycia wiadomości, a jedynie zmianę jej formy.

### Kodowanie ASCII

[ASCII](https://en.wikipedia.org/wiki/ASCII) (American Standard Code for Information Interchange) to standardowy kod znaków używany do reprezentacji tekstów w komputerach i innych urządzeniach, które wykorzystują tekst. Każdy znak jest reprezentowany przez liczbę z zakresu 0-127.

### Kodowanie UTF-8

UTF-8 (Unicode Transformation Format) to standard kodowania znaków Unicode. Jest to kodowanie zmiennodługościowe, co oznacza, że różne znaki mogą być reprezentowane przez różną liczbę bajtów. Jest wstecznie kompatybilny z ASCII oraz pozwala na reprezentację znaków za pomocą 1-4 bajtow. Początkowa seria bitów w pierwszym bajcie określa ilość bajtów, które reprezentują pojedyńczy znak, a kolejne bajty posiadają na początku stałą strukturę. Pozwala to jasno odczytywać poszczególne znaki.

| Ilość Bajtów | B_1      | B_2      | B_3      | B_4      |
| ------------ | -------- | -------- | -------- | -------- |
| 1            | 0xxxxxxx |          |          |          |
| 2            | 110xxxxx | 10xxxxxx |          |          |
| 3            | 1110xxxx | 10xxxxxx | 10xxxxxx |          |
| 4            | 11110xxx | 10xxxxxx | 10xxxxxx | 10xxxxxx |

Przykładowo litera `A` jest reprezentowana przez bajt `01000001` (kompatybilność z ASCII), a litera `ą` przez dwa bajty `11000011 10100001`.

### Kodowanie Base64

Base64 polega na przekształceniu danych binarnych na znaki ASCII. Jest to kodowanie zmiennodługościowe, co oznacza, że każde 3 bajty danych binarnych są zamieniane na 4 znaki ASCII. W przypadku, gdy liczba bajtów danych binarnych nie jest podzielna przez 3, dodawane są znaki `=`, tzw. padding.

- Zbiór znaków dla systemu dziesiętnego: `0-9`.
- Zbiór znaków dla systemu szesnastkowego: `0-9, A-F`.
- Zbiór znaków dla systemu base64: `A-Z, a-z, 0-9, +, /`.

Base64 przydaje się w przypadku, gdy chcemy przesłać dane binarne w formie tekstowej, bądź prowizorycznie ukryć zrozumiały tekst.

#### Przykład w technologiach webowych

Możemy w języku HTML umieścić obrazek w formie base64:

```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/..."/>
```

Pozwala to na umieszczenie obrazka bez konieczności odwoływania się do zewnętrznego pliku, nie jest to jednak zalecane ze względu na zwiększenie rozmiaru strony.
Istnieją pewne bazy danych, np. testowa baza danych [sakila-db](https://github.com/jOOQ/sakila), która w tabeli Staff posiada kolumnę `picture` z obrazkami czytanymi jako base64.

#### Przykład w technologiach cloud

W orkiestratorze kubernetes możemy [stworzyć resource typu secret](https://kubernetes.io/docs/concepts/configuration/secret/), który przechowuje dane w formie base64. Przykładowo, możemy stworzyć secret przechowujący dane do logowania do rejestru OCI (systemu hostującego obrazy kontenerów):

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: secret-dockercfg
type: kubernetes.io/dockercfg
data:
  .dockercfg: |
    eyJhdXRocyI6eyJodHRwczovL2V4YW1wbGUvdjEvIjp7ImF1dGgiOiJvcGVuc2VzYW1lIn19fQo=  
```

Dane w pliku `.dockercfg` są zakodowane w base64, odkowowane do postaci oryginalnej prezentują: `{"auths":{"https://example/v1/":{"auth":"opensesame"}}}`.

#### Security through obscurity

[Security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity) to praktyka polegająca na ukrywaniu informacji w wierze, że to wystarczy do zabezpieczenia systemu. Historia pokazuje, że jest to złudne przekonanie, ponieważ zabezpieczenia oparte na ukrywaniu informacji są zazwyczaj łatwe do złamania.

Stojący w bezpośredniej opozycji do wyżej podanego podejścia jest [Reguła Kerckhoffsa](https://en.wikipedia.org/wiki/Kerckhoffs%27s_principle), która mówi, że system kryptograficzny powinien być bezpieczny nawet jeśli wszystkie jego detale są znane, z wyjątkiem klucza. Zgodnie z tą zasadą, klucz powinien być jedynym elementem, który zapewnia bezpieczeństwo systemu. Praktycznie wszystkie współczesne systemy kryptograficzne są oparte na tej zasadzie, poniżej opiszę kilka z nich.

## Szyfrowanie

Szyfrowanie stanowi proces przekształcenia danych w taki sposób, aby były one niezrozumiałe dla osób, które nie posiadają odpowiedniego klucza. Wyróżniamy dwa główne rodzaje szyfrowania: symetryczne i asymetryczne. W szyfrowaniu symetrycznym używamy jednego klucza do szyfrowania i deszyfrowania danych, natomiast w szyfrowaniu asymetrycznym używamy dwóch kluczy: publicznego i prywatnego, pozwala nam to na bezpieczne przesyłanie danych bez konieczności dzielenia się kluczem prywatnym.

### Szyfrowanie Symetryczne

Istnieje niezliczona liczba szyfrów symetrycznych, które różnią się między sobą sposobem działania, ich wspólną cechą jest fakt, że używają jednego klucza zarówno do szyfrowania, oraz deszyfrowania danych.

#### Szyfr XOR

Rozważmy następujące zależności dla operacji $\oplus$ XOR:

$A \oplus 0 = A$

$A \oplus A = 0$

$A \oplus B = B \oplus A$

$(A \oplus B) \oplus C = A \oplus (B \oplus C)$

$(B \oplus A) \oplus A = B \oplus 0 = B$

Szyfrowanie metodą XOR uznaje się za jednorazowe, gdyż przy użyciu takiego samego klucza do dwóch wiadmości, dla potencjalnego łamacza szyfrów istnieje tylko jedna permutacja `0, 1`, dla których przetłumaczone zostaną jednocześnie dwie wiadomości.

Zauważmy, że (cytując Wikipedię):

> If the key is random and is at least as long as the message, the XOR cipher is much more secure than when there is key repetition within a message. When the keystream is generated by a pseudo-random number generator, the result is a stream cipher. With a key that is truly random, the result is a one-time pad, which is unbreakable in theory. 

W idealnych warunkach jest to szyfr nie do złamania metodami matematycznymi.

```plaintext
tekst = 1010
klucz = 1100

szyfrowanie:

(1010) XOR (1100) = (0110)

odszyfrowanie:

(0110) XOR (1100) = (1010)
```

#### Szyfr Cezara

Najprostszym przykładem szyfrowania symetrycznego jest szyfr Cezara (ROT13), w którym to każda litera tekstu jawnego jest przesuwana o stałą liczbę miejsc w alfabecie.

Kluczem jest liczba informująca o ile znaków przesuwamy litery w alfabecie. Przykładowo, dla klucza 3 litera `A` zostanie zaszyfrowana jako `D`, a litera `Z` jako `C` (przesunięcie w cyklu).

```plaintext
(rot3)
AXY -> (A+3)(B+3)(C+3) -> DAB
DAB -> (D-3)(A-3)(B-3) -> AXY
```

#### Szyfr AES

[AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) (Advanced Encryption Standard) to standard szyfrowania symetrycznego, który jest powszechnie stosowany w wielu aplikacjach. Jest to blokowy szyfr, co oznacza, że dane są dzielone na bloki o stałej długości, a każdy blok jest szyfrowany niezależnie od innych bloków. Klucz szyfrowania może mieć długość 128, 192 lub 256 bitów, co oznacza, że AES-128, AES-192 i AES-256.

AES wykorzystywany jest w wielu protokołach kryptograficznych, takich jak TLS, IPsec, SSH. Swoją popularność zawdzięcza szybkości działania, prostocie implementacji oraz bezpieczeństwu, opartemu na wielu iteracjach.

Pierwsze ustalenie klucza odbywa się jednak za pomocą szyfrowania asymetrycznego. 

### Szyfrowanie Asymetryczne

Szyfrowanie asymetryczne to proces, w którym używamy dwóch kluczy: publicznego i prywatnego. Klucz publiczny jest dostępny dla wszystkich, natomiast klucz prywatny jest znany tylko jednej osobie. Klucz publiczny służy do szyfrowania danych, natomiast klucz prywatny do ich deszyfrowania. Dzięki temu, że klucz prywatny jest znany tylko jednej osobie, pozwala to na bezpieczne przesyłanie danych.

Szyfrowanie asymetryczne pełni dwie funkcje:
- Szyfrowanie danych [Digital Signature](https://en.wikipedia.org/wiki/Digital_signature) - klucz publiczny służy do szyfrowania danych, które mogą być odczytane tylko przez osobę posiadającą klucz prywatny.
- Podpis cyfrowy - klucz prywatny służy do podpisywania danych, które mogą być zweryfikowane przez osobę posiadającą klucz publiczny.

#### RSA

[RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) to jeden z najpopularniejszych algorytmów szyfrowania asymetrycznego, który został opublikowany w 1977 roku przez Ronalda Rivesta, Adi Shamira i Leonarda Adlemana. RSA opiera się na trudności rozkładu liczby na czynniki pierwsze. Klucz publiczny składa się z dwóch liczb: `n` i `e`, natomiast klucz prywatny z dwóch liczb: `n` i `d`. Klucz publiczny służy do szyfrowania danych, natomiast klucz prywatny do ich deszyfrowania.

##### Algorytm RSA

Dobieramy dwie duże liczby pierwsze $p$ i $q$, a następnie obliczamy $n = p * q$. Następnie obliczamy funkcję Eulera $\phi(n) = (p-1)(q-1)$. Następnie wybieramy liczbę $e$ względnie pierwszą z $\phi(n)$. Następnie obliczamy liczbę $d$ odwrotną do $e$ modulo $\phi(n)$. Klucz publiczny to para $(n, e)$, natomiast klucz prywatny to para $(n, d)$.

Weźmy wiadomość $w$ i klucz publiczny $(n, e)$, wtedy zaszyfrowana wiadomość $c$ wygląda następująco:

$c = w^e \mod n$

Wyznaczenie $d$ odbywa się za pomocą rozszerzonego algorytmu Euklidesa, który pozwala na znalezienie liczby odwrotnej modulo $\phi(n)$.

Aby odszyfrować wiadomość $c$ potrzebujemy klucza prywatnego $(n, d)$.

$w = c^d \mod n$

##### Minimum Teorii Grup

Do zrozumienia szyfru RSA potrzebujemy minimalne podstawy z gałęzi matematyki, nazywanej teorią grup. Grupa to zbiór elementów wraz z operacją, która spełnia następujące warunki:
- **Działanie zamknięte** - wynik operacji na dowolnych dwóch elementach z grupy należy do grupy.
- **Łączność** - wynik operacji na dowolnych dwóch elementach z grupy jest jednoznaczny.
- **Element neutralny** - istnieje element neutralny, który nie zmienia wartości innych elementów.
- **Element odwrotny** - dla każdego elementu z grupy istnieje element odwrotny, który po pomnożeniu przez element daje element neutralny.

Interesująca pod względem szyfru RSA jest [grupa multiplikatywna](https://en.wikipedia.org/wiki/Multiplicative_group) $\mathbb{Z}_n^*$, która składa się z liczb względnie pierwszych z $n$. 

$$\mathbb{Z}_n^* = \{a \in \mathbb{Z}_n : \gcd(a, n) = 1\}$$

Dla przykładu, dla $n = 15$ grupa multiplikatywna $\mathbb{Z}_{15}^*$ składa się z liczb: $\{1, 2, 4, 7, 8, 11, 13, 14\}$.

Okazuje się, że jeśli $n=p$, jest liczbą pierwszą, to grupa multiplikatywna $\mathbb{Z}_p^*$ jest grupą cykliczną. 

##### Funkcja Phi Eulera

Funkcja Phi Eulera $\phi(n)$ to funkcja, która zwraca liczbę liczb względnie pierwszych z $n$ mniejszych od $n$. Dla liczby pierwszej $p$ funkcja $\phi(p)$ zwraca $p-1$, natomiast dla liczby $n=p*q$ zwraca $(p-1)(q-1)$. Zauważmy, że:
- $\phi(p) = p-1$
- $\phi(p*q) = (p-1)(q-1)$, jeśli $p$ i $q$ są liczbami pierwszymi.
- $\phi(p^k) = p^{k-1}(p-1)$, dla $k \geq 1$.
- $\phi(n) = n * \prod_{p|n} (1 - \frac{1}{p})$, gdzie $p|n$ oznacza, że $p$ dzieli $n$.

Zauważmy że $|Z_n^*| = \phi(n)$, zatem dla $n=p*q$ mamy $|\mathbb{Z}_{pq}^*| = (p-1)(q-1)$.

##### Odwrotność modulo

[Odwrotność modulo](https://en.wikipedia.org/wiki/Modular_multiplicative_inverse) $a^{-1} \mod n$ to taka liczba $b$, że $a*b \equiv 1 \mod n$. W przypadku RSA, potrzebujemy odnaleźć liczbę $d$, która spełnia warunek $e*d \equiv 1 \mod \phi(n)$. Wyznaczenie $d$ odbywa się za pomocą rozszerzonego algorytmu Euklidesa, który pozwala na znalezienie liczby odwrotnej modulo $\phi(n)$.

Jeżeli nie znamy $\phi(n)$, to nie będziemy w stanie w prosty sposób wyznaczyć $d$.
Jeżeli znamy $\phi(n)$ i $e$, to możemy wyznaczyć $d$ za pomocą rozszerzonego algorytmu Euklidesa, jest to zadanie prostsze niż rozkład $n$ na czynniki pierwsze.

##### Dowód

Załóżmy, że mamy klucz publiczny $(n, e)$. Chcemy pokazać, że klucz prywatny $(n, d)$ pozwala na odszyfrowanie wiadomości zaszyfrowanej kluczem publicznym.

$$c = w^e \mod n$$

Pokażmy:

$$w \mod n = (w^e)^d \mod n = w^{e \cdot d} \mod n$$

Skoro $e$ oraz $d$ są odwrotne modulo $\phi(n)$, to $e \cdot d \equiv 1 \mod \phi(n)$.

Zatem istnieje liczba całkowita $k$, taka że $e \cdot d = 1 + k \cdot \phi(n)$:

$$w^{e \cdot d} \mod n = w^{1 + k \cdot \phi(n)} \mod n$$

Z twierdzenia Eulera wiemy, że jeśli $w,n$ są względnie pierwsze, to $w^{\phi(n)} \equiv 1 \mod n$:

$$w^{1 + k \cdot \phi(n)} \mod n = w \cdot w^{k \cdot \phi(n)} \mod n = w \cdot (w^{\phi(n)})^k \mod n = w \cdot 1^k \mod n = w \mod n = w$$

Jeśli $w,n$ nie są względnie pierwsze, to dowód wygląda podobnie, zostawiam czytelnikowi jako ćwiczenie.

##### Siła algorytmu RSA

Bezpieczeństwo algorytmu RSA polega na trudności w rozkładzie dużych liczb na czynniki pierwsze. Im większa liczba $n$ tym trudniejsze jest rozkładanie jej na czynniki pierwsze. Obecnie w praktyce używa się kluczy 2048, bądź 4096 bitowych.

Potencjalny atakujący musi znać wartość $\phi(n)$, aby móc policzyć $d = e^{-1} \mod \phi(n)$ (inaczej jest to strzelanie w ciemno i należy sprawdzić każdą liczbę).
Zobaczmy że wyliczenie $\phi(n)$ dla dużej liczby $n$ jest trudnym obliczeniowo, ponieważ wymaga rozkładu $n$ na czynniki pierwsze. 


## Protokoły wymiany klucza

Dawniej klucze szyfrowania były przekazywane osobiście, co było bardzo niewygodne. Na szczęście wymyślono metody wymiany klucza, które pozwalają na bezpieczne przekazywanie kluczy szyfrowania, bez konieczności fizycznego spotkania się. Książeczki szyfrów przekazywane przez ambasadę, czy kurierów, to już przeszłość.

### Diffie Hellman

Diffie-Hellman to protokół wymiany klucza, który pozwala na bezpieczne przekazywanie kluczy szyfrowania. Protokół ten opiera się na trudności rozwiązania problemu logarytmu dyskretnego. Nie istnieje wydajny algorytm, który pozwala na rozwiązanie problemu logarytmu dyskretnego w skończonym ciele, co oznacza, że atakujący musi przetestować wszystkie możliwe wartości, co jest bardzo trudne dla dużych liczb.

Do zastosowania protokołu Diffie-Hellmana potrzebujemy:
- Liczby pierwszej $p$.
- Generatora $g$.
- Klucze prywatne $a$ i $b$.
- Klucze publiczne $A$ i $B$.

#### Przebieg ustalania klucza

Zobaczmy jak działa protokół Diffie-Hellmana na przykładzie dwóch uczestników: Alicji i Boba.

1. Alicja i Bob ustalają liczbę pierwszą $p$ oraz liczbę $g$ - generator grupy multiplikatywnej $\mathbb{Z}_p^*$.
2. Alicja wybiera liczbę $a$ i oblicza $A = g^a \mod p$.
3. Bob wybiera liczbę $b$ i oblicza $B = g^b \mod p$.
4. Alicja otrzymuje $B$ od Boba, natomiast Bob otrzymuje $A$ od Alicji.
5. Alicja oblicza klucz wspólny $K_a = B^a \mod p$.
6. Bob oblicza klucz wspólny $K_b = A^b \mod p$.
7. Alicja i Bob mają teraz wspólny klucz $K_a=K_b$.

#### Dowód poprawności

Pokażemy że klucz $K$ obliczony przez Alicję i Boba jest taki sam.

Bob otrzymał od Alicji $A = g^a \mod p$, zatem obliczając $K_b = A^b \mod p$ otrzyma:

$$K_b = A^b \mod p = (g^a)^b \mod p = g^{a \cdot b} \mod p$$

Alicja otrzymała od Boba $B = g^b \mod p$, zatem obliczając $K_a = B^a \mod p$ otrzyma:

$$K_a = B^a \mod p = (g^b)^a \mod p = g^{b \cdot a} \mod p$$

Z własności grupy multiplikatywnej $\mathbb{Z}_p^*$ wiemy, że $g^{a \cdot b} \mod p = g^{b \cdot a} \mod p$, zatem $K_a = K_b$.

#### Zastosowanie 

Powszechnie stosowany w ruchu sieciowym. Protokół ten pozwala na bezpieczne przekazywanie kluczy szyfrowania, bez konieczności dzielenia się kluczem prywatnym.
- [Komunikatory end-to-end, np. Signal](https://signal.org/docs/specifications/x3dh/) wykorzystuje zmodyfikowany protokół Diffie-Hellmana (X3DH) do bezpiecznej wymiany kluczy szyfrowania. Uwaga, nie jest on kwantowo bezpieczny, dlatego nie jest on jedynym zabezpieczeniem w komunikatorze.

## Funkcje hashujące

Wyobraźmy sobie, że chcemy bezpiecznie przechować hasła użytkowników w bazie danych. Nie chcemy przechowywać haseł w postaci jawnej, ponieważ w przypadku wycieku bazy danych, atakujący miałby dostęp do haseł wszystkich użytkowników. W takim przypadku stosuje się funkcje hashujące, które przekształcają ciąg bitów w sposób unikalny i nieodwracalny.

Funkcje hashujące pełnią wiele funkcji:
- Weryfikacja integralności danych (CHECKSUM)
- Przechowywanie Haseł
- Podpis cyfrowy (podpisywany jest hash wiadomości, a nie wiadomość)
- Tworzenie pochodnych klucza prywatnego, przykład - [Double Ratchet Algorithm](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm)
- Tworzenie unikalnych identyfikatorów (UUID)
- Detekcja wirusów (antywirusy porównują hashe plików z bazą danych wirusów)

Dobra funkcja hashująca musi być:
- Deterministyczna
- Szybka do policzenia
- Nieodracalna
- Odporna na kolizje
- Czuła na niewielkie zmiany danych wejściowych

### SHA256

[SHA256](https://en.wikipedia.org/wiki/SHA-2) to funkcja hashująca, która przekształca dowolnie długi ciąg bitów w ciąg 256 bitów. Jest to jedna z najpopularniejszych funkcji hashujących, która jest powszechnie stosowana w wielu aplikacjach. SHA256 jest uważana za bezpieczną, ponieważ nie istnieje skuteczny algorytm, który pozwala na znalezienie dwóch różnych wiadomości, które mają ten sam hash.

```plaintext
SHA256("The quick brown fox jumps over the lazy dog") = 9e107d9d372bb6826bd81d3542a419d6
```

### Dlaczego HashMap jest tak wydajny?

Znalezienie elementu w HashMapie odbywa się w czasie stałym $O(1)$, czyli czas potrzebny na znalezienie elementu nie zależy od ilości elementów w mapie.
Dobrze dobrana funkcja hashująca nie powoduje kolizji, czyli sytuacji, w której dwa różne klucze mają ten sam hash. W przypadku kolizji, elementy są przechowywane w postaci listy, co powoduje, że czas wyszukiwania elementu rośnie do $O(n)$, gdzie $n$ to ilość elementów w liście.

W podobnym rozumieniu konstrukcja warunkowa `switch` jest szybsza od konstrukcji `if-else`, ponieważ kompilator przekształca `switch` w tablicę skoków, co pozwala na znalezienie odpowiedniego przypadku w czasie stałym.

### Przykłady

- Chcemy w prosty sposób odnaleźć zmianę w kodzie źródłowym, możemy skorzystać z funkcji hashującej, która pozwoli nam na szybkie porównanie dwóch wersji pliku.
- Naprawmy bałagan na dysku twardym, w którym pliki rozrzucone i wielokrotnie kopiowane zajmują niepotrzebnie miejsce. Możemy wtedy skorzystać z funkcji hashującej do znalezienia duplikatów plików i usunięcia nadmiarowych kopii.
- Użytkownik identyfiuje się za pomocą hasła, które jest przechowywane w bazie danych. Nie chcemy przechowywać haseł w postaci jawnej, dlatego stosujemy funkcję hashującą, która przekształca hasło w postać nieodwracalną.

## Deniable Encryption

Dla własnego bezpieczeństwa przeczytaj [ten artykuł](https://en.wikipedia.org/wiki/Deniable_encryption) (deniable encryption).