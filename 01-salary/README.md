# Opakování PPA, načítání dat ze souboru a jejich analýza

## 1. REPL: Read-eval-print-loop

Spusťte REPL Pythonu v terminálu operačního systému. Spočtěte délku přepony pravoúhlého trojúhelníku:

1. Připravte proměnné pro délky odvěsen a = 3; b = 4
2. Vypočtěte délku přepony pravoúhlého trojúhelníku a uložte do proměnné c
   (můžete použít sqrt z modulu math, ale jde to i bez něj :-))
3. Vypište hodnotu c

## 2. CMD, Visual Studio Code

### Doporučené balíčky pro VSCode

- ruff (Astral Software)
  - An extremely fast Python linter and code formatter, written in Rust.
  - Pro přizpůsobení nastavení můžeme upravovat .ruff.toml v root adresáři našeho projektu.
  - Nebo pro globální konfiguraci použít na windows `~\AppData\Roaming\ruff\ruff.toml`    (dle dokumentace `${config_dir}/ruff/pyproject.toml`)
    - My jsme pro vás připravili nastavení, které je rozumné pro studium ADT [.ruff.toml](../.ruff.toml)
- python (microsoft)
  - Python language support with extension access points for IntelliSense (Pylance), Debugging (Python Debugger), linting, formatting, refactoring, unit tests, and more.
- MyPy Type Checker (microsoft)
  - Type checking support for Python files using MyPy.
- Code Spell Checker (Street Side Software)
  - (+Czech - Code Spell Checker)
  - "cSpell.language": "en,cs",

Vytvořte skript (textový soubor) se sekvencí příkazů, které vedou ke stejnému výsledku jako v předešlém cvičení s REPL.

### Zásady pro vypracování

1. Proměnné staticky vytvořte v kódu.
2. Importy soustřeďte na začátku textového souboru
3. Diskutujte příponu souboru (.txt .py) Fungují stejně? Proč?
    1. Program spusťte z terminálu operačního systému
    2. Program spusťte v prostředí VSC


## 3. Jednoduchá aplikace pro analýzu databáze osob

Vstupující datový soubor[^1] [[liks]](https://liks.fav.zcu.cz/adt/exam/service/download-data?filename=data-salaries-years-100Ksh.csv) obsahuje 1.1M datových vzorků s informacemi o sledované skupině osob. Data nesou záznamy z po sobě jdoucích 11 let v období 2010-2020 kódované UTF-8.
Úkolem je vytvořit program, který načte záznamy a vypočte průměrný příjem osob splňující zadanou podmínku, s
jeho pomocí bude snadné odpovídat na otázky jako:
_Jak se změnila průměrná mzda mezi lety 2014 a 2015?_

[^1]: Data jsou synteticky vyrobená -- generovaná ze statistických ukazatelů. Výsledky analýzy tedy mohou být zkreslené

1. Načtete cestu k souboru -- argument spouštěného programu.

   Ověřte, že:
    1. Programu je předán očekávaný počet argumentů.
    2. Předaný argument je cesta k existujícímu souboru.
    3. Ověřte, že pracujeme se souborem ve správném kódování znaků. `open(file_path, encoding="utf-8")`

    _pozn.: Při otevírání souborů používá python kódování závislé na platformě. Tedy je vždy rozumné explicitně uvádět kódování._

2. Připravte třídu pro reprezentaci Záznamu -- řádek v souboru s daty.
3. Vytvořte funkci _load\_data\_file_, která načtete do paměti všechny osoby z datového souboru. Cestu k souboru
      přijme jako svůj parametr. Seznam instancí osob vrátí jako svou návratovou hodnotu.

4. Ošetříme hlavičku souboru
      1. Všechny řádky obsahují očekávaný počet hodnot.
      2. Rok a příjem jsou číselné hodnoty.
      3. Řádek, který neobsahuje validní záznam přeskočte, informujte o tom ale uživatele.
5. Vytvořte funkci _count\_average\_sallary_, která jako vstupní parametr přijme seznam osob a vrátí průměrný
   příjem.
6. Ve funkci main zavolejte funkci pro načtení dat. Pohledem ověřte, že je seznam osob načtený správně.
7. Navrácený seznam osob předejte do funkce pro výpočet průměrného příjmu.
8. Hodnotu průměrného příjmu vytiskněte na standardní výstup programu.
9. Upravte funkci _load\_data\_file_ tak, aby přijímala navíc jeden parametr $year$. Funkce vrátí záznamy
   pouze z daného roku.

10. Jak se změnil průměrný příjem mezi roky 2014 a 2015?

### Kdo stíhá

11. Implementujte smyčku, která se uživatele bude doptávat na rok, který jej zajímá a počítat statistiky.
12. Kromě průměru spočítejte také medián příjmu.


## K zamyšlení

1. Je nutné v našem případě načítat všechny informace o osobách do přepravek?
2. Je možné úkol splnit jedním průchodem, to znamená bez nutnosti načítat celá data do paměti?
3. Jsou problémy které streamově nevyřešíme?
4. Jaká jsou pro a proti? 
5. Když pracujeme jen s podskupinou, má smysl načíst jen podskupinu?
6. Co je návratová hodnota programu? Jak ji nastavit? K čemu je to dobré?
7. ukažme co se stane když bude náš vstup špatný například: jako rok zadáme 2000, který v datech není.

## Tipy

1. VSC umožní při použití ctrl+click prokliknout chybu v konzoli do kódu.
2. Všimněme si podobnosti argumentů python main.py python main.py datat.csv main.py je vstup pro python data.csv je
   vstup pro main.py
3. Očekávání vstupu od uživatele intput() je stejné jako prompt REPLu
4. K dobré praxi programátora patří psát přehledný kód. Zamýšlejme se nad tím, jak větvíme svůj kód pomocí `if else`. Porovnejme následující dva příklady. Který z nich je přehlednější, jednodušší, má menší hloubku zanoření vyhodnocování podmínek?

```python
if len(arguments) == 2:
   input_path = arguments[1]
   if os.path.exists(input_path):
      data = load_data_file(input_path)
      for d in data:
         print(d)
   else:
        print(f"Soubor {input_path} neexistuje")
        exit(2)
else:
    print("Program nebyl spuštěn správně. Očekávám jméno souboru\n")
    exit(1)
```
```python 
if len(arguments) != 2:
    print("Program nebyl spuštěn správně. Očekávám jméno souboru\n")
    exit(1)

input_path = arguments[1]
if not os.path.exists(input_path):
    print(f"Soubor {input_path} neexistuje")
    exit(2)

data = load_data_file(input_path)
for d in data:
    print(d)
```

## K dalšímu procvičení na doma

1. V roce 2013 byl nedostatek krve 0+. Kolik litrů krve mohlo transfúzní oddělení v obci Kamenice v tomto roce odebrat od lidí?
   - Darovat krev může každý muž či žena ve věku 18-65 let.
   - Tělesná hmotnost dárce krve by měla být minimálně 50 kg.
   - Muži mohou darovat 4x ročně ale ženy pouze 3x.
   - Při jednom odběru získáme 450 ml krve


