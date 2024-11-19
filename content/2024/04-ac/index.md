+++
date = '2024-10-29T10:00:00+01:00'
title = '3.4 Prąd Zmienny'
[params]
  math = true
+++

- [Prąd zmienny](#prąd-zmienny)
- [Charakterystyka prądu zmiennego](#charakterystyka-prądu-zmiennego)
  - [Równanie prądu przemiennego](#równanie-prądu-przemiennego)
  - [Wartość szczytowa, międzyszczytowa i skuteczna sygnału](#wartość-szczytowa-międzyszczytowa-i-skuteczna-sygnału)
    - [Wyprowadzenie wartości skutecznej prądu przemiennego sinusoidalnego](#wyprowadzenie-wartości-skutecznej-prądu-przemiennego-sinusoidalnego)
    - [Wyprowadzenie wartości skutecznej dla przebiegu prostokątnego](#wyprowadzenie-wartości-skutecznej-dla-przebiegu-prostokątnego)
- [Przesunięcie fazowe](#przesunięcie-fazowe)
  - [Konsekwencje przesunięcia fazowego](#konsekwencje-przesunięcia-fazowego)
  - [Kondensator i cewka](#kondensator-i-cewka)

## Prąd zmienny

Prąd zmienny to prąd elektryczny, którego kierunek i natężenie zmieniają się w czasie.

## Charakterystyka prądu zmiennego

- Amplituda
- Częstotliwość
- Okres

### Równanie prądu przemiennego

- Prąd przemienny sinusoidalny. Funkcja przebiegu: $U(t)=U_p\sin(\omega t + \varphi)$

[Średnia kwadratowa (w tym dla rozkładu ciągłego)](https://pl.wikipedia.org/wiki/%C5%9Arednia_kwadratowa) - miara statystyczna pozwalająca oszacować średnią wartość funkcji ciągłej, w szczególności gdy wartości funkcji są dodatnie i ujemne.

### Wartość szczytowa, międzyszczytowa i skuteczna sygnału

Wartość szczytowa - wartość maksymalna sygnału w okresie $T$.

Wartość międzyszczytowa - różnica między wartością szczytową a wartością minimalną w okresie $T$.

Wartość skuteczna - wartość prądu stałego, która wywołałaby takie samo ciepło w oporniku jak prąd zmienny w okresie $T$.

#### Wyprowadzenie wartości skutecznej prądu przemiennego sinusoidalnego

Prąd przemienny sinusoidalny: $I(t)=I_m\sin(\omega t)$

1. Najpierw obliczamy kwadrat prądu w funkcji czasu:

$$ I^2(t) = (I_m \cdot \sin(\omega t + \phi))^2 = I_m^2 \cdot \sin^2(\omega t + \phi) $$

2. Następnie obliczamy średnią wartość $I^2(t)$ w jednym okresie $T $:

$$ I_{\text{kw}}(t) = \frac{1}{T} \int_{0}^{T} I^2(t) \, dt = \frac{1}{T} \int_{0}^{T} I_m^2 \cdot \sin^2(\omega t + \phi) \, dt = \frac{I_m^2}{T} \int_{0}^{T} \sin^2(\omega t + \phi) \, dt $$

Wiemy że $\int_{0}^{T} \sin^{2}(\omega t + \phi) \, dt = \frac{T}{2}$.

<section>

Policzmy to:

Skoro $\cos(2x) = 1 - 2\sin^2(x)$, to $\sin^2(x) = \frac{1}{2} (1 - \cos(2x))$.

$$ \int \sin^2(t) dt = \int \frac{1}{2} (1 - \cos(2t)) dt = \frac{1}{2} \int 1 - \cos(2t) dt = \frac{1}{2} \left( t - \frac{1}{2} \sin(2t) \right) + C $$

</section>

Następnie:

$$ I_{\text{kw}} = \frac{I_m^2}{T} \cdot \frac{T}{2} = \frac{I_m^2}{2} $$

$$ I_{\text{RMS}} = \sqrt{I_{\text{kw}}} = \sqrt{\frac{I_m^2}{2}} = \frac{I_m}{\sqrt{2}} $$ 


#### Wyprowadzenie wartości skutecznej dla przebiegu prostokątnego

Niech $T=t_2-t_0$ oraz $t_1 \in (t_0, t_2)$, wtedy przebieg prostokątny możemy zapisać jako:

Przebieg prostokątny: $U(t)=\begin{cases} U & \text{dla } t_0 \leq t \leq t_1 \\ 0 & \text{dla } t_1 \leq t \leq t_2 \end{cases}$

1. Obliczamy kwadrat napięcia w funkcji czasu:

$$ U_{\text{kw}}(t) = \begin{cases} U^2 & \text{dla } t_0 \leq t \leq t_1 \\ 0 & \text{dla } t_1 \leq t \leq t_2 \end{cases} $$

2. Liczymy średnią wartość kwadratu napięcia w jednym okresie $T$:

$$ U_{\text{RMS}} = \frac{1}{T} \int_{t_0}^{t_2} U_{\text{kw}}(t) \, dt = \frac{1}{T} \int_{t_0}^{t_1} U^2 \, dt = \frac{U^2}{T} \int_{t_0}^{t_1} \, dt = \frac{U^2}{T} \cdot (t_1 - t_0) = U^2 \cdot \text{\% wypełnienia}$$

## Przesunięcie fazowe

W prądzie przemiennym sinusoidalnym, przesunięcie fazowe to różnica faz między napięciem a prądem, nie muszą one być w fazie.

### Konsekwencje przesunięcia fazowego

- Moc czynna, bierna i pozorna

### Kondensator i cewka

![img](https://www.cmm.gov.mo/eng/exhibition/secondfloor/moreinfo/2_4_4_PhaseShift.html)

- Kondensator: prąd wyprzedza napięcie o 90° -  pełne naładowanie lub rozładowanie kondensatora zajmuje określoną ilość czasu, prąd płynie zanim napięcie osiągnie swoją maksymalną wartość
- Cewka: prąd opóźnia napięcie o 90° - prąd płynie z opóźnieniem względem napięcia, ponieważ cewka ma swoje pole magnetyczne, które musi się zbudować

![alt text](phase-shift.png)

Określanie przesunięcia fazowego na podstawie wykresu wskazowego.

- Reguła CIUL: Kondensator (Prąd) (Napięcie) Cewka.