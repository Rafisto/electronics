+++
date = '2022-11-25T10:00:00+01:00'

title = '1.2 Układy Logiczne'
+++

## 25.11.2022 – Układy logiczne

### Bramka Logiczna

Bramka logiczna – aktywny element elektroniczny, będący najczęściej układem scalonym, realizujący fizycznie pewną prostą funkcję logiczną , której argumenty oraz sama funkcja mogą przybierać wartości 0 albo 1. 

[Zob. Algebra Boole’a]

### Funkcje logiczne

Wyróżniamy trzy podstawowe funkcje logiczne, które mogą być realizowane elektronicznie:
- Koniunkcja AND
- Alternatywa OR
- Negacja NOT

Na gruncie tych trzech operacji logicznych możemy skonstruować nieco bardziej skomplikowane operatory:
- Negacja koniunkcji NAND
- Negacja alternatywy NOR
- Alternatywa wykluczająca XOR
- Negacja alternatywy wykluczającej XNOR

Wyróżniamy do tego dwie bramki, które są funkcjonalnie pełne, ponieważ możemy z nich tylko samych zbudować układ realizujący dowolną funkcję logiczną:
- NAND
- NOR


[Zob. W. Kurpiewski, Logika - wykłady i materiały dydaktyczne dla studentów Wydziału Humanistycznego AGH]

Dla układu z dwoma wejściami P oraz Q możemy sporządzić tzw. tablicę prawdy, będącą tabelą argumentów i wartości poszczególnych funkcji logicznych. Przy liczeniu operacji złożonych można utworzyć tabelę specyficzną dla układu i kolejno rozpisywać wartości logiczne.

| P   | Q   | P  AND Q | P OR  Q | NOT P | P NAND Q | P NOR Q | P XOR Q | P XNOR Q |
| --- | --- | -------- | ------- | ----- | -------- | ------- | ------- | -------- |
| 0   | 0   | 0        | 0       | 1     | 1        | 1       | 0       | 1        |
| 0   | 1   | 0        | 1       | 1     | 1        | 0       | 1       | 0        |
| 1   | 0   | 0        | 1       | 0     | 1        | 0       | 1       | 0        |
| 1   | 1   | 1        | 1       | 0     | 0        | 0       | 0       | 1        |

Przy projektowaniu układu realizującego funkcje złożone, możemy skorzystać z kilku praw logicznych, które mogą uprościć układ. Są to między innymi:

Pierwsze prawo de Morgana - negacja koniunkcji jest równoważna alternatywie negacji:
    
$\lnot\left(p\land q\right)\Longleftrightarrow(\lnot p\vee\lnot q)$

Drugie prawo de Morgana - negacja alternatywy jest równoważna koniunkcji negacji:

$\lnot\left(p\vee q\right)\Longleftrightarrow(\lnot p\land\lnot q)$

Prawo kontrapozycji (transpozycji) – równoważność implikacji prostej i przeciwstawnej:

$\left(p\Rightarrow q\right)\Longleftrightarrow\left(\lnot q\Rightarrow\lnot p\right)$

Prawo negacji implikacji - zaprzeczenie implikacji jest równoważne koniunkcji poprzednika i negacji następnika implikacji:

$\lnot\left(a\Longrightarrow b\right)\Longleftrightarrow(a\land\lnot b)$

