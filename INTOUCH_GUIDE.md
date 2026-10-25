# Przewodnik Krok po Kroku: System SCADA w AVEVA / Wonderware InTouch

Kompletna instrukcja wykonania aplikacji wizualizacyjnej dla **Projektu 11 (Bojler)**, spełniającej wszystkie wymagania z instrukcji laboratoryjnych (`Pierwsze kroki`, `Alarmy.pdf`, `Logowanie.pdf`).

---

## Krok 1: Utworzenie aplikacji i konfiguracja Bezpieczeństwa (2 Użytkowników)
*(Na podstawie pliku `Logowanie.pdf`)*

- [ ] Uruchom InTouch Application Manager -> `New` -> wybierz `Modern InTouch Application` (lub `Legacy`) i nazwij aplikację `Projekt11_Bojler`. Otwórz ją w **WindowMaker**.
- [ ] Z górnego menu wybierz `Special -> Security -> Select Security Type` i zaznacz **InTouch**.
- [ ] Wybierz `Special -> Security -> Log On...` i wpisz domyślne dane administratora:
  * **Name:** `administrator`
  * **Password:** `wonderware`
- [ ] Wybierz `Special -> Security -> Configure Users...` i dodaj dwóch wymaganych użytkowników:
  1. **User Name:** `Automatyk` | **Password:** `auto123` | **Access Level:** `9001` *(pełny dostęp do zmiany nastaw $T_{zad}, T1, T2$, progów alarmowych i konfiguracji)*
  2. **User Name:** `Operator` | **Password:** `oper123` | **Access Level:** `5000` *(obsługa bieżąca układu, zmiana trybu, załączanie ręczne, potwierdzanie alarmów)*
- [ ] Wejdź w `Special -> Configure -> WindowViewer...` i ustaw czasy nieaktywności:
  * **Inactivity Warning:** `20` (sekund)
  * **Inactivity Timeout:** `40` (sekund)

---

## Krok 2: Definicja Grup Alarmowych
*(Na podstawie pliku `Alarmy.pdf`)*

- [ ] Wybierz z menu `Special -> Alarm Groups...`.
- [ ] Kliknij `Add` i utwórz dwie grupy podrzędne względem `$System`:
  * **`grupa1`** *(Alarmy procesowe: temperatura i ciśnienie w bojlerze)*
  * **`grupa2`** *(Alarmy wykonawcze i zabezpieczeń: przekaźnik AL, pompa)*

---

## Krok 3: Definicja Zmiennych w `Tagname Dictionary` (21 tagów)
- [ ] Otwórz `Special -> Tagname Dictionary`, klikaj `New` i zdefiniuj poniższe 21 zmiennych (zaznacz u góry opcję `Details & Alarms`, aby od razu wpisać progi alarmowe!):

| Lp. | Tagname | Type | Group | Min / Max / Init | Konfiguracja Alarmów i Zdarzeń (`Details & Alarms`) |
| :-: | :--- | :--- | :--- | :--- | :--- |
| 1 | **`Temp_Bojler`** | Memory Real | `grupa1` | `0` / `180` / Init: `20` | Zaznacz `Log Events`. Alarmy: **LoLo**=`15` (Pri: 500), **Low**=`128` (Pri: 250), **High**=`132` (Pri: 250), **HiHi**=`145` (Pri: 1). |
| 2 | **`Cisnienie`** | Memory Real | `grupa1` | `0` / `10` / Init: `1.0` | Zaznacz `Log Events`. Alarmy: **LoLo**=`2.5` (Pri: 1 - $p_{min}$!), **Low**=`2.8` (Pri: 250), **High**=`5.0` (Pri: 250), **HiHi**=`5.5` (Pri: 1 - $p_{max}$!). |
| 3 | **`T_zad`** | Memory Real | `$System` | `20` / `140` / Init: `130` | Temperatura zadana ($130^\circ\text{C}$). Zaznacz `Log Events`. |
| 4 | **`T1_Prog`** | Memory Real | `$System` | `20` / `140` / Init: `128` | Dolny próg komparatora ($T1 < T_{zad}$). |
| 5 | **`T2_Prog`** | Memory Real | `$System` | `20` / `145` / Init: `132` | Górny próg komparatora ($T2 > T_{zad}$). |
| 6 | **`Wsp_Suszarek`**| Memory Real | `$System` | `0` / `1.0` / Init: `0.5` | Współczynnik jednoczesności pracy 3 suszarek ($0\dots 1$). |
| 7 | **`Zasilanie_Gl`**| Memory Discrete | `grupa2` | Init: `Off` (0) | Załączenie zasilania głównego 3-fazowego (`Log Events`). |
| 8 | **`Pompa_Obieg`** | Memory Discrete | `grupa2` | Init: `Off` (0) | Praca pompy obiegowej wody (`Log Events`). |
| 9 | **`Przekaznik_AL`**| Memory Discrete | `grupa2` | Init: `Off` (0) | Stan przekaźnika bezpieczeństwa `AL` (`1`=Praca OK, `0`=Stop/Awaria). |
| 10 | **`Tryb_Auto`** | Memory Discrete | `grupa2` | Init: `Off` (0) | `0` = Tryb Ręczny (poza PLC), `1` = Tryb Automatyczny (PLC). |
| 11 | **`PLC_I1`** | Memory Discrete | `$System` | Init: `Off` (0) | Wejście PLC `I1` z komparatora (Próg 1: $T < T1$). |
| 12 | **`PLC_I2`** | Memory Discrete | `$System` | Init: `Off` (0) | Wejście PLC `I2` z komparatora (Próg 2: $T > T2$). |
| 13 | **`PLC_I3`** | Memory Discrete | `grupa2` | Init: `On` (1) | Wejście PLC `I3` z przekaźnika `AL` (`0`=OK, `1`=Awaria/Reset). **Alarm State:** `On`, Priority: `1`, Comment: `AWARIA - ODPADNIECIE PRZEKAZNIKA AL!`. |
| 14 | **`Krok_Licznika`**| Memory Integer | `$System` | `0` / `15` / Init: `0` | Aktualny krok rewersyjnego licznika binarnego (0–15). |
| 15 | **`Zegar_Kroku`** | Memory Integer | `$System` | `0` / `20` / Init: `0` | Licznik czasu kroku generatora zegarowego (0–20 s). |
| 16 | **`Sekcja_Q1_2kW`**| Memory Discrete | `$System` | Init: `Off` (0) | Załączenie Sekcji 1 (`K1` / `KM1` – 1 grzałka = 2 kW). |
| 17 | **`Sekcja_Q2_4kW`**| Memory Discrete | `$System` | Init: `Off` (0) | Załączenie Sekcji 2 (`K2` / `KM2` – 2 grzałki = 4 kW). |
| 18 | **`Sekcja_Q3_8kW`**| Memory Discrete | `$System` | Init: `Off` (0) | Załączenie Sekcji 3 (`K3` / `KM3` – 4 grzałki = 8 kW). |
| 19 | **`Sekcja_Q4_16kW`**| Memory Discrete| `$System` | Init: `Off` (0) | Załączenie Sekcji 4 (`K4` / `KM4` – 8 grzałek = 16 kW). |
| 20 | **`Moc_Grzalek`** | Memory Integer | `$System` | `0` / `30` / Init: `0` | Aktualna załączona moc grzałek [kW]. |
| 21 | **`od`, `do`, `typ_alarmu`, `typ_alarmu_nazwa`** | wg ćw. 5 | `$System` | `od`=1, `do`=999 | 4 zmienne pomocnicze z instrukcji `Alarmy.pdf` (`od`, `do`: Memory Integer; `typ_alarmu`, `typ_alarmu_nazwa`: Memory Message). *(Łącznie 24 zmienne < 32!)* |

---

## Krok 4: Skrypt Symulacyjny (`Application Script`)
- [ ] Wybierz z menu `Special -> Scripts -> Application Scripts...`.
- [ ] Ustaw **Condition Type:** `While Running`, a w polu **Every:** wpisz `500` Msec (lub `1000` Msec).
- [ ] Wklej poniższy kod i kliknij `Validate`, a następnie `OK`:

```pascal
{ 1. LOGIKA ZASILANIA I POMPY OBIEGOWEJ }
IF Zasilanie_Gl == 0 THEN
    Pompa_Obieg = 0;
    Przekaznik_AL = 0;
ENDIF;

{ 2. ZABEZPIECZENIA (pmin = 2.5 bar, pmax = 5.5 bar, Tmax = 145 C) }
IF (Pompa_Obieg == 0) OR (Cisnienie < 2.5) OR (Cisnienie > 5.5) OR (Temp_Bojler >= 145.0) THEN
    Przekaznik_AL = 0;
ENDIF;

{ Odwzorowanie styku NC przekaźnika AL na wejście I3 sterownika PLC }
IF Przekaznik_AL == 1 THEN
    PLC_I3 = 0; { Normalna praca -> I3 = 0 }
ELSE
    PLC_I3 = 1; { Awaria / Wyłączenie -> I3 = 1, zerowanie licznika! }
    Krok_Licznika = 0;
ENDIF;

{ 3. KOMPARATOR TEMPERATURY PT100 (Progi T1 i T2) }
IF Temp_Bojler < T1_Prog THEN
    PLC_I1 = 1;
ELSE
    PLC_I1 = 0;
ENDIF;

IF Temp_Bojler > T2_Prog THEN
    PLC_I2 = 1;
ELSE
    PLC_I2 = 0;
ENDIF;

{ 4. PRACA W TRYBIE AUTOMATYCZNYM (Z udziałem licznika PLC) }
IF Tryb_Auto == 1 THEN
    IF PLC_I3 == 0 THEN
        Zegar_Kroku = Zegar_Kroku + 1;
        IF Zegar_Kroku >= 20 THEN
            Zegar_Kroku = 0;
            { Zliczanie DO PRZODU gdy T < T1 }
            IF (PLC_I1 == 1) AND (Krok_Licznika < 15) THEN
                Krok_Licznika = Krok_Licznika + 1;
            ENDIF;
            { Zliczanie W TYŁ gdy T > T2 }
            IF (PLC_I2 == 1) AND (Krok_Licznika > 0) THEN
                Krok_Licznika = Krok_Licznika - 1;
            ENDIF;
        ENDIF;

        { Dekodowanie licznika 0..15 na wyjścia binarne Q4, Q3, Q2, Q1 }
        Sekcja_Q4_16kW = (Krok_Licznika >= 8);
        Sekcja_Q3_8kW  = ((Krok_Licznika MOD 8) >= 4);
        Sekcja_Q2_4kW  = ((Krok_Licznika MOD 4) >= 2);
        Sekcja_Q1_2kW  = ((Krok_Licznika MOD 2) == 1);
    ELSE
        Zegar_Kroku = 0;
        Sekcja_Q1_2kW = 0;
        Sekcja_Q2_4kW = 0;
        Sekcja_Q3_8kW = 0;
        Sekcja_Q4_16kW = 0;
    ENDIF;
ELSE
    { 5. PRACA W TRYBIE RĘCZNYM (Bez udziału PLC, ale chroniona przez AL!) }
    Zegar_Kroku = 0;
    Krok_Licznika = 0;
    IF Przekaznik_AL == 0 THEN
        Sekcja_Q1_2kW = 0;
        Sekcja_Q2_4kW = 0;
        Sekcja_Q3_8kW = 0;
        Sekcja_Q4_16kW = 0;
    ENDIF;
ENDIF;

{ 6. OBLICZENIE MOCY GRZANIA I SYMULACJA TEMPERATURY W BOJLERZE }
Moc_Grzalek = (Sekcja_Q1_2kW * 2) + (Sekcja_Q2_4kW * 4) + (Sekcja_Q3_8kW * 8) + (Sekcja_Q4_16kW * 16);

Temp_Bojler = Temp_Bojler + (Moc_Grzalek * 0.10) - (Wsp_Suszarek * 1.8) - ((Temp_Bojler - 20.0) * 0.005);
IF Temp_Bojler < 20.0 THEN
    Temp_Bojler = 20.0;
ENDIF;
```

---

## Krok 5: Budowa 3 Okien Wizualizacji

### Okno 1: `Bojler_Glowne` (Ekran synoptyczny i sterowniczy)
- [ ] Utwórz okno `File -> New Window`, Name: `Bojler_Glowne`, Type: `Replace`, Width: `1280`, Height: `800`.
- [ ] **Wizualizacja Bojlera, Pompy i Suszarek:**
  - [ ] Wstaw zbiornik z `ArchestrA Symbol Library -> Vessels -> SSTankCylinderVertFill` (lub z `Wizards -> Tanks`) i przypisz mu zmienną `Temp_Bojler`.
  - [ ] Wstaw rurociągi (`Pipes`), pompę obiegową (`Pumps` -> powiąż z `Pompa_Obieg`) oraz 3 bloki symbolizujące 3 suszarki odbierające ciepło.
- [ ] **Sekwencja Załączania Układu (4 przyciski po lewej stronie):**
  - [ ] Przycisk **1. ZASILANIE GŁÓWNE 3x400V**: `Touch Pushbuttons -> Discrete Value -> Toggle`, Tag: `Zasilanie_Gl`.
  - [ ] Przycisk **2. POMPA OBIEGOWA**: `Touch Pushbuttons -> Action` (When Pushed):
    ```pascal
    IF Zasilanie_Gl == 1 THEN
        IF Pompa_Obieg == 0 THEN
            Pompa_Obieg = 1;
            Cisnienie = 3.8; { Wytworzenie ciśnienia roboczego przez pompę }
        ELSE
            Pompa_Obieg = 0;
            Cisnienie = 1.2;
        ENDIF;
    ENDIF;
    ```
  - [ ] Przycisk **3. START UKŁADU REGULACJI (AL)**: `Touch Pushbuttons -> Action`:
    ```pascal
    IF (Zasilanie_Gl == 1) AND (Pompa_Obieg == 1) AND (Cisnienie >= 2.5) AND (Cisnienie <= 5.5) AND (Temp_Bojler < 145.0) THEN
        Przekaznik_AL = 1;
    ENDIF;
    ```
  - [ ] Przycisk **4. STOP UKŁADU**: `Touch Pushbuttons -> Action`: `Przekaznik_AL = 0;`.
- [ ] **Wybór Trybu i Sekcje Grzałek (Tryb Ręczny vs Auto):**
  - [ ] Przycisk **TRYB PRACY (RĘCZNY / AUTO)**: `Discrete Value -> Toggle`, Tag: `Tryb_Auto` (On Msg: `TRYB AUTOMATYCZNY (PLC)`, Off Msg: `TRYB RĘCZNY (POZA PLC)`).
  - [ ] Wstaw 4 przyciski dla sekcji grzałek: **K1 (2 kW)**, **K2 (4 kW)**, **K3 (8 kW)**, **K4 (16 kW)**.
  - [ ] Dla każdego z tych 4 przycisków ustaw:
    * `Touch Pushbuttons -> Discrete Value -> Toggle` odpowiednio dla `Sekcja_Q1_2kW`, `Sekcja_Q2_4kW`, `Sekcja_Q3_8kW`, `Sekcja_Q4_16kW`.
    * `Fill Color -> Discrete` dla tych samych tagów (Zielony gdy On, Szary gdy Off).
    * **Kluczowe:** Zaznacz `Miscellaneous -> Disable` i wpisz warunek: `(Tryb_Auto == 1) OR (Przekaznik_AL == 0)`. Dzięki temu w trybie Ręcznym klikasz w nie i ręcznie sterujesz grzałkami, a w trybie Auto steruje nimi wyłącznie licznik PLC!
- [ ] **Suwaki Symulacji (do demonstracji działania na zajęciach):**
  - [ ] Wstaw suwak (`Wizards -> Sliders -> Vertical Slider`) dla zmiennej **`Wsp_Suszarek`** (zakres `0` do `1`) – podpisz: *Obciążenie cieplne suszarek (0..1)*.
  - [ ] Wstaw drugi suwak dla zmiennej **`Cisnienie`** (zakres `0` do `10`) – podpisz: *Symulacja ciśnienia obiegu [bar] (pmin=2.5, pmax=5.5)*. Pozwoli Ci to jednym ruchem suwaka zasymulować pęknięcie rury ($p < 2,5$) lub wzrost ciśnienia ($p > 5,5$) i pokazać zadziałanie przekaźnika `AL`!
- [ ] **Panel Nastaw Progu T1 i T2 (Tylko dla Automatyka!):**
  - [ ] Wstaw pola tekstowe `#.#` z animacją `User Inputs -> Analog` dla zmiennych `T_zad`, `T1_Prog`, `T2_Prog` oraz zaznacz w nich opcję **Disable** z warunkiem: `$AccessLevel < 9000`.
- [ ] Dodaj na dole przyciski nawigacyjne `Show Window` otwierające okno **`Alarmy`** oraz okno **`Haslo`**.

---

### Okno 2: `Haslo` (Okno Logowania Użytkowników)
*(Dokładnie wg instrukcji `Logowanie.pdf`, str. 2–3)*

- [ ] Utwórz okno `Haslo` (Type: `Popup`, rozmiar np. `400 x 350`).
- [ ] Wstaw 4 przyciski, 3 teksty `#` i 2 lampki i skonfiguruj je dokładnie wg instrukcji laboratoryjnej:
  * Przycisk **Nazwisko operatora**: `Touch Links -> User Inputs -> String`, Tagname: `$OperatorEntered`, Input Only: `Yes`.
  * Przycisk **Hasło**: `Touch Links -> User Inputs -> String`, Tagname: `$PasswordEntered`, Input Only: `Yes`.
  * Przycisk **Wylogowanie**: `Touch Pushbuttons -> Action`:
    ```pascal
    $OperatorEntered = "None";
    $PasswordEntered = "";
    ```
   
  * Przycisk **OK**: `Touch Pushbuttons -> Hide Window` -> wybierz okno `Haslo`.
  * Górny tekst `#`: `Value Display -> String`, Expression: `$Operator`.
  * Środkowy tekst `#`: `Value Display -> String`, Expression: `$PasswordEntered`.
  * Dolny tekst `#`: `Value Display -> Analog`, Expression: `$AccessLevel`.
  * Lewa lampka: `Fill Color -> Discrete`, Expression: `$InactivityWarning`.
  * Prawa lampka: `Fill Color -> Discrete`, Expression: `$InactivityTimeout`.

---

### Okno 3: `Alarmy` (System Alarmów i Zdarzeń Bieżących oraz Historycznych)
*(Dokładnie wg instrukcji `Alarmy.pdf`, str. 2–4)*

- [ ] Utwórz okno `Alarmy` (Type: `Replace` lub `Overlay`, rozmiar `1280 x 800`).
- [ ] Wstaw obiekt `Wizards -> Alarm Displays -> Dist. Alarm Display`. Kliknij na niego dwukrotnie, upewnij się, że ma nazwę **`ALMOBJ_1`**, a w polu `Alarm Query` wpisz `\InTouch!$System`.
- [ ] Wstaw przyciski przełączania trybu alarmów (`Touch Pushbuttons -> Action`):
  * Przycisk **Alarmy bieżące**:
    ```pascal
    typ_alarmu = "summ";
    typ_alarmu_nazwa = "bieżące";
    almQuery("ALMOBJ_1", "\InTouch!$System", od, do, "all", typ_alarmu);
    ```
   
  * Przycisk **Alarmy historyczne**:
    ```pascal
    typ_alarmu = "hist";
    typ_alarmu_nazwa = "historyczne";
    almQuery("ALMOBJ_1", "\InTouch!$System", od, do, "all", typ_alarmu);
    ```
   
- [ ] Wstaw 3 przyciski filtrowania grup alarmowych:
  * Przycisk **Grupa 1 (Bojler)**: `almQuery("ALMOBJ_1", "\InTouch!grupa1", od, do, "all", typ_alarmu);`
  * Przycisk **Grupa 2 (Sterowanie)**: `almQuery("ALMOBJ_1", "\InTouch!grupa2", od, do, "all", typ_alarmu);`
  * Przycisk **$System (Wszystkie)**: `almQuery("ALMOBJ_1", "\InTouch!$System", od, do, "all", typ_alarmu);`
- [ ] Wstaw przycisk **Potwierdzenie wszystkich alarmów** (`Touch Pushbuttons -> Action`):
  ```pascal
  almAckAll("ALMOBJ_1", "Potwierdzono przez operatora");
  ```
 
- [ ] Wstaw pola edycyjne `Od #` (`User Inputs -> Analog`, Tag: `od`) oraz `Do #` (Tag: `do`), a także przyciski zmiany progów alarmowych `Temp_Bojler.HiHiLimit`, `Temp_Bojler.HiLimit`, `Temp_Bojler.LoLimit`, `Temp_Bojler.LoLoLimit` (z blokadą `Disable`: `$AccessLevel < 9000` – edycja tylko dla Automatyka).
- [ ] Dodaj przycisk **Powrót do ekranu Bojlera** (`Show Window -> Bojler_Glowne`).
