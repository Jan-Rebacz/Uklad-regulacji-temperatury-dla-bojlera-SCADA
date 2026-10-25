# Plan Działania i Workflow Zespołowy (2 Osoby)

Dokument organizuje pracę nad projektem dla dwóch osób, tak aby praca mogła przebiegać równolegle (jedna osoba w **SEE Electrical**, druga w **Wonderware/AVEVA InTouch**), korzystając ze wspólnej bazy oznaczeń.

---

## 1. Mapa Myśli Projektu

```text
PROJEKT 11: BOJLER 3000L (130°C, 15x2kW = 30kW)
 │
 ├── 1. CZĘŚĆ OBLICZENIOWA I KONCEPCYJNA (OBLICZENIA_I_OPIS.md + BOM_ELEMENTY.md)
 │    ├── Podział 15 grzałek na 4 sekcje binarne: 2kW (1), 4kW (2), 8kW (4), 16kW (8)
 │    ├── Pompa obiegowa 3-fazowa M1 (1.5 kW) wymuszająca obieg pod ciśnieniem
 │    ├── Obliczenia prądów, zabezpieczeń (B6, B10, B16, B25, PKZM0-4, RCD) i kabli
 │    └── Algorytm sterowania PLC (Generator 10s/20s + Licznik rewersyjny CTUD 0..15)
 │
 ├── 2. DOKUMENTACJA ELEKTRYCZNA – SEE ELECTRICAL (Osoba A)
 │    ├── Arkusz 1: Obwód główny zasilania 3x400V (Pompa M1 + 4 sekcje grzałek E1..E4)
 │    ├── Arkusz 2: Obwód bezpieczeństwa 24V DC (Przekaźnik AL) + Wybór trybu (Ręczny bez PLC / Auto)
 │    ├── Arkusz 3: Sterownik PLC (Wejścia I1..I3, Wyjścia Q1..Q4) + Sygnalizacja H1..H7 i HA1
 │    ├── Zabudowa 2D: Wnętrze szafy 600x800 mm + Elewacja drzwi (przyciski i lampki)
 │    └── Raporty automatyczne: Zestawienie materiałów (BOM), lista zacisków, lista I/O PLC
 │
 └── 3. SYSTEM SCADA – AVEVA / WONDERWARE INTOUCH (Osoba B)
      ├── Bezpieczeństwo: 2 użytkowników (Operator = 5000, Automatyk = 9001) + Inactivity Timeout
      ├── Tagname Dictionary: 21 zmiennych (mieści się w limicie 32 tagów wersji Demo!)
      ├── Skrypt symulacyjny (Application Script): Fizyka bojlera, ciśnienia, licznika i awarii
      └── Okna HMI:
           ├── 1. Bojler_Glowne (Synoptyka, przełącznik Ręczny/Auto, suwaki zakłóceń)
           ├── 2. Alarmy (Bieżące "summ", Historyczne "hist", grupy, potwierdzanie)
           └── 3. Haslo (Okno logowania operatora i automatyka)
```

---

## 2. Podział zadań na 2 osoby

### Osoba A: Inżynier Elektryk (SEE Electrical + Dokumentacja Word/PDF)
1. Założenie projektu w SEE Electrical (szablon: *Oznaczenia wg rdzenia*).
2. Narysowanie **Arkusza 1 (Zasilanie)**, **Arkusza 2 (Sterowanie i Tryb Ręczny/Auto)** oraz **Arkusza 3 (PLC i Sygnalizacja)** wg instrukcji z pliku `SEE_ELECTRICAL_GUIDE.md`.
3. Przypisanie kodów katalogowych z pliku `BOM_ELEMENTY.md`.
4. Wykonanie rysunku **Zabudowy aparatury 2D** (szafa sterownicza + drzwi z sygnalizacją).
5. Wygenerowanie zestawień i wydruk całości projektu elektrycznego do pliku PDF[.
6. Złożenie sprawozdania końcowego na bazie pliku `OBLICZENIA_I_OPIS.md`.

### Osoba B: Inżynier SCADA / PLC (InTouch + Algorytm PLC)
1. Utworzenie nowej aplikacji w InTouch (*Modern* lub *Legacy InTouch Application*).
2. Konfiguracja bezpieczeństwa (`Configure Users`: Operator i Automatyk) oraz grup alarmowych (`Grupa1`, `Grupa2`).
3. Zdefiniowanie 21 zmiennych w `Tagname Dictionary` wraz z progami alarmowymi `LoLo, Low, High, HiHi`.
4. Wklejenie i przetestowanie skryptu symulacyjnego w `Application Scripts` (`While Running`).
5. Zbudowanie 3 okien (`Bojler_Glowne`, `Alarmy`, `Haslo`) zgodnie z instrukcją w `INTOUCH_GUIDE.md`.

---

## 3. Scenariusz testowy przed oddaniem projektu (Checklista odbiorowa)

Przed pokazaniem projektu prowadzącemu przetestujcie w InTouch (`Runtime`) następujące kroki:

- [ ] **Test 1 (Kolejność załączania):** Próba kliknięcia `START UKŁADU (AL)` przy wyłączonej pompie obiegowej nie może załączyć układu. Dopiero po włączeniu `Zasilania Głównego` -> `Pompy Obiegowej` (gdy ciśnienie wzrośnie powyżej $2,5\text{ bar}$) kliknięcie `START UKŁADU` załącza przekaźnik `AL` (zmienna `PLC_I3` zmienia stan z `1` na `0`).
- [ ] **Test 2 (Tryb Automatyczny – Dogrzewanie):** W trybie `AUTO` przy temperaturze poniżej $T1$ ($128^\circ\text{C}$) co 20 sekund (lub szybciej przy przyspieszonej symulacji) licznik `Krok_Licznika` rośnie od 0 do 15, załączając binarnie sekcje `Q1` (2 kW), `Q2` (4 kW), `Q3` (8 kW), `Q4` (16 kW).
- [ ] **Test 3 (Strefa nieczułości i Próg T2):** Gdy temperatura wejdzie pomiędzy $T1$ ($128^\circ\text{C}$) a $T2$ ($132^\circ\text{C}$), licznik zatrzymuje się, utrzymując aktualną moc grzania. Gdy przekroczy $T2$ ($132^\circ\text{C}$), licznik zlicza „w tył”, redukując moc.
- [ ] **Test 4 (Tryb Ręczny bez PLC):** Po przełączeniu w tryb `RĘCZNY` licznik PLC jest nieaktywny, a operator może ręcznie klikać przyciski 4 sekcji grzałek (`2 kW`, `4 kW`, `8 kW`, `16 kW`), zmieniając moc grzania.
- [ ] **Test 5 (Zabezpieczenia awaryjne):** Obniżenie suwakiem ciśnienia poniżej $p_{min} = 2,5\text{ bar}$ lub podniesienie powyżej $p_{max} = 5,5\text{ bar}$ powoduje natychmiastowe odpadnięcie przekaźnika `AL`, ustawienie wejścia `PLC_I3 = 1`, wyzerowanie licznika, odcięcie wszystkich grzałek (zarówno w trybie Auto, jak i Ręcznym!) oraz wyzwolenie alarmu optycznego i akustycznego.
- [ ] **Test 6 (Poziomy dostępu):** Zalogowanie jako `Operator` (`AccessLevel = 5000`) blokuje edycję progów $T1, T2$ i limitów alarmowych. Zalogowanie jako `Automatyk` (`AccessLevel = 9001`) odblokowuje te pola.
