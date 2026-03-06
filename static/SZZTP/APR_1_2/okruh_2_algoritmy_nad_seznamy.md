# Okruh 2: Komplexní algoritmy nad seznamy

## Co se naučíte
- Filtrování a vyhledávání v seznamech
- Jednoduché třídění (selection sort, insertion sort)
- Binární vyhledávání (rychlé vyhledávání v seřazeném seznamu)
- Merge sort (efektivní třídění)
- Analýza časové složitosti algoritmů

---

## 1. Filtrování

### Praktický příklad

Máte seznam studentů s jejich body a chcete najít ty, kteří mají více než 50 bodů:

```python
studenti = [
    {"jmeno": "Anna", "body": 85},
    {"jmeno": "Pavel", "body": 42},
    {"jmeno": "Eva", "body": 67},
    {"jmeno": "Martin", "body": 35},
    {"jmeno": "Jana", "body": 91}
]

# Filtrování - způsob 1: List comprehension
prospeli = [s for s in studenti if s["body"] >= 50]

print(prospeli)
# [{"jmeno": "Anna", "body": 85}, 
#  {"jmeno": "Eva", "body": 67}, 
#  {"jmeno": "Jana", "body": 91}]
```

### Filtrování - krok po kroku

```python
# Způsob 2: Klasický cyklus (více vidět, co se děje)
prospeli = []

for student in studenti:
    if student["body"] >= 50:
        prospeli.append(student)
        
# Co se děje?
# 1. Projdi každého studenta
# 2. Pokud má >= 50 bodů, přidej ho do nového seznamu
# 3. Ostatní přeskoč
```

### Použití funkce filter()

```python
# Způsob 3: Pomocí filter() - funkcionální přístup
def ma_50_bodu(student):
    return student["body"] >= 50

prospeli = list(filter(ma_50_bodu, studenti))

# Nebo s lambda funkcí (anonymní funkce)
prospeli = list(filter(lambda s: s["body"] >= 50, studenti))
```

### Časová složitost filtrování

**O(n)** - lineární

**Proč?**
- Musíme projít KAŽDÝ prvek v seznamu
- n prvků → n kontrol

```
Seznam 5 studentů → 5 kontrol
Seznam 100 studentů → 100 kontrol
Seznam 1000 studentů → 1000 kontrol
```

---

## 2. Vyhledávání

### Lineární vyhledávání (Sequential Search)

**Princip:** Projdi seznam od začátku do konce, dokud nenajdeš hledaný prvek.

```python
def linealni_vyhledavani(seznam, hledany):
    """
    Najde index prvního výskytu hledaného prvku.
    Vrací -1, pokud prvek není v seznamu.
    """
    for i in range(len(seznam)):
        if seznam[i] == hledany:
            return i  # Našli jsme! Vrátíme index
    return -1  # Nenašli jsme

# Použití
cisla = [64, 34, 25, 12, 22, 11, 90]
index = linealni_vyhledavani(cisla, 22)
print(f"Číslo 22 je na indexu: {index}")  # 4
```

### Vizualizace lineárního vyhledávání

```
Hledáme: 22

Krok 1: [64, 34, 25, 12, 22, 11, 90]
         ↑
         64 == 22? NE, pokračuj

Krok 2: [64, 34, 25, 12, 22, 11, 90]
             ↑
             34 == 22? NE, pokračuj

Krok 3: [64, 34, 25, 12, 22, 11, 90]
                 ↑
                 25 == 22? NE, pokračuj

Krok 4: [64, 34, 25, 12, 22, 11, 90]
                     ↑
                     12 == 22? NE, pokračuj

Krok 5: [64, 34, 25, 12, 22, 11, 90]
                         ↑
                         22 == 22? ANO! Vrať index 4
```

### Časová složitost lineárního vyhledávání

**O(n)** - lineární

- **Nejlepší případ:** O(1) - prvek je hned na začátku
- **Průměrný případ:** O(n/2) = O(n) - v průměru najdeme v polovině
- **Nejhorší případ:** O(n) - prvek je na konci nebo není v seznamu

---

## 3. Binární vyhledávání

### Podmínka: SEZNAM MUSÍ BÝT SEŘAZENÝ!

**Princip:** Půlíme seznam, dokud nenajdeme prvek (jako hádání čísla).

### Analogie: Hádání čísla 1-100

```
Hádám číslo mezi 1 a 100.

Pokus 1: "Je to 50?"
         "Ne, je to VĚTŠÍ"
         Vím, že to NENÍ 1-50 → hledám v 51-100

Pokus 2: "Je to 75?"
         "Ne, je to MENŠÍ"
         Vím, že to NENÍ 76-100 → hledám v 51-74

Pokus 3: "Je to 63?"
         "Ne, je to VĚTŠÍ"
         → hledám v 64-74

Pokus 4: "Je to 69?"
         "ANO!"
         
Našel jsem číslo za 4 pokusy místo až 69!
```

### Implementace binárního vyhledávání

```python
def binarni_vyhledavani(serazeny_seznam, hledany):
    """
    Vyhledávání půlením intervalu.
    FUNGUJE JEN NA SEŘAZENÉM SEZNAMU!
    """
    levy = 0
    pravy = len(serazeny_seznam) - 1
    
    while levy <= pravy:
        stred = (levy + pravy) // 2  # Index prostředního prvku
        
        if serazeny_seznam[stred] == hledany:
            return stred  # Našli jsme!
        
        elif serazeny_seznam[stred] < hledany:
            # Hledaný je VĚTŠÍ → hledej v pravé polovině
            levy = stred + 1
        
        else:
            # Hledaný je MENŠÍ → hledej v levé polovině
            pravy = stred - 1
    
    return -1  # Nenašli jsme

# Použití
cisla = [11, 12, 22, 25, 34, 64, 90]  # MUSÍ být seřazený!
index = binarni_vyhledavani(cisla, 25)
print(f"Číslo 25 je na indexu: {index}")  # 3
```

### Vizualizace binárního vyhledávání

```
Hledáme: 25 v seřazeném seznamu

Start:  [11, 12, 22, 25, 34, 64, 90]
         L              S           P
         0              3           6
         
         Stred (index 3) = 25
         25 == 25? ANO! Našli jsme na indexu 3

---

Jiný příklad - hledáme: 64

Krok 1: [11, 12, 22, 25, 34, 64, 90]
         L              S           P
         0              3           6
         
         Stred = 25
         64 > 25 → hledej VPRAVO
         Nový interval: [34, 64, 90]

Krok 2: [34, 64, 90]
         L   S      P
         4   5      6
         
         Stred = 64
         64 == 64? ANO! Našli jsme na indexu 5
```

### Časová složitost binárního vyhledávání

**O(log n)** - logaritmická

**Co to znamená?**

Při každém kroku PŮLÍME prostor, kde hledáme:

```
1000 prvků → půlíme → 500 → 250 → 125 → 63 → 32 → 16 → 8 → 4 → 2 → 1
             Kolik kroků? Asi 10!

Pro n = 1000: log₂(1000) ≈ 10 kroků
Pro n = 1 000 000: log₂(1 000 000) ≈ 20 kroků

Lineární hledání: 1 000 000 kroků!
Binární hledání: 20 kroků!
```

**Srovnání:**

| Velikost seznamu | Lineární O(n) | Binární O(log n) |
|------------------|---------------|------------------|
| 10 | 10 | 4 |
| 100 | 100 | 7 |
| 1 000 | 1 000 | 10 |
| 1 000 000 | 1 000 000 | 20 |

---

## 4. Selection Sort (Třídění výběrem)

### Princip

Opakovaně najdi NEJMENŠÍ prvek v neseřazené části a přesuň ho na začátek.

### Analogie: Třídění karet

```
Máte karty: [7, 3, 9, 2, 5]

Krok 1: Najděte nejmenší (2), vyměňte s prvním
        [2, 3, 9, 7, 5]
         ✓ (seřazeno)

Krok 2: Najděte nejmenší v [3, 9, 7, 5] → je to 3, už je na správném místě
        [2, 3, 9, 7, 5]
         ✓  ✓

Krok 3: Najděte nejmenší v [9, 7, 5] → je to 5, vyměňte s 9
        [2, 3, 5, 7, 9]
         ✓  ✓  ✓

Krok 4: Najděte nejmenší v [7, 9] → je to 7, už je na správném místě
        [2, 3, 5, 7, 9]
         ✓  ✓  ✓  ✓

Krok 5: Zbývá jen [9] → hotovo!
        [2, 3, 5, 7, 9]
         ✓  ✓  ✓  ✓  ✓
```

### Implementace

```python
def selection_sort(seznam):
    """
    Třídí seznam výběrem (selection sort).
    Mění původní seznam!
    """
    n = len(seznam)
    
    # Projdi všechny pozice kromě poslední
    for i in range(n - 1):
        # Najdi index nejmenšího prvku v neseřazené části
        min_index = i
        
        for j in range(i + 1, n):
            if seznam[j] < seznam[min_index]:
                min_index = j
        
        # Vyměň nejmenší prvek s prvkem na pozici i
        seznam[i], seznam[min_index] = seznam[min_index], seznam[i]
        
        print(f"Krok {i+1}: {seznam}")  # Pro vizualizaci
    
    return seznam

# Použití
cisla = [64, 34, 25, 12, 22, 11, 90]
print("Původní:", cisla)
serazene = selection_sort(cisla.copy())
print("Seřazené:", serazene)
```

### Výstup:

```
Původní: [64, 34, 25, 12, 22, 11, 90]
Krok 1: [11, 34, 25, 12, 22, 64, 90]
Krok 2: [11, 12, 25, 34, 22, 64, 90]
Krok 3: [11, 12, 22, 34, 25, 64, 90]
Krok 4: [11, 12, 22, 25, 34, 64, 90]
Krok 5: [11, 12, 22, 25, 34, 64, 90]
Krok 6: [11, 12, 22, 25, 34, 64, 90]
Seřazené: [11, 12, 22, 25, 34, 64, 90]
```

### Časová složitost Selection Sort

**O(n²)** - kvadratická

**Proč?**
- Vnější cyklus: n-1 průchodů
- Vnitřní cyklus: hledání minima v zbytku

```
n = 7 prvků

Průchod 1: porovnání 6 prvků
Průchod 2: porovnání 5 prvků
Průchod 3: porovnání 4 prvků
Průchod 4: porovnání 3 prvků
Průchod 5: porovnání 2 prvků
Průchod 6: porovnání 1 prvku

Celkem: 6 + 5 + 4 + 3 + 2 + 1 = 21 porovnání
Obecně: n(n-1)/2 ≈ n²/2 = O(n²)
```

---

## 5. Insertion Sort (Třídění vkládáním)

### Princip

Postupně beru prvky a vkládám je na správné místo v už seřazené části (jako třídění karet v ruce).

### Analogie: Karty v ruce

```
Berete karty z balíčku a postupně je řadíte v ruce:

Start: [] (prázdná ruka)
       [5, 2, 4, 6, 1] (balíček)

Krok 1: Vezmu 5 → [5]
        Zbývá: [2, 4, 6, 1]

Krok 2: Vezmu 2 → kam patří? Před 5
        [2, 5]
        Zbývá: [4, 6, 1]

Krok 3: Vezmu 4 → kam patří? Mezi 2 a 5
        [2, 4, 5]
        Zbývá: [6, 1]

Krok 4: Vezmu 6 → kam patří? Za 5
        [2, 4, 5, 6]
        Zbývá: [1]

Krok 5: Vezmu 1 → kam patří? Na začátek
        [1, 2, 4, 5, 6]
        Zbývá: []
```

### Implementace

```python
def insertion_sort(seznam):
    """
    Třídí seznam vkládáním (insertion sort).
    Mění původní seznam!
    """
    n = len(seznam)
    
    # Začínáme od druhého prvku (první už je "seřazený")
    for i in range(1, n):
        klic = seznam[i]  # Prvek, který vkládáme
        j = i - 1
        
        # Posouváme větší prvky doprava
        while j >= 0 and seznam[j] > klic:
            seznam[j + 1] = seznam[j]
            j -= 1
        
        # Vložíme klíč na správné místo
        seznam[j + 1] = klic
        
        print(f"Krok {i}: {seznam}")  # Pro vizualizaci
    
    return seznam

# Použití
cisla = [64, 34, 25, 12, 22, 11, 90]
print("Původní:", cisla)
serazene = insertion_sort(cisla.copy())
print("Seřazené:", serazene)
```

### Vizualizace krok po kroku

```
Původní: [64, 34, 25, 12, 22, 11, 90]

Krok 1: Vkládám 34
        [34, 64, 25, 12, 22, 11, 90]
         ✓   ✓  (seřazená část)

Krok 2: Vkládám 25
        [25, 34, 64, 12, 22, 11, 90]
         ✓   ✓   ✓

Krok 3: Vkládám 12
        [12, 25, 34, 64, 22, 11, 90]
         ✓   ✓   ✓   ✓

Krok 4: Vkládám 22
        [12, 22, 25, 34, 64, 11, 90]
         ✓   ✓   ✓   ✓   ✓

Krok 5: Vkládám 11
        [11, 12, 22, 25, 34, 64, 90]
         ✓   ✓   ✓   ✓   ✓   ✓

Krok 6: Vkládám 90
        [11, 12, 22, 25, 34, 64, 90]
         ✓   ✓   ✓   ✓   ✓   ✓   ✓
```

### Časová složitost Insertion Sort

**O(n²)** - kvadratická (v nejhorším případě)

- **Nejlepší případ:** O(n) - seznam už je seřazený
- **Průměrný případ:** O(n²)
- **Nejhorší případ:** O(n²) - seznam je seřazený obráceně

**Výhoda:** Velmi rychlý pro TÉMĚŘ seřazené seznamy!

---

## 6. Merge sort (Třídění slučováním)

### Princip: Rozděl a panuj (Divide and Conquer)

1. **Rozděl** seznam na poloviny, dokud nemáš jednosměrné seznamy
2. **Slučuj** seřazené poloviny zpět dohromady

### Vizualizace rozkladu

```
                [38, 27, 43, 3, 9, 82, 10]
                          |
          ----------------+----------------
          |                              |
    [38, 27, 43, 3]              [9, 82, 10]
          |                              |
      ----+----                      ----+----
      |       |                      |       |
  [38, 27] [43, 3]                [9, 82]  [10]
      |       |                      |       |
    --+--   --+--                  --+--     |
    |   |   |   |                  |   |     |
  [38][27][43][3]                [9] [82]  [10]
    
Nyní slučujeme (merge) seřazené části:

  [38][27] → [27, 38]
  [43][3]  → [3, 43]
  [27, 38] + [3, 43] → [3, 27, 38, 43]
  
  [9][82]  → [9, 82]
  [9, 82] + [10] → [9, 10, 82]
  
  [3, 27, 38, 43] + [9, 10, 82] → [3, 9, 10, 27, 38, 43, 82]
```

### Implementace

```python
def merge_sort(seznam):
    """
    Třídí seznam pomocí merge sort.
    Vrací NOVÝ seřazený seznam.
    """
    # Bázový případ: seznam s 0 nebo 1 prvkem je už seřazený
    if len(seznam) <= 1:
        return seznam
    
    # Rozděl seznam na poloviny
    stred = len(seznam) // 2
    leva_cast = seznam[:stred]
    prava_cast = seznam[stred:]
    
    # Rekurzivně seřaď obě poloviny
    leva_serazena = merge_sort(leva_cast)
    prava_serazena = merge_sort(prava_cast)
    
    # Slouč seřazené poloviny
    return merge(leva_serazena, prava_serazena)


def merge(leva, prava):
    """
    Sloučí dva seřazené seznamy do jednoho seřazeného.
    """
    vysledek = []
    i = j = 0
    
    # Porovnávej prvky z obou seznamů a přidávej menší
    while i < len(leva) and j < len(prava):
        if leva[i] <= prava[j]:
            vysledek.append(leva[i])
            i += 1
        else:
            vysledek.append(prava[j])
            j += 1
    
    # Přidej zbývající prvky (jeden seznam už je prázdný)
    vysledek.extend(leva[i:])
    vysledek.extend(prava[j:])
    
    return vysledek


# Použití
cisla = [38, 27, 43, 3, 9, 82, 10]
print("Původní:", cisla)
serazene = merge_sort(cisla)
print("Seřazené:", serazene)
```

### Jak funguje slučování (merge)?

```
Slučuji: [3, 27, 38] a [9, 10, 82]

Krok 1: Porovnej 3 vs 9 → 3 je menší
        Výsledek: [3]
        Zbývá: [27, 38] a [9, 10, 82]

Krok 2: Porovnej 27 vs 9 → 9 je menší
        Výsledek: [3, 9]
        Zbývá: [27, 38] a [10, 82]

Krok 3: Porovnej 27 vs 10 → 10 je menší
        Výsledek: [3, 9, 10]
        Zbývá: [27, 38] a [82]

Krok 4: Porovnej 27 vs 82 → 27 je menší
        Výsledek: [3, 9, 10, 27]
        Zbývá: [38] a [82]

Krok 5: Porovnej 38 vs 82 → 38 je menší
        Výsledek: [3, 9, 10, 27, 38]
        Zbývá: [] a [82]

Krok 6: Levý seznam je prázdný, přidej zbytek pravého
        Výsledek: [3, 9, 10, 27, 38, 82]
```

### ⏱️ Časová složitost Merge Sort

**O(n log n)** - linearitmická

**Proč?**
- **Rozklad:** log n úrovní (půlíme seznam)
- **Slučování:** n operací na každé úrovni

```
Pro n = 8 prvků:

Úroveň 0:     [1 seznam o 8 prvcích]     → 8 operací
Úroveň 1:     [2 seznamy po 4 prvcích]   → 8 operací
Úroveň 2:     [4 seznamy po 2 prvcích]   → 8 operací
Úroveň 3:     [8 seznamů po 1 prvku]     → 0 operací (bázový případ)

Celkem: 3 úrovně (log₂ 8 = 3) × 8 operací = 24 operací
Obecně: log n úrovní × n operací = n log n
```

**Výhody Merge Sort:**
- Garantovaná O(n log n) i v nejhorším případě
- Stabilní třídění (zachovává pořadí stejných prvků)

**Nevýhody:**
- Potřebuje extra paměť pro pomocné pole (O(n))

---

## Srovnání algoritmů

### Třídící algoritmy

| Algoritmus | Časová složitost | Prostorová složitost | Stabilní? | Kdy použít? |
|------------|------------------|---------------------|-----------|-------------|
| **Selection Sort** | O(n²) | O(1) | Ne | Malé seznamy, málo paměti |
| **Insertion Sort** | O(n²) | O(1) | Ano | Téměř seřazené seznamy |
| **Merge Sort** | O(n log n) | O(n) | Ano | Velké seznamy, potřeba rychlosti |

### Vyhledávací algoritmy

| Algoritmus | Časová složitost | Podmínka | Kdy použít? |
|------------|------------------|----------|-------------|
| **Lineární** | O(n) | Žádná | Neseřazené seznamy |
| **Binární** | O(log n) | Seřazený seznam! | Seřazené seznamy, časté vyhledávání |

### Růst časové složitosti

```
Pro n = 1000 prvků:

O(1):        1 operace         → okamžité
O(log n):    ~10 operací       → velmi rychlé
O(n):        1 000 operací     → rychlé
O(n log n):  ~10 000 operací   → přijatelné
O(n²):       1 000 000 operací → pomalé!
O(2ⁿ):       2¹⁰⁰⁰ operací     → nemožné!
```

---

## Shrnutí pro zkoušku

### Klíčové body

1. **Filtrování** - O(n)
   - Projít celý seznam a vybrat prvky podle podmínky

2. **Lineární vyhledávání** - O(n)
   - Projít seznam dokud nenajdeme prvek
   - Funguje na jakémkoliv seznamu

3. **Binární vyhledávání** - O(log n)
   - POUZE na seřazeném seznamu!
   - Půlíme prostor hledání
   - Mnohem rychlejší než lineární

4. **Selection Sort** - O(n²)
   - Opakovaně najdi minimum a přesuň na začátek
   - Jednoduché, ale pomalé

5. **Insertion Sort** - O(n²)
   - Vkládej prvky na správné místo
   - Rychlé pro téměř seřazené seznamy

6. **Merge Sort** - O(n log n)
   - Rozděl a panuj
   - Rychlé i pro velké seznamy
   - Potřebuje extra paměť

### Časová složitost - pořadí rychlosti

```
Od nejrychlejší po nejpomalejší:

O(1) > O(log n) > O(n) > O(n log n) > O(n²) > O(2ⁿ)
  ↑                                              ↑
konstantní                                  exponenciální
```

### Otázky ke zkoušce

**1. Jaký je rozdíl mezi lineárním a binárním vyhledáváním?**
- Lineární: O(n), funguje vždy
- Binární: O(log n), POUZE na seřazeném seznamu

**2. Proč je binární vyhledávání rychlejší?**
- Půlí prostor hledání při každém kroku
- log n místo n kroků

**3. Jaká je časová složitost insertion sort?**
- O(n²) v průměru a nejhorším případě
- O(n) pro téměř seřazený seznam

**4. Proč je merge sort lepší než selection sort?**
- Merge sort: O(n log n) - rychlejší pro velké seznamy
- Selection sort: O(n²) - pomalejší

**5. Jak funguje princip "rozděl a panuj" u merge sort?**
- Rozděl seznam na poloviny
- Seřaď každou polovinu rekurzivně
- Slouč seřazené poloviny

---

## 🎯 Praktické příklady k procvičení

### Příklad 1: Porovnání rychlosti vyhledávání

```python
import time

# Vytvoř velký seznam
velky_seznam = list(range(1000000))

# Lineární vyhledávání
start = time.time()
index = velky_seznam.index(999999)
cas_linearni = time.time() - start

# Binární vyhledávání (seznam je už seřazený)
start = time.time()
index = binarni_vyhledavani(velky_seznam, 999999)
cas_binarni = time.time() - start

print(f"Lineární: {cas_linearni:.6f} sekund")
print(f"Binární: {cas_binarni:.6f} sekund")
print(f"Binární je {cas_linearni/cas_binarni:.0f}x rychlejší!")
```

### Příklad 2: Kdy použít insertion sort vs merge sort

```python
import random

# Téměř seřazený seznam (jen pár prvků na špatném místě)
temer_serazeny = list(range(100))
random.shuffle(temer_serazeny[:5])  # Pomíchej jen prvních 5

# Insertion sort bude velmi rychlý!
serazeny = insertion_sort(temer_serazeny.copy())

# ---

# Náhodný seznam
nahodny = [random.randint(1, 1000) for _ in range(100)]

# Tady je lepší merge sort
serazeny = merge_sort(nahodny)
```

### Příklad 3: Filtrování a třídění kombinace

```python
studenti = [
    {"jmeno": "Anna", "body": 85, "rocnik": 2},
    {"jmeno": "Pavel", "body": 42, "rocnik": 1},
    {"jmeno": "Eva", "body": 67, "rocnik": 2},
    {"jmeno": "Martin", "body": 91, "rocnik": 3},
]

# 1. Filtruj studenty z 2. ročníku
druhy_rocnik = [s for s in studenti if s["rocnik"] == 2]

# 2. Seřaď podle bodů (od nejvyššího)
serazeni = sorted(druhy_rocnik, key=lambda s: s["body"], reverse=True)

print(serazeni)
# [{'jmeno': 'Anna', 'body': 85, 'rocnik': 2},
#  {'jmeno': 'Eva', 'body': 67, 'rocnik': 2}]
```

---
