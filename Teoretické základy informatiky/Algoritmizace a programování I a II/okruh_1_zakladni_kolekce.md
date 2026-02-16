# Okruh 1: Základní abstraktní kolekce

## 📚 Co se naučíte
- Seznamy (List) a jejich operace
- Slovníky (Dictionary) a kdy je použít
- Iterátory - procházení kolekcí
- Časová složitost - jak měřit rychlost algoritmů
- Fronta (Queue) a Zásobník (Stack)

---

## 1️⃣ SEZNAMY (List)

### Praktický příklad na začátek

Představte si, že máte seznam studentů ve třídě:

```python
studenti = ["Anna", "Pavel", "Eva", "Martin", "Jana"]
```

**Co s tím můžete dělat?**

```python
# 1. Přístup k prvkům (podle indexu)
prvni_student = studenti[0]        # "Anna" - indexy začínají od 0!
treti_student = studenti[2]        # "Eva"

# 2. Přidání nového studenta
studenti.append("Petr")            # Přidá na konec
# studenti = ["Anna", "Pavel", "Eva", "Martin", "Jana", "Petr"]

# 3. Vložení doprostřed
studenti.insert(1, "Lukáš")        # Vloží na pozici 1
# studenti = ["Anna", "Lukáš", "Pavel", "Eva", "Martin", "Jana", "Petr"]

# 4. Odebrání studenta
studenti.remove("Eva")             # Odebere prvního "Eva"
# studenti = ["Anna", "Lukáš", "Pavel", "Martin", "Jana", "Petr"]

# 5. Zjištění délky
pocet = len(studenti)              # 6

# 6. Kontrola, zda je prvek v seznamu
je_anna = "Anna" in studenti       # True
je_tom = "Tom" in studenti         # False
```

### ⏱️ Časová složitost operací se seznamem

**Co to vůbec je "časová složitost"?**

Časová složitost říká: **"Jak moc se zpomalí operace, když zvětším seznam?"**

Představte si to takto:
- Máte seznam 10 studentů → operace trvá X času
- Máte seznam 100 studentů → kolikrát déle to trvá?
- Máte seznam 1000 studentů → kolikrát déle to trvá?

#### Rychlé operace - O(1) "konstantní čas"

```python
studenti = ["Anna", "Pavel", "Eva", "Martin", "Jana"]

# Přístup k prvku podle indexu
x = studenti[2]                    # O(1) - RYCHLÉ!
# Nezáleží, jestli má seznam 10 nebo 1000 prvků
# Počítač okamžitě ví, kde hledat
```

**Proč je to rychlé?** 
- Seznam je v paměti jako řada políček vedle sebe
- Počítač ví: "Na pozici 2 se dostanu: začátek + 2 kroky"
- Nevyhledává celý seznam!

```
Index:     0       1        2      3         4
        +-------+-------+-------+-------+-------+
        | Anna  | Pavel | Eva   | Martin| Jana  |
        +-------+-------+-------+-------+-------+
                          ↑
                    Chci index 2 → skočím přímo sem
```

#### Přidání na konec - O(1) "konstantní čas"

```python
studenti.append("Petr")            # O(1) - RYCHLÉ!
# Přidá na konec → prostě připojí další políčko
```

#### Pomalé operace - O(n) "lineární čas"

```python
# Hledání prvku (bez znalosti indexu)
if "Martin" in studenti:           # O(n) - POMALÉ!
    print("Našel jsem Martina!")

# Proč je to pomalé?
# Python musí projít celý seznam, prvek po prvku:
# "Je to Anna? Ne. Je to Pavel? Ne. Je to Eva? Ne. Je to Martin? ANO!"
```

**n** = počet prvků v seznamu

- 10 prvků → maximálně 10 kontrol
- 100 prvků → maximálně 100 kontrol  
- 1000 prvků → maximálně 1000 kontrol

```python
# Vložení doprostřed
studenti.insert(1, "Lukáš")        # O(n) - POMALÉ!

# Proč?
# Musí posunout všechny prvky za pozicí 1 doprava:
```

```
Před:
Index:  0       1       2       3
      +-------+-------+-------+-------+
      | Anna  | Pavel | Eva   | Martin|
      +-------+-------+-------+-------+

Po insert(1, "Lukáš"):
Index:  0       1       2       3       4
      +-------+-------+-------+-------+-------+
      | Anna  | Lukáš | Pavel | Eva   | Martin|
      +-------+-------+-------+-------+-------+
                        ↑       ↑       ↑
                  Všechny se posunuly doprava!
```

#### Shrnutí časové složitosti seznamu

| Operace | Složitost | Vysvětlení |
|---------|-----------|------------|
| `seznam[i]` | O(1) | Přímý přístup podle indexu |
| `seznam.append(x)` | O(1) | Přidání na konec |
| `len(seznam)` | O(1) | Délka je uložená |
| `x in seznam` | O(n) | Musí projít celý seznam |
| `seznam.remove(x)` | O(n) | Musí najít a posunout prvky |
| `seznam.insert(i, x)` | O(n) | Musí posunout prvky |

---

## 2️⃣ SLOVNÍKY (Dictionary)

### Praktický příklad

Potřebujete uložit telefonní čísla studentů:

```python
# Se seznamem by to bylo nepraktické:
studenti = ["Anna", "Pavel", "Eva"]
telefony = ["777111222", "777333444", "777555666"]
# Jak najdete telefon na Annu? Musíte ji najít v prvním seznamu,
# zjistit index a pak použít stejný index v druhém seznamu. Složité!

# Se slovníkem je to jednoduché:
telefony = {
    "Anna": "777111222",
    "Pavel": "777333444",
    "Eva": "777555666"
}

# Použití:
tel_anna = telefony["Anna"]        # "777111222" - O(1) RYCHLÉ!
```

### Klíč vs Hodnota

```python
slovnik = {
    klíč1: hodnota1,
    klíč2: hodnota2
}

# Příklad:
vek = {
    "Anna": 20,     # klíč: "Anna", hodnota: 20
    "Pavel": 22,    # klíč: "Pavel", hodnota: 22
    "Eva": 21       # klíč: "Eva", hodnota: 21
}
```

### Základní operace

```python
telefony = {"Anna": "777111222", "Pavel": "777333444"}

# 1. Přidání nového záznamu
telefony["Martin"] = "777777888"

# 2. Změna existujícího
telefony["Anna"] = "666999888"

# 3. Odebrání
del telefony["Pavel"]

# 4. Kontrola existence klíče
if "Anna" in telefony:             # O(1) - RYCHLÉ!
    print("Anna má záznam")

# 5. Získání hodnoty s výchozí hodnotou (pokud klíč neexistuje)
tel = telefony.get("Neexistující", "Nenalezeno")  # "Nenalezeno"
```

### ⏱️ Časová složitost slovníku

| Operace | Složitost | Vysvětlení |
|---------|-----------|------------|
| `slovnik[klic]` | O(1) | Přímý přístup pomocí hashe |
| `slovnik[klic] = hodnota` | O(1) | Přidání/změna |
| `klic in slovnik` | O(1) | Kontrola existence |
| `del slovnik[klic]` | O(1) | Odebrání |

**Proč je slovník tak rychlý?**
- Používá **hash funkci** - převede klíč na číslo (hash)
- Hash říká, kam v paměti hodnotu uložit
- Nehledá postupně jako seznam!

### Kdy použít seznam vs slovník?

**Seznam:**
- Když záleží na pořadí
- Když potřebujete přístup podle pozice (index)
- Příklad: seznam kroků v receptu

**Slovník:**
- Když potřebujete rychlé vyhledávání podle klíče
- Když pořadí není důležité (v Python 3.7+ se sice pořadí zachovává, ale není to jeho hlavní účel)
- Příklad: telefonní seznam, databáze studentů

---

## 3️⃣ ITERÁTORY

### Co to je iterátor?

Iterátor je způsob, jak **postupně procházet** kolekci prvek po prvku.

### Praktický příklad

```python
studenti = ["Anna", "Pavel", "Eva", "Martin"]

# Základní iterace (procházení)
for student in studenti:
    print(student)

# Co se děje na pozadí?
# Python vytvoří iterátor, který postupně vrací:
# 1. krok: "Anna"
# 2. krok: "Pavel"
# 3. krok: "Eva"
# 4. krok: "Martin"
# 5. krok: KONEC (StopIteration)
```

### Iterace přes slovník

```python
vek = {"Anna": 20, "Pavel": 22, "Eva": 21}

# Iterace přes klíče
for jmeno in vek:
    print(jmeno)                   # Anna, Pavel, Eva

# Iterace přes hodnoty
for cislo in vek.values():
    print(cislo)                   # 20, 22, 21

# Iterace přes páry (klíč, hodnota)
for jmeno, cislo in vek.items():
    print(f"{jmeno} má {cislo} let")
```

### Manuální vytvoření iterátoru

```python
studenti = ["Anna", "Pavel", "Eva"]

# Vytvoření iterátoru
iterator = iter(studenti)

# Ruční postupné získávání prvků
prvni = next(iterator)             # "Anna"
druhy = next(iterator)             # "Pavel"
treti = next(iterator)             # "Eva"
# dalsi = next(iterator)           # CHYBA! StopIteration - už není co vrátit
```

**Proč jsou iterátory užitečné?**
- Šetří paměť - nemusíte mít celou kolekci najednou v paměti
- Umožňují pracovat s nekonečnými sekvencemi
- Standardní způsob práce s kolekcemi v Pythonu

---

## 4️⃣ FRONTA (Queue)

### Princip: FIFO - First In, First Out

**"Kdo přišel první, ten odchází první"** - jako fronta u pokladny.

```
Fronta lidí u pokladny:

Konec fronty ← [Jana] ← [Martin] ← [Eva] ← [Pavel] ← [Anna] → K pokladně
                 (nový)                                (první)

Anna přišla první → obslouží se první
Pavel přišel druhý → obslouží se druhý
atd.
```

### Implementace v Pythonu

```python
from collections import deque

# Vytvoření fronty
fronta = deque()

# Přidání do fronty (na konec)
fronta.append("Anna")              # Fronta: [Anna]
fronta.append("Pavel")             # Fronta: [Anna, Pavel]
fronta.append("Eva")               # Fronta: [Anna, Pavel, Eva]

# Odebrání z fronty (z čela)
prvni = fronta.popleft()           # "Anna"
                                   # Fronta: [Pavel, Eva]
druhy = fronta.popleft()           # "Pavel"
                                   # Fronta: [Eva]
```

### ⏱️ Časová složitost fronty

| Operace | Složitost |
|---------|-----------|
| `fronta.append(x)` (přidání na konec) | O(1) |
| `fronta.popleft()` (odebrání z čela) | O(1) |

### Praktické použití fronty

- **Tisk dokumentů**: Kdo poslal první, ten se vytiskne první
- **Zpracování požadavků**: Server zpracovává dotazy v pořadí, jak přišly
- **BFS algoritmus**: Prohledávání do šířky (breadth-first search)

---

## 5️⃣ ZÁSOBNÍK (Stack)

### Princip: LIFO - Last In, First Out

**"Kdo přišel poslední, ten odchází první"** - jako zásobník talířů.

```
Zásobník talířů:

  [Talíř 3]  ← Přidal jsem poslední → Vezmu si první
  [Talíř 2]
  [Talíř 1]
  ---------
   Stůl
```

### Implementace v Pythonu

```python
# V Pythonu použijeme prostý seznam!

zasobnik = []

# Přidání na vrchol (push)
zasobnik.append("Talíř 1")         # Zásobník: [Talíř 1]
zasobnik.append("Talíř 2")         # Zásobník: [Talíř 1, Talíř 2]
zasobnik.append("Talíř 3")         # Zásobník: [Talíř 1, Talíř 2, Talíř 3]

# Odebrání z vrcholu (pop)
vrchni = zasobnik.pop()            # "Talíř 3"
                                   # Zásobník: [Talíř 1, Talíř 2]
dalsi = zasobnik.pop()             # "Talíř 2"
                                   # Zásobník: [Talíř 1]

# Nahlédnutí na vrchol (bez odebrání)
if zasobnik:                       # Kontrola, že není prázdný
    vrchol = zasobnik[-1]          # "Talíř 1"
```

### ⏱️ Časová složitost zásobníku

| Operace | Složitost |
|---------|-----------|
| `zasobnik.append(x)` (push) | O(1) |
| `zasobnik.pop()` (pop) | O(1) |
| `zasobnik[-1]` (nahlédnutí) | O(1) |

### Praktické použití zásobníku

- **Zpět v prohlížeči**: Historie navštívených stránek
- **Ctrl+Z (Undo)**: Poslední akce se vrátí první
- **Vyhodnocování výrazů**: `(2 + 3) * 4`
- **DFS algoritmus**: Prohledávání do hloubky (depth-first search)
- **Volání funkcí**: Call stack v programování

---

## 📋 SHRNUTÍ PRO ZKOUŠKU

### Základní kolekce

| Kolekce | Hlavní použití | Přístup | Vyhledávání |
|---------|----------------|---------|-------------|
| **Seznam** | Seřazená data, přístup podle pozice | O(1) | O(n) |
| **Slovník** | Párování klíč-hodnota, rychlé vyhledávání | O(1) | O(1) |

### Specializované kolekce

| Kolekce | Princip | Přidání | Odebrání | Použití |
|---------|---------|---------|----------|---------|
| **Fronta** | FIFO | O(1) | O(1) | Zpracování v pořadí |
| **Zásobník** | LIFO | O(1) | O(1) | Historie, zpět |

### Časová složitost - rychlý přehled

- **O(1)** - konstantní → RYCHLÉ! (přístup v seznamu, operace ve slovníku)
- **O(n)** - lineární → roste s velikostí (vyhledávání v seznamu)
- **O(log n)** - logaritmická → velmi rychlé i pro velká data (binární vyhledávání)
- **O(n²)** - kvadratická → POMALÉ! (vnořené cykly)

### Klíčové otázky ke zkoušce

1. **Jaký je rozdíl mezi seznamem a slovníkem?**
   - Seznam: pozice (index), O(n) vyhledávání
   - Slovník: klíč-hodnota, O(1) vyhledávání

2. **Kdy použít frontu vs zásobník?**
   - Fronta: FIFO - pořadí je důležité (tisk, požadavky)
   - Zásobník: LIFO - zpět k předchozímu (undo, historie)

3. **Co je iterátor?**
   - Způsob postupného procházení kolekce

4. **Jaká je časová složitost vyhledávání v seznamu?**
   - O(n) - musí projít celý seznam v nejhorším případě

---

## 🎯 Praktické příklady k procvičení

### Příklad 1: Fronta v nemocnici

```python
from collections import deque

pacienti = deque()

# Příchod pacientů
pacienti.append("Pan Novák - 8:00")
pacienti.append("Paní Svobodová - 8:15")
pacienti.append("Pan Dvořák - 8:30")

# Ošetřování (FIFO)
while pacienti:
    aktualni = pacienti.popleft()
    print(f"Ošetřuji: {aktualni}")

# Výstup:
# Ošetřuji: Pan Novák - 8:00
# Ošetřuji: Paní Svobodová - 8:15
# Ošetřuji: Pan Dvořák - 8:30
```

### Příklad 2: Zásobník operací (Undo)

```python
operace = []  # Zásobník

# Provádění operací
operace.append("Napsal 'Ahoj'")
operace.append("Napsal ' světe'")
operace.append("Smazal poslední slovo")

print("Historie:", operace)

# Undo (LIFO)
if operace:
    posledni = operace.pop()
    print(f"Vracím: {posledni}")
    
# Výstup:
# Historie: ['Napsal Ahoj', 'Napsal  světe', 'Smazal poslední slovo']
# Vracím: Smazal poslední slovo
```

### Příklad 3: Rychlé vyhledávání se slovníkem

```python
# Databáze studentů
studenti_seznam = [
    ("Anna", 20, "anna@email.cz"),
    ("Pavel", 22, "pavel@email.cz"),
    ("Eva", 21, "eva@email.cz")
]

# Převod na slovník pro rychlé vyhledávání
studenti_slovnik = {
    jmeno: {"vek": vek, "email": email}
    for jmeno, vek, email in studenti_seznam
}

# Rychlé vyhledávání O(1)
info_anna = studenti_slovnik["Anna"]
print(info_anna)  # {'vek': 20, 'email': 'anna@email.cz'}
```
