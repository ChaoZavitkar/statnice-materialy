# Okruh 4 – Funkce, polynomy, numerické řešení rovnic
**KI/SZZTP – Teoretické základy informatiky**

---

## 1. Reálná funkce jedné reálné proměnné

### Co je to funkce?

Funkce `f` je **pravidlo**, které každému číslu `x` z definičního oboru přiřadí právě jedno číslo `y`.

Píšeme: `y = f(x)`

- `x` = **argument** (nezávislá proměnná)
- `y` = **funkční hodnota** (závislá proměnná)

> **Klíčové:** Každému `x` přísluší **nejvýše jedno** `y`. Naopak více různých `x` může mít stejné `y`.

---

### Definiční obor (D(f)) a Obor hodnot (H(f))

| Pojem | Definice | Příklad pro `f(x) = √x` |
|---|---|---|
| **Definiční obor** D(f) | Množina všech `x`, pro která je `f(x)` definována | D(f) = ⟨0, +∞) |
| **Obor hodnot** H(f) | Množina všech hodnot, které `f(x)` může nabývat | H(f) = ⟨0, +∞) |

**Jak najít definiční obor — praktická pravidla:**

1. **Zlomek:** jmenovatel ≠ 0
   - `f(x) = 1/(x−2)` → D(f) = ℝ \ {2}
2. **Sudá odmocnina:** výraz pod odmocninou ≥ 0
   - `f(x) = √(x−3)` → D(f) = ⟨3, +∞)
3. **Logaritmus:** argument > 0
   - `f(x) = ln(x+1)` → D(f) = (−1, +∞)
4. **Kombinace:** uplatni všechna pravidla najednou

> **Příklad:** `f(x) = √(x−1) / (x−3)`
>
> - Odmocnina: `x − 1 ≥ 0` → `x ≥ 1`
> - Jmenovatel: `x − 3 ≠ 0` → `x ≠ 3`
> - **D(f) = ⟨1, 3) ∪ (3, +∞)**

---

### Graf funkce

Graf funkce `f` je množina všech bodů `[x, f(x)]` v rovině, kde `x ∈ D(f)`.

**Základní grafické vlastnosti:**

| Vlastnost | Definice | Na grafu poznáme |
|---|---|---|
| **Sudá funkce** | `f(−x) = f(x)` | Souměrná podle osy `y` |
| **Lichá funkce** | `f(−x) = −f(x)` | Středově souměrná podle počátku |
| **Rostoucí** | `x₁ < x₂ ⟹ f(x₁) < f(x₂)` | Graf "stoupá" zleva doprava |
| **Klesající** | `x₁ < x₂ ⟹ f(x₁) > f(x₂)` | Graf "klesá" zleva doprava |
| **Periodická** | `f(x+T) = f(x)` | Vzor se opakuje s periodou `T` |

---

### Limita funkce

#### Intuitivní chápání

Limita `lim[x→a] f(x) = L` znamená: **čím blíže je `x` k hodnotě `a`, tím blíže je `f(x)` k hodnotě `L`.**

Důležité: v bodě `a` nemusí být funkce ani definována!

#### Výpočet limit — nejčastější situace

**1. Přímé dosazení (funkce je spojitá):**
```
lim[x→2] (x² + 3x) = 4 + 6 = 10
```

**2. Neurčitý výraz `0/0` — rozložení na součin:**
```
         x² − 4        (x−2)(x+2)
lim[x→2] ——————— = lim ——————————— = lim (x+2) = 4
          x − 2         (x−2)
```

**3. Neurčitý výraz `∞/∞` — vydělíme nejvyšší mocninou:**
```
         3x² + 2x        3 + 2/x
lim[x→∞] ————————— = lim ————————— = 3/2
          2x² − 1        2 − 1/x²
```

**4. Jednostranné limity:**
- `lim[x→a⁺]` = limita zprava
- `lim[x→a⁻]` = limita zleva
- Limita existuje ⟺ obě jednostranné limity jsou si rovny

> **Příklad:** `f(x) = |x| / x`
> - zprava: `lim[x→0⁺] = 1`
> - zleva: `lim[x→0⁻] = −1`
> - **Limita neexistuje** (jednostranné limity jsou různé)

#### Důležité limity (nutno znát):

```
lim[x→0] sin(x)/x = 1

lim[x→∞] (1 + 1/x)ˣ = e ≈ 2.718

lim[x→0] (eˣ − 1)/x = 1
```

---

### Spojitost funkce

Funkce `f` je **spojitá v bodě `a`**, pokud platí všechny tři podmínky:

1. `f(a)` je definována
2. `lim[x→a] f(x)` existuje
3. `lim[x→a] f(x) = f(a)`

Neformálně: **graf lze nakreslit bez zvednutí tužky**.

**Typy nespojitostí:**

| Typ | Popis | Příklad |
|---|---|---|
| **Odstranitelná** | Limita existuje, ale ≠ f(a) nebo f(a) neexistuje | `sin(x)/x` v bodě 0 |
| **Skok** | Jednostranné limity jsou různé | `sgn(x)` v bodě 0 |
| **Neohraničená** | Limita je ±∞ | `1/x` v bodě 0 |

> **Základní věta:** Součet, rozdíl, součin a podíl (≠ 0 ve jmenovateli) spojitých funkcí je opět spojitá funkce.
> Polynomy jsou spojité všude. Racionální funkce jsou spojité všude, kde jsou definovány.

---

## 2. Polynomy

### Definice

Polynom (mnohočlen) stupně `n` je funkce ve tvaru:

```
p(x) = aₙxⁿ + aₙ₋₁xⁿ⁻¹ + ... + a₁x + a₀
```

kde `aₙ ≠ 0` a koeficienty `a₀, a₁, ..., aₙ ∈ ℝ`.

- `aₙ` = **vedoucí koeficient**
- `n` = **stupeň polynomu** (deg p)

**Příklady:**
- `p(x) = 3x³ − 2x + 5` → stupeň 3
- `p(x) = 7` → stupeň 0 (konstantní polynom)

---

### Základní vlastnosti polynomů

1. **Definiční obor:** vždy celá ℝ
2. **Spojitost:** spojité na celé ℝ
3. **Počet kořenů:** polynom stupně `n` má nejvýše `n` reálných kořenů (a přesně `n` kořenů v ℂ, počítáme-li násobnosti)
4. **Dělení polynomů:** pro každé `p(x)` a `d(x) ≠ 0` existují jednoznačně `q(x)` (podíl) a `r(x)` (zbytek) tak, že:
   ```
   p(x) = d(x) · q(x) + r(x),   deg(r) < deg(d)
   ```
5. **Bezoutova věta:** `x = a` je kořen `p(x)` právě tehdy, když `(x − a)` dělí `p(x)` beze zbytku, tj. `p(a) = 0`.

---

### Hornerovo schéma

**Problém:** Jak efektivně vyčíslit hodnotu polynomu `p(x₀)`?

**Naivní přístup** (přímé dosazení) vyžaduje mnoho násobení. Pro polynom stupně `n` potřebujeme `n + (n−1) + ... + 1 = n(n+1)/2` násobení.

**Hornerovo schéma** to zvládne s pouhými `n` násobeními a `n` sčítáními — výrazně efektivnější!

#### Princip (přepis pomocí vnořených závorek):

```
p(x) = aₙxⁿ + aₙ₋₁xⁿ⁻¹ + ... + a₁x + a₀
     = (...((aₙ · x + aₙ₋₁) · x + aₙ₋₂) · x + ...) · x + a₀
```

#### Algoritmus — tabulkový postup:

Pro `p(x) = aₙxⁿ + ... + a₀` a bod `x₀`:

| krok | koeficienty |
|------|-------------|
| Začínáme: | `b_n = aₙ` |
| Opakujeme: | `b_k = b_{k+1} · x₀ + aₖ` pro `k = n−1, n−2, ..., 0` |
| Výsledek: | `p(x₀) = b₀` |

#### Příklad: `p(x) = 2x³ − 3x² + x − 5`, chceme `p(2)`:

Koeficienty: `[2, −3, 1, −5]`, vyčíslujeme v `x₀ = 2`

```
b₃ = 2
b₂ = 2 · 2 + (−3) = 4 − 3 = 1
b₁ = 1 · 2 + 1     = 2 + 1 = 3
b₀ = 3 · 2 + (−5)  = 6 − 5 = 1
```

**p(2) = 1** ✓ (ověřte přímým dosazením: 2·8 − 3·4 + 2 − 5 = 16 − 12 + 2 − 5 = 1 ✓)

> **Bonus:** Hornerovo schéma lze použít i pro **dělení polynomu** výrazem `(x − x₀)`. Koeficienty `b_n, ..., b_1` jsou koeficienty podílu, `b₀` je zbytek.

---

## 3. Numerické řešení nelineárních rovnic

**Cíl:** Najít kořeny rovnice `f(x) = 0`, tj. hodnoty `x` pro které `f(x) = 0`.

**Proč numericky?** Pro polynomy stupně ≥ 5 a obecné nelineární funkce neexistuje analytický vzorec. Používáme iterační metody, které postupně zpřesňují odhad.

---

### Metoda půlení intervalu (bisekce)

#### Předpoklady:
- Funkce `f` je **spojitá** na `[a, b]`
- Platí `f(a) · f(b) < 0` (funkce mění znaménko → kořen leží uvnitř)

Z **Bolzanovy věty** plyne: existuje alespoň jeden kořen `ξ ∈ (a, b)`.

#### Algoritmus:

```
1. Zvol interval [a, b] tak, že f(a)·f(b) < 0
2. Opakuj:
   a. Vypočti střed:  c = (a + b) / 2
   b. Pokud f(c) = 0: nalezen přesný kořen, konec
   c. Pokud f(a)·f(c) < 0: nový interval = [a, c]  (kořen je v levé polovině)
   d. Jinak:            nový interval = [c, b]  (kořen je v pravé polovině)
3. Zastav, když |b − a| < ε  (dosáhneme požadované přesnosti)
```

#### Vizualizace:

```
f(a) > 0                              f(b) < 0
  |                                      |
  a --------c₁------- b    → kořen vlevo od c₁
  a ---c₂-- c₁                → kořen vpravo od c₂
         c₂ --c₃-- c₁         → ...
```

#### Konvergence:

Po `n` krocích je chyba maximálně:
```
|ξ − cₙ| ≤ (b − a) / 2ⁿ
```

Počet kroků pro přesnost `ε`:
```
n ≥ log₂((b − a) / ε)
```

#### Příklad: `f(x) = x³ − x − 2 = 0` na `[1, 2]`

```
f(1) = 1 − 1 − 2 = −2 < 0
f(2) = 8 − 2 − 2 = +4 > 0  → podmínka splněna

krok 1: c = 1.5,  f(1.5) = 3.375 − 1.5 − 2 = −0.125 < 0
        → nový interval: [1.5, 2]

krok 2: c = 1.75, f(1.75) = 5.359 − 1.75 − 2 ≈ 1.609 > 0
        → nový interval: [1.5, 1.75]

krok 3: c = 1.625, ...
```

Kořen se blíží `x ≈ 1.5214`.

**Výhody:** jednoduchá implementace, garantovaná konvergence
**Nevýhody:** pomalá konvergence (lineární), nutnost znát interval se změnou znaménka

---

### Newtonova metoda (metoda tečen)

#### Idea:

V aktuálním odhadu `xₖ` nahradíme funkci `f` její **tečnou** a jako nový odhad `xₖ₊₁` vezmeme průsečík tečny s osou `x`.

#### Odvození:

Tečna v bodě `(xₖ, f(xₖ))` má rovnici:
```
y − f(xₖ) = f'(xₖ) · (x − xₖ)
```

Průsečík s osou `x` (y = 0):
```
0 = f(xₖ) + f'(xₖ) · (xₖ₊₁ − xₖ)
```

```
           f(xₖ)
xₖ₊₁ = xₖ − ————————
            f'(xₖ)
```

> **Iterační vzorec Newtonovy metody:**
> ```
>            f(xₙ)
> xₙ₊₁ = xₙ − ———————
>             f'(xₙ)
> ```

#### Algoritmus:

```
1. Zvol počáteční odhad x₀
2. Opakuj:
   a. Vypočti x_{n+1} = x_n − f(x_n)/f'(x_n)
   b. Zastav, když |x_{n+1} − x_n| < ε  (nebo |f(x_{n+1})| < ε)
```

#### Příklad: `f(x) = x² − 2 = 0` (hledáme √2), `x₀ = 1`

```
f(x)  = x² − 2
f'(x) = 2x

         x₀² − 2    1 − 2     1
x₁ = x₀ − ———————— = 1 − ———— = 1 + — = 1.5
            2x₀         2       2

          1.5² − 2      2.25 − 2
x₂ = 1.5 − ————————— = 1.5 − ————————— = 1.5 − 0.0833... = 1.4167
             2·1.5          3

x₃ ≈ 1.4142...
```

Skutečná hodnota √2 ≈ 1.41421356... — velmi rychlá konvergence!

#### Konvergence:

Newtonova metoda má **kvadratickou konvergenci** — počet přesných číslic se přibližně zdvojnásobí v každém kroku!

```
Bisekce:  1, 2, 3, 4, 5... přesných číslic (pomalá, lineární)
Newton:   1, 2, 4, 8, 16.. přesných číslic (rychlá, kvadratická)
```

#### Podmínky a problémy:

| Situace | Problém |
|---|---|
| `f'(xₙ) = 0` | Metoda selhává — dělení nulou |
| Špatný počáteční odhad | Může divergovat nebo skočit na jiný kořen |
| Funkce s více kořeny | Nemusíme najít ten, který chceme |

**Zaručená konvergence:** pokud na `[a, b]` platí:
- `f(a)·f(b) < 0`
- `f'(x) ≠ 0` (monotónní)
- `f''(x)` nemění znaménko (konvexní nebo konkávní)

a `x₀` splňuje `f(x₀)·f''(x₀) > 0`, pak Newtonova metoda konverguje.

---

## Srovnávací tabulka metod

| Vlastnost | Bisekce | Newton |
|---|---|---|
| **Počáteční podmínka** | interval [a,b] se změnou znaménka | jeden bod x₀, znát f'(x) |
| **Konvergence** | vždy (při splnění podmínek) | ne vždy, závisí na x₀ |
| **Rychlost** | lineární — pomalá | kvadratická — velmi rychlá |
| **Výpočet derivace** | není potřeba | potřeba f'(x) |
| **Implementační složitost** | jednoduchá | střední |
| **Použití** | když nevíme kde začít | když máme dobrý odhad |

---
