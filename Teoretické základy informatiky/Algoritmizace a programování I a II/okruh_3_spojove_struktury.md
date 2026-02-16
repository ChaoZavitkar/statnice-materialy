# Okruh 3: Spojové datové struktury

## 📚 Co se naučíte
- Jednosměrný spojový seznam (Linked List)
- Základní operace: vkládání, výmaz, vyhledávání
- Binární strom (Binary Tree)
- Operace nad binárním stromem
- Časová složitost všech operací
- Kdy použít spojové struktury místo polí/seznamů

---

## 🔗 CO JSOU SPOJOVÉ STRUKTURY?

### Problém s klasickými seznamy (pole)

```python
# Klasický Python seznam (interně pole)
seznam = [10, 20, 30, 40, 50]

# V paměti vypadá takto:
# [10][20][30][40][50]  ← vedle sebe, souvislý blok paměti
#  ↑   ↑   ↑   ↑   ↑
# index 0,1,2,3,4
```

**Problém:** Co když chcete vložit prvek doprostřed?

```python
seznam.insert(2, 25)  # Vložit 25 na pozici 2

# Musí se posunout všechny prvky doprava!
# [10][20][  ][30][40][50]  ← udělat místo
# [10][20][25][30][40][50]  ← vložit a posunout
#              ↑   ↑   ↑ 
#          Všechny se posunuly → O(n) - POMALÉ!
```

### Řešení: Spojový seznam

**Nápad:** Prvky nemusí být vedle sebe! Každý prvek "ukáže" na další.

```
[10|●]→[20|●]→[30|●]→[40|●]→[50|×]
  ↑            
 data|odkaz
```

**Výhoda:** Vložení doprostřed = jen změna odkazů! O(1)

---

## 1️⃣ JEDNOSMĚRNÝ SPOJOVÝ SEZNAM

### Struktura uzlu (Node)

Každý prvek (uzel) obsahuje:
1. **Data** - hodnota, kterou ukládáme
2. **Odkaz (next)** - ukazatel na další uzel

```python
class Node:
    """Jeden uzel spojového seznamu"""
    def __init__(self, data):
        self.data = data      # Uložená hodnota
        self.next = None      # Odkaz na další uzel (zatím žádný)

# Vytvoření uzlu
uzel1 = Node(10)
print(f"Data: {uzel1.data}")    # 10
print(f"Next: {uzel1.next}")    # None (zatím nikam neukazuje)
```

### Vizualizace jednotlivého uzlu

```
   +--------+--------+
   |  data  |  next  |
   +--------+--------+
   |   10   |  None  |
   +--------+--------+
```

### Propojení uzlů

```python
# Vytvoření tří uzlů
uzel1 = Node(10)
uzel2 = Node(20)
uzel3 = Node(30)

# Propojení
uzel1.next = uzel2    # 10 ukazuje na 20
uzel2.next = uzel3    # 20 ukazuje na 30
uzel3.next = None     # 30 je konec (už je None)

# Máme spojový seznam: 10 → 20 → 30
```

### Vizualizace propojeného seznamu

```
uzel1        uzel2        uzel3
+----+---+  +----+---+  +----+---+
| 10 | ●-|->| 20 | ●-|->| 30 | × |
+----+---+  +----+---+  +----+---+
   ↑
  head (začátek)
```

### Kompletní třída LinkedList

```python
class LinkedList:
    """Spojový seznam"""
    def __init__(self):
        self.head = None  # Začátek seznamu (hlava)
    
    def je_prazdny(self):
        """Kontrola, zda je seznam prázdný"""
        return self.head is None
    
    def vypis(self):
        """Vypíše celý seznam"""
        if self.je_prazdny():
            print("Seznam je prázdný")
            return
        
        aktualni = self.head
        while aktualni is not None:
            print(aktualni.data, end=" → ")
            aktualni = aktualni.next
        print("None")

# Použití
seznam = LinkedList()
seznam.head = Node(10)
seznam.head.next = Node(20)
seznam.head.next.next = Node(30)

seznam.vypis()  # 10 → 20 → 30 → None
```

---

## 2️⃣ VKLÁDÁNÍ DO SPOJOVÉHO SEZNAMU

### A) Vložení na ZAČÁTEK (prepend)

```python
def vloz_na_zacatek(self, data):
    """Vloží nový uzel na začátek seznamu - O(1)"""
    novy_uzel = Node(data)
    novy_uzel.next = self.head  # Nový uzel ukazuje na starý začátek
    self.head = novy_uzel        # Nový uzel je nový začátek
```

**Vizualizace:**

```
Před: head → [10|●] → [20|×]

Krok 1: Vytvoř nový uzel
        [5|?]

Krok 2: Nový uzel ukazuje na starý head
        [5|●] → [10|●] → [20|×]

Krok 3: Head je teď nový uzel
  head → [5|●] → [10|●] → [20|×]
```

**⏱️ Časová složitost: O(1)** - konstantní, vždy stejně rychlé!

### B) Vložení na KONEC (append)

```python
def vloz_na_konec(self, data):
    """Vloží nový uzel na konec seznamu - O(n)"""
    novy_uzel = Node(data)
    
    # Pokud je seznam prázdný
    if self.je_prazdny():
        self.head = novy_uzel
        return
    
    # Najdi poslední uzel
    aktualni = self.head
    while aktualni.next is not None:
        aktualni = aktualni.next
    
    # Připoj nový uzel za poslední
    aktualni.next = novy_uzel
```

**Vizualizace:**

```
Před: head → [10|●] → [20|●] → [30|×]

Krok 1: Projdi až na konec
        head → [10|●] → [20|●] → [30|×]
                                   ↑
                                aktuální

Krok 2: Připoj nový uzel
        head → [10|●] → [20|●] → [30|●] → [40|×]
                                           ↑
                                      nový uzel
```

**⏱️ Časová složitost: O(n)** - musíme projít celý seznam, abychom našli konec

### C) Vložení DOPROSTŘED (na konkrétní pozici)

```python
def vloz_na_pozici(self, data, pozice):
    """Vloží uzel na danou pozici - O(n)"""
    # Pozice 0 = vložení na začátek
    if pozice == 0:
        self.vloz_na_zacatek(data)
        return
    
    novy_uzel = Node(data)
    aktualni = self.head
    
    # Najdi uzel PŘED místem vložení
    for i in range(pozice - 1):
        if aktualni is None:
            print("Pozice je mimo rozsah!")
            return
        aktualni = aktualni.next
    
    # Vlož nový uzel
    novy_uzel.next = aktualni.next
    aktualni.next = novy_uzel
```

**Vizualizace vložení na pozici 2:**

```
Před: head → [10|●] → [20|●] → [40|×]
                       ↑
                  pozice 1 (před vložením)

Krok 1: Najdi uzel na pozici 1 (20)
        head → [10|●] → [20|●] → [40|×]
                         ↑
                    aktuální

Krok 2: Vytvoř nový uzel a přesměruj odkazy
        head → [10|●] → [20|●] → [30|●] → [40|×]
                         ↑        ↑
                    aktuální  nový uzel
```

**⏱️ Časová složitost: O(n)** - musíme dojít na správnou pozici

---

## 3️⃣ VÝMAZ ZE SPOJOVÉHO SEZNAMU

### A) Výmaz ze ZAČÁTKU

```python
def vymaz_zacatek(self):
    """Odstraní první uzel - O(1)"""
    if self.je_prazdny():
        print("Seznam je prázdný!")
        return
    
    self.head = self.head.next  # Hlava je teď druhý uzel
```

**Vizualizace:**

```
Před: head → [10|●] → [20|●] → [30|×]

Po:          head → [20|●] → [30|×]
     
     [10] už není dostupný → automaticky se uvolní z paměti
```

**⏱️ Časová složitost: O(1)** - konstantní

### B) Výmaz z KONCE

```python
def vymaz_konec(self):
    """Odstraní poslední uzel - O(n)"""
    if self.je_prazdny():
        print("Seznam je prázdný!")
        return
    
    # Pokud je jen jeden uzel
    if self.head.next is None:
        self.head = None
        return
    
    # Najdi PŘEDPOSLEDNÍ uzel
    aktualni = self.head
    while aktualni.next.next is not None:
        aktualni = aktualni.next
    
    # Odstraň odkaz na poslední uzel
    aktualni.next = None
```

**Vizualizace:**

```
Před: head → [10|●] → [20|●] → [30|×]
                       ↑
                  předposlední

Krok 1: Najdi předposlední (20)
        head → [10|●] → [20|●] → [30|×]
                         ↑
                    aktuální

Krok 2: Odstraň odkaz
        head → [10|●] → [20|×]
        
        [30] už není dostupný → uvolní se z paměti
```

**⏱️ Časová složitost: O(n)** - musíme najít předposlední uzel

### C) Výmaz KONKRÉTNÍ HODNOTY

```python
def vymaz_hodnotu(self, hodnota):
    """Odstraní první výskyt dané hodnoty - O(n)"""
    if self.je_prazdny():
        return
    
    # Pokud je hodnota na začátku
    if self.head.data == hodnota:
        self.head = self.head.next
        return
    
    # Najdi uzel PŘED uzlem s hodnotou
    aktualni = self.head
    while aktualni.next is not None:
        if aktualni.next.data == hodnota:
            # Přeskoč uzel s hodnotou
            aktualni.next = aktualni.next.next
            return
        aktualni = aktualni.next
    
    print(f"Hodnota {hodnota} nebyla nalezena")
```

**Vizualizace výmazu hodnoty 20:**

```
Před: head → [10|●] → [20|●] → [30|●] → [40|×]
              ↑                  ↑
         aktuální           chceme vymazat

Krok 1: Najdi uzel před 20 (je to 10)
        head → [10|●] → [20|●] → [30|●] → [40|×]
                ↑
           aktuální

Krok 2: Přesměruj odkaz (přeskoč 20)
        head → [10|●] ----+---→ [30|●] → [40|×]
                          |
                       [20|●] (už není dostupný)
```

**⏱️ Časová složitost: O(n)** - v nejhorším případě projdeme celý seznam

---

## 4️⃣ VYHLEDÁVÁNÍ VE SPOJOÉM SEZNAMU

```python
def vyhledej(self, hodnota):
    """Najde uzel s danou hodnotou - O(n)"""
    aktualni = self.head
    pozice = 0
    
    while aktualni is not None:
        if aktualni.data == hodnota:
            return pozice  # Našli jsme!
        aktualni = aktualni.next
        pozice += 1
    
    return -1  # Nenašli jsme
```

**Vizualizace hledání hodnoty 30:**

```
head → [10|●] → [20|●] → [30|●] → [40|×]
        ↓        ↓        ↓
    10==30?  20==30?  30==30? ANO! Vrať pozici 2
```

**⏱️ Časová složitost: O(n)** - v nejhorším případě projdeme celý seznam

---

## 📊 SROVNÁNÍ: POLE vs SPOJOVÝ SEZNAM

| Operace | Pole (Python list) | Spojový seznam |
|---------|-------------------|----------------|
| Přístup na index i | **O(1)** | **O(n)** |
| Vyhledávání | O(n) | O(n) |
| Vložení na začátek | O(n) | **O(1)** |
| Vložení na konec | **O(1)** | O(n)* |
| Vložení doprostřed | O(n) | O(n) |
| Výmaz ze začátku | O(n) | **O(1)** |
| Výmaz z konce | **O(1)** | O(n) |
| Výmaz doprostřed | O(n) | O(n) |

*Poznámka: Pokud si pamatujeme odkaz na konec (tail), je to O(1)*

### Kdy použít spojový seznam?

✅ **Použij spojový seznam když:**
- Často vkládáš/mažeš na začátku
- Nevíš předem velikost dat
- Nepotřebuješ přímý přístup k prvkům podle indexu

❌ **NEPOUŽÍVEJ spojový seznam když:**
- Potřebuješ rychlý přístup na index (array[5])
- Málo vkládáš/mažeš, hodně čteš
- Paměť je omezená (spojový seznam má overhead - odkazy)

---

## 🌳 BINÁRNÍ STROM

### Co je to binární strom?

**Strom** = hierarchická struktura, kde každý uzel může mít maximálně **2 děti** (levé a pravé).

### Terminologie

```
           [50]        ← ROOT (kořen)
          /    \
       [30]    [70]    ← Vnitřní uzly
       /  \      \
    [20] [40]   [80]   ← LISTY (nemají děti)
```

- **Root (kořen):** Nejvyšší uzel (50)
- **Parent (rodič):** Uzel, který má děti (50 je rodič 30 a 70)
- **Child (dítě):** Uzel pod rodičem (30 a 70 jsou děti 50)
- **Leaf (list):** Uzel bez dětí (20, 40, 80)
- **Hloubka:** Počet hran od kořene k uzlu
- **Výška stromu:** Maximální hloubka = nejdelší cesta od kořene k listu

### Struktura uzlu stromu

```python
class TreeNode:
    """Uzel binárního stromu"""
    def __init__(self, data):
        self.data = data
        self.left = None   # Levé dítě
        self.right = None  # Pravé dítě

# Vytvoření stromu z obrázku nahoře
root = TreeNode(50)
root.left = TreeNode(30)
root.right = TreeNode(70)
root.left.left = TreeNode(20)
root.left.right = TreeNode(40)
root.right.right = TreeNode(80)
```

### Vizualizace uzlu

```
    +------+
    | data |
    +------+
    /      \
 left     right
   ↓         ↓
 None      None
```

---

## 5️⃣ OPERACE NAD BINÁRNÍM STROMEM

### A) VKLÁDÁNÍ DO BINÁRNÍHO VYHLEDÁVACÍHO STROMU (BST)

**Pravidlo BST:**
- Levé podstrom < rodič
- Pravé podstrom > rodič

```python
def vloz_do_bst(root, data):
    """
    Vloží hodnotu do binárního vyhledávacího stromu.
    Vrací nový kořen.
    """
    # Bázový případ: prázdný strom
    if root is None:
        return TreeNode(data)
    
    # Rekurzivně najdi správné místo
    if data < root.data:
        root.left = vloz_do_bst(root.left, data)
    else:
        root.right = vloz_do_bst(root.right, data)
    
    return root
```

**Vizualizace vložení hodnot 50, 30, 70, 20:**

```
Krok 1: Vlož 50
        [50]

Krok 2: Vlož 30 (30 < 50 → doleva)
        [50]
        /
      [30]

Krok 3: Vlož 70 (70 > 50 → doprava)
        [50]
        /  \
      [30] [70]

Krok 4: Vlož 20 (20 < 50 → doleva, 20 < 30 → doleva)
        [50]
        /  \
      [30] [70]
      /
    [20]
```

**⏱️ Časová složitost:**
- **Vyvážený strom:** O(log n) - půlíme prostor podobně jako binární vyhledávání
- **Nevyvážený strom:** O(n) - může degenerovat na spojový seznam

```
Vyvážený strom:          Nevyvážený strom (degenerovaný):
      [50]                      [10]
     /    \                       \
  [30]    [70]                   [20]
  /  \      \                      \
[20] [40]  [80]                   [30]
                                    \
Výška: 2                           [40]
O(log n)                            \
                                   [50]
                                   
                                   Výška: 4
                                   O(n) - je to vlastně spojový seznam!
```

### B) VYHLEDÁVÁNÍ V BST

```python
def vyhledej_v_bst(root, hodnota):
    """
    Vyhledá hodnotu v binárním vyhledávacím stromu.
    Vrací True/False.
    """
    # Prázdný strom nebo jsme nenašli
    if root is None:
        return False
    
    # Našli jsme!
    if root.data == hodnota:
        return True
    
    # Hledej v levém nebo pravém podstromu
    if hodnota < root.data:
        return vyhledej_v_bst(root.left, hodnota)
    else:
        return vyhledej_v_bst(root.right, hodnota)
```

**Vizualizace hledání hodnoty 40:**

```
        [50]     40 < 50 → jdi doleva
        /  \
      [30] [70]  40 > 30 → jdi doprava
      /  \
    [20] [40]    40 == 40 → NAŠLI JSME!
```

**⏱️ Časová složitost:**
- **Vyvážený strom:** O(log n)
- **Nevyvážený strom:** O(n)

### C) VÝMAZ Z BST

Výmaz je nejsložitější operace! Máme 3 případy:

**Případ 1: Uzel je list (nemá děti)**
```
        [50]
        /  \
      [30] [70]
      /
    [20] ← Vymažeme 20
    
→ Prostě odstraníme
    
        [50]
        /  \
      [30] [70]
```

**Případ 2: Uzel má JEDNO dítě**
```
        [50]
        /  \
      [30] [70] ← Vymažeme 70
             \
            [80]
            
→ Nahradíme ho jeho dítětem

        [50]
        /  \
      [30] [80]
```

**Případ 3: Uzel má DVĚ děti**
```
        [50] ← Vymažeme 50
        /  \
      [30] [70]
      /  \    \
    [20][40] [80]
    
→ Najdeme NEJMENŠÍ hodnotu v pravém podstromu (70)
→ Nahradíme jí 50

        [70]
        /  \
      [30] [80]
      /  \
    [20][40]
```

```python
def vymaz_z_bst(root, hodnota):
    """Vymaže hodnotu z BST - O(log n) až O(n)"""
    if root is None:
        return None
    
    # Najdi uzel k vymazání
    if hodnota < root.data:
        root.left = vymaz_z_bst(root.left, hodnota)
    elif hodnota > root.data:
        root.right = vymaz_z_bst(root.right, hodnota)
    else:
        # Našli jsme uzel k vymazání!
        
        # Případ 1: List (žádné děti)
        if root.left is None and root.right is None:
            return None
        
        # Případ 2: Jedno dítě
        if root.left is None:
            return root.right
        if root.right is None:
            return root.left
        
        # Případ 3: Dvě děti
        # Najdi nejmenší hodnotu v pravém podstromu
        min_uzel = najdi_minimum(root.right)
        root.data = min_uzel.data
        root.right = vymaz_z_bst(root.right, min_uzel.data)
    
    return root

def najdi_minimum(root):
    """Najde nejmenší hodnotu (nejlevější uzel)"""
    while root.left is not None:
        root = root.left
    return root
```

---

## 6️⃣ PRŮCHODY STROMEM

Existují 3 hlavní způsoby, jak projít všechny uzly ve stromu:

### A) In-order (Inorder) - LDR
**Pořadí:** Levý podstrom → Data → Pravý podstrom

```python
def inorder(root):
    """In-order průchod stromem"""
    if root is not None:
        inorder(root.left)       # Levý podstrom
        print(root.data, end=" ")  # Data
        inorder(root.right)      # Pravý podstrom
```

**Pro BST vrací hodnoty SEŘAZENÉ vzestupně!**

```
        [50]
        /  \
      [30] [70]
      /  \    \
    [20][40] [80]

In-order: 20 30 40 50 70 80  ← Seřazené!
```

### B) Pre-order (Preorder) - DLR
**Pořadí:** Data → Levý podstrom → Pravý podstrom

```python
def preorder(root):
    """Pre-order průchod"""
    if root is not None:
        print(root.data, end=" ")  # Data
        preorder(root.left)       # Levý
        preorder(root.right)      # Pravý
```

```
        [50]
        /  \
      [30] [70]
      /  \    \
    [20][40] [80]

Pre-order: 50 30 20 40 70 80  ← Rodič před dětmi
```

### C) Post-order (Postorder) - LRD
**Pořadí:** Levý → Pravý → Data

```python
def postorder(root):
    """Post-order průchod"""
    if root is not None:
        postorder(root.left)      # Levý
        postorder(root.right)     # Pravý
        print(root.data, end=" ")  # Data
```

```
        [50]
        /  \
      [30] [70]
      /  \    \
    [20][40] [80]

Post-order: 20 40 30 80 70 50  ← Děti před rodičem
```

**Použití průchodů:**
- **In-order:** Získání seřazených dat z BST
- **Pre-order:** Kopírování stromu, vytvoření prefixové notace
- **Post-order:** Mazání stromu (smažu nejdřív děti, pak rodiče)

---

## 📋 SHRNUTÍ PRO ZKOUŠKU

### Spojový seznam - časová složitost

| Operace | Časová složitost |
|---------|------------------|
| Vložení na začátek | O(1) |
| Vložení na konec | O(n) |
| Výmaz ze začátku | O(1) |
| Výmaz z konce | O(n) |
| Vyhledávání | O(n) |
| Přístup na index | O(n) |

### Binární vyhledávací strom - časová složitost

| Operace | Vyvážený | Nevyvážený |
|---------|----------|------------|
| Vložení | O(log n) | O(n) |
| Vyhledávání | O(log n) | O(n) |
| Výmaz | O(log n) | O(n) |

### Klíčové rozdíly

**Spojový seznam:**
- Lineární struktura (jako řetěz)
- Každý uzel má max 1 následníka
- Rychlé operace na začátku

**Binární strom:**
- Hierarchická struktura
- Každý uzel má max 2 děti
- Rychlé vyhledávání (pokud vyvážený)

### Otázky ke zkoušce

**1. Jaká je výhoda spojového seznamu oproti poli?**
- Vložení/výmaz na začátku je O(1)
- Dynamická velikost bez realokace

**2. Kdy je spojový seznam pomalejší než pole?**
- Přístup na index: O(n) vs O(1)
- Vyhledávání: stejné O(n), ale pole má lepší cache lokalitu

**3. Co je to BST a jaké má vlastnosti?**
- Binární vyhledávací strom
- Levý podstrom < rodič < pravý podstrom
- Umožňuje rychlé vyhledávání O(log n)

**4. Proč může BST degenerovat na O(n)?**
- Pokud vkládáme seřazená data
- Strom se stane nevyváženým (spojový seznam)

**5. Jaké jsou průchody stromem a k čemu slouží?**
- In-order: seřazené hodnoty v BST
- Pre-order: rodič před dětmi
- Post-order: děti před rodičem (mazání)

---

## 🎯 Praktické příklady

### Příklad 1: Kompletní spojový seznam

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def vloz_na_zacatek(self, data):
        novy = Node(data)
        novy.next = self.head
        self.head = novy
    
    def vypis(self):
        aktualni = self.head
        while aktualni:
            print(aktualni.data, end=" → ")
            aktualni = aktualni.next
        print("None")

# Použití
seznam = LinkedList()
seznam.vloz_na_zacatek(30)
seznam.vloz_na_zacatek(20)
seznam.vloz_na_zacatek(10)
seznam.vypis()  # 10 → 20 → 30 → None
```

### Příklad 2: Binární vyhledávací strom

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

def vloz(root, data):
    if root is None:
        return TreeNode(data)
    if data < root.data:
        root.left = vloz(root.left, data)
    else:
        root.right = vloz(root.right, data)
    return root

def inorder(root):
    if root:
        inorder(root.left)
        print(root.data, end=" ")
        inorder(root.right)

# Vytvoření stromu
root = None
hodnoty = [50, 30, 70, 20, 40, 80]
for h in hodnoty:
    root = vloz(root, h)

inorder(root)  # 20 30 40 50 70 80 (seřazené!)
```