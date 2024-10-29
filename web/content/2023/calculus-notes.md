+++
date = '2023-10-07T10:00:00+01:00'

title = '2.1b Postawy granic, pochodnych i całek'
+++

## Podstawy granic, pochodnych, całek

### Funkcje

Funkcja $f: X\rightarrow Y$ to przyporządkowanie, które każdemu elementowi $x\in X$ odpowiada dokładnie jeden element zbioru $y\in Y$:
- Zbiór $X$ to dziedzina funkcji.
- Elementy $e\in X$ to argumenty qfunkcji.
- Zbiór $Y$ to przeciwdziedzina funkcji.
- Elementy $y\in Y$ które sa przyporządkowane przynajmniej jednemu argumentowi $x$ to zbiór wartości funkcji.

Z1. Narysuj funkcje $f: X\rightarrow Y$ przyporządkowującą liczbom $\{1,2,3\}$ liczby $\{4,5,6\}$.

### Kwantyfikatory

- $\forall$ - dla każdego
- $\exists$ - istnieje taki


Z2. Zdefiniuj za pomocą kwantyfikatorów liczby pierwsze.
Liczba $p\in\mathbb{N}$ jest pierwsza $\iff \forall{n\in\mathbb{N}}: (n|p)\implies (n=1)\lor(n=p)$ 

(Cauchy) Definicja granicy funkcji w punkcie:

$$lim_{x\rightarrow x_0} = g \iff \forall_{\varepsilon>0} \exists_{\delta>0} \forall_{x\in\mathbb{R}} |x-x_0|<\delta \implies |f(x)-g|<\epsilon$$

P1. Pokazać graficznie jak funkcja $f(x)=\frac{1}{x}$ dąży w $\infty$ do $0$

### Pochodne

Definicja pochodnej:

$\frac{dy}{dx} = lim_{h\rightarrow 0} \frac{f(x)+f(x+h)}{h}$

Wyprowadzenie pochodnej z definicji:

$\frac{d(x^2)}{dx} = lim_{h\rightarrow 0}$
$\frac{f(x)+f(x+h)}{h} = lim_{h\rightarrow 0}$
$\frac{(x+h)^2+x^2}{h} = lim_{h\rightarrow 0}$
$\frac{2x^2+2xh+h^2}{h} = \left[\frac{1}{0}\right]$
$= lim_{h\rightarrow 0} \frac{2x^2}{h}+\frac{2xh}{h}+\frac{h^2}{h}=0+2x+0=2x$ 

Z3. Oblicz $lim_{x\rightarrow 0} \frac{1}{x}$. Skorzystaj z definicji granicy prawostronnej i lewostronnej.

Interpretacja geometryczna pochodnej - styczna do wykresu funkcji.

### Całki

P2. Narysuj dowolną funkcję ciągłą która nie jest funkcją liniową.

Kres górny funkcji to najmniejsza liczba ograniczająca te funkcję z góry:

$$M = \sup f(x)$$

Kres dolny funkcji to najmniejsza liczba ograniczająca te funkcję z dołu:

$$m = \inf f(x)$$

Podziałem $P$ przedziału $[a,b]$ nazywa się każdy (ściśle) rosnący ciąg skończony ${\displaystyle (p_{0},\dots ,p_{n})}$ elementów nazywanych punktami podziału tego przedziału, w którym pierwszy i ostatni wyraz ciągu wskazują odpowiednio początek i koniec przedziału.

Każda para „sąsiednich” punktów podziału ${\displaystyle (p_{i-1},p_{i})}$ wyznacza podprzedział ${\displaystyle P_{i}=[p_{i-1},p_{i}]}$ o długości ${\displaystyle |P_{i}|=\Delta p_{i}:=p_{i}-p_{i-1}}$ dla ${\displaystyle i=1,\dots n.}$

Sumę częściową Riemanna funkcji $f$ nazywamy liczbę:

$$R_{f,P(q_1,q_2,\dots q_n)} = \sum_{i=1}^{n} f(q_i)\cdot\Delta p_i$$

Funkcję $f$ nazywa się całkowalną w sensie Riemanna lub krótko R-całkowalną, jeśli dla dowolnego ciągu normalnego ${\displaystyle (P^{k})}$ podziałów przedziału $[a,b]$, istnieje (niezależna od wyboru punktów pośrednich) granica:

$$R_{f}=\lim_{k\to \infty} R_{{f,P^{k}\left(q_{1}^{k},\dots ,q_{{n_{k}}}^{k}\right)}}$$

nazywana wtedy **całką Riemanna** tej funkcji. $R_f$ Oznaczamy ją jako $\int f(x) dx$

![obraz](https://github.com/ALO-PWr-Elektronika/Classes/assets/88141065/6e4e60d5-9964-46d1-a2f1-103ce74a67e2)

Z4. Oblicz $\int x^2 dx$, $\int sin(x) dx$

### Równania różniczkowe

Równanie które określa zależność między funkcją, a jej pochodnymi. Rozwiązaniem równania są wszystkie funkcje które spełniają daną zależność.

Przykład. Rozwiązanie RR pierwszego stopnia (standardową metodą obustronnego całkowania).
$$y'=y\implies \frac{dy}{dx} = y\implies \frac{dy}{y} = dx\implies$$
$$\int \frac{1}{y} dy = \int dx \implies ln(y)=x + C\implies y=C\cdot e^x$$ 

Z5. Rozważmy ruch harmoniczny w którym siła $F=m\frac{d^2x}{dx^2}=-kx$, jest zależna od $x$. Wyprowadź wzór na $x(t)$.

Po zapoznaniu się z powyższym zachęcam do przeczytania kilku przykładów:
- [(RW-E01-22) Wyprowadzenie równań ruchu oraz dwa różne równanie różniczkowe](https://github.com/ALO-PWr-Elektronika/Classes/blob/master/2022/other/differential.equations.md)
