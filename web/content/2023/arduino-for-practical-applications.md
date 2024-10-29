+++
date = '2023-10-23T10:00:00+01:00'

title = '2.3 Arduino for practical applications'
+++

## Arduino dla zastosowań praktycznych

### Teoria

- Mikrokontroler - 
    Elektroniczny układ sterujący, wykonujący precyzyjnie określone zadania. Znajduje zastosowanie w wielu dziedzinach, od automatyki przemysłowej po urządzenia codziennego użytku.
- Mikrokomputer - 
    Samodzielny system z procesorem, pamięcią i interfejsami, pozwalający na wykonywanie bardziej złożonych operacji niż mikrokontroler. Przydatny w aplikacjach wymagających obliczeń lub interakcji z użytkownikiem.

#### Programowanie Arduino

Arduino składa się z mikrokontrolera, który jest mózgiem całego układu. Mikrokontroler ma wiele pinów, z których każdy może być programowany do pełnienia różnych funkcji:

- Wejścia cyfrowe (Digital Inputs): Są to piny, które mogą przyjmować tylko dwa stany: HIGH (1) lub LOW (0). Oznacza to, że mogą być używane do odczytu sygnałów cyfrowych, takich jak przyciski, czujniki obecności, czy cyfrowe sygnały z innych urządzeń.

- Wyjścia cyfrowe (Digital Outputs): Te piny mogą przesyłać tylko sygnały cyfrowe, tj. również tylko HIGH (1) lub LOW (0). Wykorzystywane są do sterowania diodami LED, przekaźnikami i innymi urządzeniami, które można włączać i wyłączać.

- Wejścia analogowe (Analog Inputs): Są to piny, które mogą odczytywać napięcia w zakresie od 0 do 5 voltów. Pozwala to na pomiar zmiennej wielkości, jak np. temperatura, jasność lub napięcie.

- Wyjścia analogowe (PWM - Pulse Width Modulation): Chociaż Arduino nie ma prawdziwych wyjść analogowych, wiele pinów może generować sygnały PWM. Jest to technika, która pozwala na emulację sygnału analogowego przez szybkie przełączanie między stanami HIGH i LOW.

- Komunikacja szeregowa (Serial Communication): Piny takie jak RX (receive) i TX (transmit) służą do komunikacji z innymi urządzeniami, takimi jak komputery, inne mikrokontrolery czy moduły Bluetooth.

- Inne specjalne funkcje: Niektóre piny mają specjalne funkcje, takie jak obsługa przerwań (interrupts), komunikacja I2C, SPI czy obsługa portów szeregowych.

- Zasilanie i uziemienie (Power and Ground): Oprócz pinów wejściowych i wyjściowych, Arduino ma piny służące do dostarczania zasilania (5V) oraz uziemienia (GND) dla podłączanych urządzeń.

- Aref (Analog Reference): Jest to pin, który pozwala na ustawienie zewnętrznego napięcia referencyjnego dla pomiarów analogowych.

Konfiguracja dla HC-SR04:

```c
#include <Arduino.h>

const int trigPin = 12;
const int echoPin = 14;

#define SOUND_VELOCITY 0.034

long duration;
float distanceCm;

void setup() {
  Serial.begin(115200); 
  pinMode(trigPin, OUTPUT); 
  pinMode(echoPin, INPUT); 
}

void loop() {

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);

  distanceCm = duration * SOUND_VELOCITY/2;

  Serial.print("Distance (cm): ");
  Serial.println(distanceCm);

  delay(1000);
}
```

#### Telemetria

Zmiana danych na wykresy. [Teleplot](https://github.com/nesnes/teleplot).

#### Podstawy oscyloskopu

Używanie oscyloskopu to kluczowa umiejętność w pracy z elektroniką. Oto krótka instrukcja:

1. Skonfiguruj i podłącz oscyloskop do źródła zasilania oraz do układu, którego sygnał chcesz analizować. Upewnij się, że masę oscyloskopu i masy układu są połączone.

2. Na osi poziomej, ustaw podstawę czasu tak, aby była odpowiednia dla badanego sygnału. Często zaczyna się od ustawienia na 1 ms / div i dostosowuje w zależności od potrzeb.

3. Na osi pionowej, ustaw skalę napięcia, aby zmierzyć sygnał w sposób odpowiedni. Może to być 1V / div lub inna wartość, w zależności od oczekiwanego zakresu.

4. Ustaw trigger. Trigger określa, kiedy oscyloskop ma zacząć pomiar sygnału. Może to być na przykład opadające zbocze, rosnące zbocze, lub inny warunek. Wartość wyzwalania można dostosować tak, aby chwycić sygnał w odpowiednim momencie.

5. Teraz możesz odczytywać sygnał na ekranie oscyloskopu. Zazwyczaj będzie to linia lub krzywa reprezentująca zmiany napięcia w czasie.

6. Oscyloskopy często mają funkcje do pomiaru częstotliwości, amplitudy, czasu trwania sygnału itp. Skorzystaj z tych funkcji, aby uzyskać bardziej precyzyjne informacje o sygnale.

7. Jeśli analizujesz bardziej skomplikowany sygnał, możesz użyć funkcji oscyloskopu, takich jak FFT (Fast Fourier Transform), aby przekształcić sygnał w dziedzinę częstotliwości.

9. Zapisz lub zrób zrzut ekranu, aby zachować dokumentację wyników pomiarów. To przyda się w późniejszej analizie lub jako dowód.

Po zakończeniu pracy, wyłącz oscyloskop, odłącz sondy i inne urządzenia.

Podstawy zasilacza laboratoryjnego:

- Ustaw U/I Proste :)

#### Bonus - Jak podłączyć diody led do ładowarki.

