# Okruh 5 – Diferenciální a integrální počet
**KI/SZZTP – Teoretické základy informatiky**

---

## 1. Derivace funkce

### Co je derivace?

Derivace funkce `f` v bodě `x₀` je **okamžitá rychlost změny** funkce v tomto bodě.

#### Formální definice:

```
         f(x₀ + h) − f(x₀)
f'(x₀) = lim ———————————————
         h→0         h
```

Pokud limita existuje, říkáme, že funkce `f` je **diferencovatelná** v bodě `x₀`.

#### Alternativní zápisy:

```
f'(x)  nebo  df/dx  nebo  dy/dx  nebo  Df(x)
```

---

### Geometrický význam derivace

**Derivace = směrnice tečny** ke grafu funkce v daném bodě.

```
Rovnice tečny v bodě [x₀, f(x₀)]:

y − f(x₀) = f'(x₀) · (x − x₀)

nebo přepsáno:

y = f(x₀) + f'(x₀) · (x − x₀)
```

#### Vizualizace:

```
      y
      |     
      |        ╱ tečna (směrnice = f'(x₀))
      |      ╱
      |    ╱
      |  ● (x₀, f(x₀))
      | ╱ graf funkce
      |╱
      ————————————————— x
```

**Fyzikální interpretace:**
- Poloha s(t) → rychlost v(t) = s'(t)
- Rychlost v(t) → zrychlení a(t) = v'(t) = s''(t)

---

### Základní vzorce pro derivace

**Musíš znát nazpaměť:**

| Funkce | Derivace | Poznámka |
|--------|----------|----------|
| `c` (konstanta) | `0` | Konstanta zmizí |
| `x` | `1` | |
| `xⁿ` | `n·xⁿ⁻¹` | Mocninná funkce |
| `eˣ` | `eˣ` | Jediná funkce = své derivaci |
| `aˣ` | `aˣ · ln(a)` | Pro a > 0, a ≠ 1 |
| `ln(x)` | `1/x` | Pro x > 0 |
| `logₐ(x)` | `1/(x·ln(a))` | |
| `sin(x)` | `cos(x)` | |
| `cos(x)` | `−sin(x)` | |
| `tan(x)` | `1/cos²(x) = sec²(x)` | |
| `arcsin(x)` | `1/√(1−x²)` | |
| `arctan(x)` | `1/(1+x²)` | |
| `√x` | `1/(2√x)` | Speciální případ xⁿ |

---

### Pravidla pro derivování

#### 1. Konstantní násobek:
```
[c·f(x)]' = c·f'(x)
```

#### 2. Součet/rozdíl:
```
[f(x) ± g(x)]' = f'(x) ± g'(x)
```

#### 3. Součin (produktové pravidlo):
```
[f(x)·g(x)]' = f'(x)·g(x) + f(x)·g'(x)
```

**Mnemotechnická pomůcka:** "první′ · druhá + první · druhá′"

#### 4. Podíl (quotientové pravidlo):
```
[f(x)/g(x)]' = [f'(x)·g(x) − f(x)·g'(x)] / [g(x)]²
```

**Mnemotechnická pomůcka:** "dole na druhou dolů, čitatel: vrch′ · spod − vrch · spod′"

#### 5. Složená funkce (řetízkové pravidlo):
```
[f(g(x))]' = f'(g(x)) · g'(x)
```

Nejdřív derivujeme "vnější", pak "vnitřní" a násobíme.

---

### Příklady výpočtu derivací

#### Příklad 1: Mocninná funkce
```
f(x) = 3x⁴ − 2x² + 5x − 7

f'(x) = 3·4x³ − 2·2x + 5·1 − 0
      = 12x³ − 4x + 5
```

#### Příklad 2: Součin
```
f(x) = x² · sin(x)

f'(x) = (x²)' · sin(x) + x² · (sin(x))'
      = 2x · sin(x) + x² · cos(x)
```

#### Příklad 3: Podíl
```
f(x) = (x+1)/(x−2)

f'(x) = [(x+1)' · (x−2) − (x+1) · (x−2)'] / (x−2)²
      = [1 · (x−2) − (x+1) · 1] / (x−2)²
      = [x − 2 − x − 1] / (x−2)²
      = −3 / (x−2)²
```

#### Příklad 4: Složená funkce
```
f(x) = sin(x²)

Vnější: sin(u),  u = x²
Vnější': cos(u)
Vnitřní': 2x

f'(x) = cos(x²) · 2x = 2x · cos(x²)
```

#### Příklad 5: Kombinace pravidel
```
f(x) = e^(x²) · ln(x)

Použijeme produktové pravidlo:
f'(x) = [e^(x²)]' · ln(x) + e^(x²) · [ln(x)]'

Pro e^(x²) použijeme řetízkové pravidlo:
[e^(x²)]' = e^(x²) · 2x

f'(x) = e^(x²) · 2x · ln(x) + e^(x²) · (1/x)
      = e^(x²) · [2x·ln(x) + 1/x]
```

---

### Vyšší derivace

**Druhá derivace** f''(x) = derivace první derivace
**Třetí derivace** f'''(x) = derivace druhé derivace
...

Obecně: **n-tá derivace** značíme `f⁽ⁿ⁾(x)`

**Příklad:**
```
f(x) = x⁴

f'(x)   = 4x³
f''(x)  = 12x²
f'''(x) = 24x
f⁽⁴⁾(x) = 24
f⁽⁵⁾(x) = 0  (a všechny další)
```

---

## 2. Aplikace derivací

### Lokální extrémy funkce

#### Definice:

- **Lokální maximum** v `x₀`: `f(x₀) ≥ f(x)` pro všechna `x` v okolí `x₀`
- **Lokální minimum** v `x₀`: `f(x₀) ≤ f(x)` pro všechna `x` v okolí `x₀`

#### Nutná podmínka pro extrém:

**Fermatova věta:** Pokud `f` má v `x₀` lokální extrém a existuje `f'(x₀)`, pak:
```
f'(x₀) = 0
```

Body, kde `f'(x) = 0` nebo `f'(x)` neexistuje, nazýváme **stacionární body** nebo **kritické body**.

> **Důležité:** `f'(x₀) = 0` je nutná podmínka, ne postačující! Ne každý stacionární bod je extrém.

#### Postačující podmínka — test první derivace:

V okolí `x₀`:
- `f'(x)` mění znaménko z `+` na `−` → **lokální maximum**
- `f'(x)` mění znaménko z `−` na `+` → **lokální minimum**
- `f'(x)` nemění znaménko → **není extrém** (inflexní bod)

```
Maximum:    ╱‾╲     f' > 0 | f' < 0
Minimum:    ╲_╱     f' < 0 | f' > 0
Sedlo:      ╱ ╱     f' > 0 | f' > 0  (není extrém)
```

#### Postačující podmínka — test druhé derivace:

Pokud `f'(x₀) = 0`:
- `f''(x₀) > 0` → **lokální minimum** (funkce je "konvexní")
- `f''(x₀) < 0` → **lokální maximum** (funkce je "konkávní")
- `f''(x₀) = 0` → **test selhává**, použij test první derivace

#### Algoritmus hledání lokálních extrémů:

```
1. Najdi f'(x)
2. Řeš rovnici f'(x) = 0 → dostaneš kritické body
3. Pro každý kritický bod x₀:
   a. Použij test druhé derivace, nebo
   b. Použij test první derivace (znaménko f' vlevo/vpravo od x₀)
4. Vyhodnoť, zda je maximum, minimum nebo sedlo
```

#### Příklad: `f(x) = x³ − 3x² − 9x + 5`

```
1. f'(x) = 3x² − 6x − 9

2. f'(x) = 0
   3x² − 6x − 9 = 0
   x² − 2x − 3 = 0
   (x − 3)(x + 1) = 0
   → x₁ = −1,  x₂ = 3

3. Test druhé derivace:
   f''(x) = 6x − 6

   V x₁ = −1:  f''(−1) = −6 − 6 = −12 < 0  → maximum
   V x₂ = 3:   f''(3)  = 18 − 6 = 12 > 0   → minimum

4. Výsledek:
   Lokální maximum v x = −1,  f(−1) = −1 + 3 + 9 + 5 = 16
   Lokální minimum v x = 3,   f(3)  = 27 − 27 − 27 + 5 = −22
```

---

### Navazování křivek

**Problém:** Jak hladce spojit dvě křivky (např. části silnice, kolejnice)?

#### Podmínky hladkosti:

Nechť máme dvě funkce `f₁(x)` a `f₂(x)`, které se mají spojit v bodě `x₀`:

| Stupeň hladkosti | Podmínky | Význam |
|------------------|----------|--------|
| **C⁰ spojitost** | `f₁(x₀) = f₂(x₀)` | Křivky se dotýkají (bez skoku) |
| **C¹ spojitost** | `f₁(x₀) = f₂(x₀)` a `f₁'(x₀) = f₂'(x₀)` | Stejná tečna (bez "zlomu") |
| **C² spojitost** | Navíc `f₁''(x₀) = f₂''(x₀)` | Stejná křivost (hladký přechod) |

```
C⁰:  ‾‾╲_    (skok zakázán, zlom povolen)
        ╲
C¹:  ‾‾‾╲    (hladký přechod, změna křivosti povolena)
        ╲
C²:  ‾‾‾╲    (dokonale hladký přechod)
       ╲
```

#### Příklad: Spojení parabolických oblouků

Máme `f₁(x) = ax² + bx + c` na `[0, 1]` a chceme navázat `f₂(x) = dx² + ex + f` v bodě `x = 1` s C¹ hladkostí.

Podmínky:
```
f₁(1) = f₂(1)     →  a + b + c = d + e + f
f₁'(1) = f₂'(1)   →  2a + b = 2d + e
```

Tyto rovnice nám dají vztahy mezi koeficienty.

**Praktické použití:** návrh silnic, kolejnic, animačních křivek (spline), designu karosérií...

---

## 3. Primitivní funkce (neurčitý integrál)

### Definice

Funkce `F(x)` je **primitivní funkcí** k funkci `f(x)` na intervalu `I`, pokud platí:
```
F'(x) = f(x)  pro všechna x ∈ I
```

**Neurčitý integrál:**
```
∫ f(x) dx = F(x) + C
```

kde `C` je **integrační konstanta** (libovolná reálná konstanta).

> **Důležité:** Primitivní funkce není určena jednoznačně — liší se o konstantu.

---

### Základní vzorce pro integrály

**Musíš znát nazpaměť:**

| Funkce f(x) | Primitivní funkce ∫f(x)dx | Poznámka |
|-------------|---------------------------|----------|
| `xⁿ` (n≠−1) | `xⁿ⁺¹/(n+1) + C` | Mocninná funkce |
| `1/x` | `ln|x| + C` | Důležité: absolutní hodnota! |
| `eˣ` | `eˣ + C` | |
| `aˣ` | `aˣ/ln(a) + C` | Pro a > 0, a ≠ 1 |
| `sin(x)` | `−cos(x) + C` | Pozor na znaménko! |
| `cos(x)` | `sin(x) + C` | |
| `1/cos²(x)` | `tan(x) + C` | |
| `1/√(1−x²)` | `arcsin(x) + C` | |
| `1/(1+x²)` | `arctan(x) + C` | |

---

### Pravidla pro integrování

#### 1. Konstantní násobek:
```
∫ c·f(x) dx = c · ∫ f(x) dx
```

#### 2. Součet/rozdíl:
```
∫ [f(x) ± g(x)] dx = ∫ f(x) dx ± ∫ g(x) dx
```

#### 3. Substituce (záměna proměnné):

Pro integrál tvaru `∫ f(g(x))·g'(x) dx`:

```
Substituce: u = g(x)
            du = g'(x) dx

∫ f(g(x))·g'(x) dx = ∫ f(u) du
```

Po integraci vrátíme zpět: `u → g(x)`

**Příklad:**
```
∫ 2x·eˣ² dx

Substituce: u = x²,  du = 2x dx

∫ eˣ² · 2x dx = ∫ eᵘ du = eᵘ + C = eˣ² + C
```

#### 4. Per partes (metoda integrace per partes):

Pro integrál tvaru `∫ u·v' dx`:

```
∫ u·v' dx = u·v − ∫ u'·v dx
```

**Volba:** vhodně zvolíme `u` (co derivovat) a `v'` (co integrovat).

**Pravidlo LIATE** pro volbu `u`:
- **L**ogaritmus
- **I**nverse trig (arcsin, arctan...)
- **A**lgebraická (polynomy)
- **T**rigonometrická (sin, cos...)
- **E**xponenciální (eˣ, aˣ)

**Příklad:**
```
∫ x·eˣ dx

u = x     →  u' = 1
v' = eˣ   →  v = eˣ

∫ x·eˣ dx = x·eˣ − ∫ 1·eˣ dx
          = x·eˣ − eˣ + C
          = eˣ(x − 1) + C
```

---

### Příklady výpočtu neurčitých integrálů

#### Příklad 1: Mocninná funkce
```
∫ (3x² − 4x + 5) dx = 3·x³/3 − 4·x²/2 + 5x + C
                     = x³ − 2x² + 5x + C
```

#### Příklad 2: Substituce
```
∫ sin(3x) dx

Substituce: u = 3x,  du = 3dx  →  dx = du/3

∫ sin(u) · (du/3) = (1/3) ∫ sin(u) du
                  = (1/3)·(−cos(u)) + C
                  = −(1/3)cos(3x) + C
```

#### Příklad 3: Složitější substituce
```
∫ x/√(x²+1) dx

Substituce: u = x² + 1,  du = 2x dx  →  x dx = du/2

∫ x/√(x²+1) dx = ∫ 1/√u · (du/2)
               = (1/2) ∫ u⁻¹/² du
               = (1/2) · 2u¹/² + C
               = √u + C
               = √(x²+1) + C
```

---

## 4. Určitý integrál

### Definice a geometrický význam

**Určitý integrál** funkce `f(x)` od `a` do `b`:
```
∫ᵃᵇ f(x) dx
```

**Geometrický význam:** 
- Pokud `f(x) ≥ 0` na `[a, b]`: **plocha pod grafem** funkce
- Pokud `f(x) < 0`: plocha se počítá se **záporným znaménkem**

```
      y
      |     
      |  ╱‾‾╲  ← f(x) > 0
      | ╱    ╲  plocha S = ∫ᵃᵇ f(x) dx
      |╱______╲___
      a        b  x
```

---

### Newton-Leibnizova formule (základní věta kalkulu)

Pokud `F(x)` je primitivní funkce k `f(x)`, pak:

```
∫ᵃᵇ f(x) dx = F(b) − F(a) = [F(x)]ᵃᵇ
```

**Postup výpočtu:**
1. Najdi primitivní funkci `F(x)` k `f(x)`
2. Vypočti `F(b) − F(a)`

> **Důležité:** Integrační konstanta `C` se odečte, takže ji nemusíme psát u určitého integrálu.

---

### Vlastnosti určitého integrálu

```
1. ∫ᵃᵃ f(x) dx = 0

2. ∫ᵃᵇ f(x) dx = −∫ᵇᵃ f(x) dx

3. ∫ᵃᵇ f(x) dx = ∫ᵃᶜ f(x) dx + ∫ᶜᵇ f(x) dx  (aditivita)

4. ∫ᵃᵇ [f(x) + g(x)] dx = ∫ᵃᵇ f(x) dx + ∫ᵃᵇ g(x) dx

5. ∫ᵃᵇ c·f(x) dx = c · ∫ᵃᵇ f(x) dx
```

---

### Příklady výpočtu určitých integrálů

#### Příklad 1: Polynom
```
∫₀² (x² + 2x) dx = [x³/3 + x²]₀²
                 = (8/3 + 4) − (0 + 0)
                 = 8/3 + 12/3
                 = 20/3
```

#### Příklad 2: Exponenciální funkce
```
∫₀¹ eˣ dx = [eˣ]₀¹ = e¹ − e⁰ = e − 1 ≈ 1.718
```

#### Příklad 3: Trigonometrická funkce
```
∫₀^(π/2) sin(x) dx = [−cos(x)]₀^(π/2)
                    = −cos(π/2) − (−cos(0))
                    = 0 − (−1)
                    = 1
```

#### Příklad 4: Se substitucí
```
∫₁⁴ x/√x dx = ∫₁⁴ x^(1/2) dx
             = [x^(3/2) / (3/2)]₁⁴
             = (2/3)[x^(3/2)]₁⁴
             = (2/3)(8 − 1)
             = 14/3
```

---

## 5. Aplikace určitého integrálu

### Objem rotačního tělesa

Když funkci `f(x)` na intervalu `[a, b]` **rotujeme kolem osy `x`**, vznikne **rotační těleso**.

**Vzorec pro objem:**
```
V = π ∫ᵃᵇ [f(x)]² dx
```

#### Odvození (neformální):

- Vezmeme tenký "plátok" v bodě `x` o tloušťce `dx`
- Po rotaci vznikne tenký válec o poloměru `r = f(x)`
- Objem plátku: `dV = π·r²·dx = π·[f(x)]²·dx`
- Celkový objem = suma všech plátkův → integrál

```
      y
      |  f(x)
      | ╱‾╲
      |╱   ╲___
      a     b  x
      
Rotace kolem osy x:
      
       ╱‾╲
      ( • ) ← průřez je kruh s poloměrem f(x)
       ╲_╱
```

#### Příklad: Objem kužele

Funkce: `f(x) = x` na `[0, h]` (výška kužele = `h`, poloměr = `h`)

```
V = π ∫₀ʰ x² dx = π [x³/3]₀ʰ = π · h³/3 = (π·h³)/3
```

Pro standardní kužel s výškou `h` a poloměrem základny `r`:
- `f(x) = (r/h)·x`
- `V = π ∫₀ʰ [(r/h)·x]² dx = π·(r²/h²) ∫₀ʰ x² dx = π·(r²/h²)·(h³/3) = (1/3)πr²h` ✓

#### Příklad: Objem koule

Funkce: `f(x) = √(r² − x²)` na `[−r, r]` (půlkruh otočený kolem osy x)

```
V = π ∫₋ᵣʳ [√(r² − x²)]² dx
  = π ∫₋ᵣʳ (r² − x²) dx
  = π [r²x − x³/3]₋ᵣʳ
  = π [(r³ − r³/3) − (−r³ + r³/3)]
  = π [2r³ − 2r³/3]
  = π · (4r³/3)
  = (4/3)πr³  ✓ (známý vzorec!)
```

---

## 6. Numerické derivování

**Problém:** Jak aproximovat derivaci `f'(x)`, když známe pouze hodnoty funkce v několika bodech?

### Odvození ze základní definice:

```
         f(x + h) − f(x)
f'(x) ≈ ―――――――――――――――――  (dopředná diference)
               h
```

Pro malé `h` je to dobrá aproximace.

### Tři základní schémata:

| Metoda | Vzorec | Chyba |
|--------|--------|-------|
| **Dopředná diference** | `[f(x+h) − f(x)] / h` | O(h) |
| **Zpětná diference** | `[f(x) − f(x−h)] / h` | O(h) |
| **Centrální diference** | `[f(x+h) − f(x−h)] / (2h)` | O(h²) |

**Centrální diference je přesnější** — má chybu O(h²) místo O(h).

#### Příklad: `f(x) = x²` v bodě `x = 2`, `h = 0.1`

Přesná hodnota: `f'(2) = 4`

```
Dopředná:   [f(2.1) − f(2)] / 0.1 = [4.41 − 4] / 0.1 = 4.1
Zpětná:     [f(2) − f(1.9)] / 0.1 = [4 − 3.61] / 0.1 = 3.9
Centrální:  [f(2.1) − f(1.9)] / 0.2 = [4.41 − 3.61] / 0.2 = 4.0  ✓ (přesně!)
```

### Druhá derivace:

```
f''(x) ≈ [f(x+h) − 2f(x) + f(x−h)] / h²
```

---

## 7. Numerická integrace

**Problém:** Jak aproximovat `∫ᵃᵇ f(x) dx`, když funkci neumíme integrovat analyticky nebo máme pouze tabulková data?

**Idea:** Aproximujeme plochu pod křivkou pomocí jednoduchých geometrických útvarů.

---

### Obdélníková metoda (metoda pravoúhlíků)

**Princip:** Nahradíme plochu pod křivkou součtem obdélníků.

```
      y
      |     
      |  ╱‾‾╲  
      | ╱    ╲  
      |█████████  ← obdélníky
      a        b  x
```

#### Levý obdélník:
```
∫ᵃᵇ f(x) dx ≈ (b − a) · f(a)
```

#### Pravý obdélník:
```
∫ᵃᵇ f(x) dx ≈ (b − a) · f(b)
```

#### Střední obdélník:
```
∫ᵃᵇ f(x) dx ≈ (b − a) · f((a+b)/2)
```

#### Složená obdélníková metoda:

Rozdělíme `[a, b]` na `n` stejných dílů o délce `h = (b−a)/n`:

```
∫ᵃᵇ f(x) dx ≈ h · [f(x₀) + f(x₁) + ... + f(xₙ₋₁)]
```

kde `xᵢ = a + i·h`.

**Chyba:** O(h) — lineární konvergence

---

### Lichoběžníková metoda (trapezoidní pravidlo)

**Princip:** Nahradíme plochu pod křivkou lichoběžníky.

```
      y
      |     
      |  ╱‾‾╲  
      | ╱╱╱╱╱╲  ← lichoběžníky
      |╱______╲
      a        b  x
```

#### Základní vzorec:
```
∫ᵃᵇ f(x) dx ≈ (b − a) · [f(a) + f(b)] / 2
```

Plocha lichoběžníku = (základna) × (průměr výšek).

#### Složená lichoběžníková metoda:

```
∫ᵃᵇ f(x) dx ≈ (h/2) · [f(x₀) + 2f(x₁) + 2f(x₂) + ... + 2f(xₙ₋₁) + f(xₙ)]
```

kde `h = (b−a)/n` a `xᵢ = a + i·h`.

**Zjednodušená paměť:** krajní body mají váhu 1, vnitřní body váhu 2.

**Chyba:** O(h²) — kvadratická konvergence (lepší než obdélníky!)

---

### Simpsonovo pravidlo (parabolická metoda)

**Princip:** Nahradíme plochu pod křivkou parabolami (polynomy 2. stupně).

Potřebujeme **lichý počet bodů** (sudý počet intervalů).

```
      y
      |     
      |  ╱‾‾╲  
      | ╱~~~~╲  ← parabola procházející 3 body
      |╱______╲
      a   m   b  x
```

#### Základní vzorec (3 body):
```
∫ᵃᵇ f(x) dx ≈ (h/3) · [f(a) + 4f((a+b)/2) + f(b)]
```

kde `h = (b−a)/2`.

#### Složené Simpsonovo pravidlo:

Pro `n` sudé:
```
∫ᵃᵇ f(x) dx ≈ (h/3) · [f(x₀) + 4f(x₁) + 2f(x₂) + 4f(x₃) + ... + 4f(xₙ₋₁) + f(xₙ)]
```

**Pravidlo vah:** 1 — 4 — 2 — 4 — 2 — ... — 4 — 1

**Chyba:** O(h⁴) — velmi rychlá konvergence!

---

### Srovnání metod

| Metoda | Tvar aproximace | Chyba | Složitost |
|--------|-----------------|-------|-----------|
| **Obdélníková** | Konstantní funkce | O(h) | Nejjednodušší |
| **Lichoběžníková** | Přímka | O(h²) | Jednoduchá |
| **Simpsonova** | Parabola | O(h⁴) | Nejpřesnější |

**Praktická volba:**
- Obdélníková: když potřebujeme jen hrubý odhad
- Lichoběžníková: dobrý kompromis mezi přesností a jednoduchostí
- Simpsonova: když potřebujeme vysokou přesnost

---

### Příklad výpočtu: `∫₀¹ eˣ dx`

Přesná hodnota: `e − 1 ≈ 1.71828`

Použijeme `n = 2` (3 body), `h = 0.5`:

#### Obdélníková (střední):
```
≈ 1.0 · e^0.5 = 1.0 · 1.64872 = 1.64872
Chyba: 0.070
```

#### Lichoběžníková:
```
≈ (0.5/2) · [e⁰ + 2e^0.5 + e¹]
= 0.25 · [1 + 2·1.64872 + 2.71828]
= 0.25 · 7.01572
= 1.75393
Chyba: 0.036
```

#### Simpsonova:
```
≈ (0.5/3) · [e⁰ + 4e^0.5 + e¹]
= (1/6) · [1 + 4·1.64872 + 2.71828]
= (1/6) · 10.31316
= 1.71886
Chyba: 0.0006  ✓ (výrazně přesnější!)
```

---

## Rychlý přehled — co musím umět říct u zkoušky

1. **Derivace:**
   - Definice (limita podílu)
   - Geometrický význam (směrnice tečny)
   - Základní vzorce (xⁿ, eˣ, ln(x), sin, cos)
   - Pravidla (součet, součin, podíl, složená funkce)

2. **Lokální extrémy:**
   - Nutná podmínka (f' = 0)
   - Postačující podmínky (test 1. nebo 2. derivace)
   - Algoritmus hledání
   - Příklad výpočtu

3. **Navazování křivek:**
   - C⁰, C¹, C² spojitost
   - Podmínky (rovnost funkcí, derivací...)

4. **Primitivní funkce:**
   - Definice (F' = f)
   - Základní vzorce (xⁿ, 1/x, eˣ, sin, cos)
   - Metody (substituce, per partes)

5. **Určitý integrál:**
   - Geometrický význam (plocha)
   - Newton-Leibnizova formule
   - Aplikace: objem rotačního tělesa `V = π∫[f(x)]²dx`

6. **Numerické metody:**
   - Derivování: centrální diference `[f(x+h)−f(x−h)]/(2h)`
   - Integrace: obdélníková (O(h)), lichoběžníková (O(h²)), Simpsonova (O(h⁴))
   - Vzorce pro každou metodu
   - Srovnání přesnosti

---

*Připraveno pro KI/SZZTP | SZZ příprava*
