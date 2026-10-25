# Przewodnik Krok po Kroku: Projekt w SEE Electrical

Ten dokument prowadzi krok po kroku przez wykonanie pełnej dokumentacji elektrycznej w programie **SEE Electrical V8R4** zgodnie z instrukcjami laboratoryjnymi.

---

## Krok 0: Założenie projektu i konfiguracja
- [ ] Uruchom SEE Electrical i wybierz `Plik -> Nowy`.
- [ ] Nazwij projekt `Projekt11_Bojler` i wybierz szablon: **Oznaczenia wg rdzenia**.
- [ ] W oknie `Informacje o projekcie` wpisz w polu *Opis projektu 01*: `Układ regulacji temperatury dla bojlera`, a w polu *Projektował*: Wasze nazwiska.
- [ ] Upewnij się, że siatka na pasku narzędzi jest ustawiona na **5.00 mm**, a w zakładce `Schematyka` włączona jest opcja **Połączenia automatyczne**.

---

## Krok 1: Arkusz 0001 – Zasilanie główne, Pompa i 4 Sekcje Grzałek
- [ ] Kliknij prawym przyciskiem myszy na `Schematy zasadnicze -> Nowy`. W polu *Opis strony 01* wpisz: `Obwód główny zasilania (Pompa i Grzałki)`.
- [ ] **Linie potencjałowe:**
  - [ ] Wciśnij `F11` (Linia potencjałowa górna) i wstaw kolejno: **L1**, **L2**, **L3**.
  - [ ] Zmień styl linii na kreskowy niebieski i wciśnij `F12` (Linia dolna) -> wstaw **N**.
  - [ ] Zmień styl linii na kreskowo-punktowy zielony (`F12`) -> wstaw **PE**. Przywróć kolor czarny i linię ciągłą.
- [ ] **Kolumna 1 (Wejście zasilania):**
  - [ ] Wstaw rozłącznik główny **Q0** (`Symbole wg kodu -> APATOR -> Rozłączniki -> RAB 000 P3`) oraz wyłącznik różnicowoprądowy **F0**.
- [ ] **Kolumna 2 (Pompa obiegowa M1 – 1,5 kW):**
  - [ ] Z biblioteki `Norma EN60617 -> Silniki i generatory` wstaw na dole `Silnik 3faz. + PE`. Nadaj oznaczenie **M1**, opis: `Pompa obiegowa 1,5kW`.
  - [ ] Nad silnikiem wstaw 4 zaciski `1 zacisk 90° pionowy` (listwa **X1**, zaciski 1, 2, 3, 4).
  - [ ] Nad zaciskami wstaw zestyki główne stycznika (`Zestyki styczników 2P, 3P, 4P -> 3P-ZZ mocy 2`) i oznacz jako **KM0**.
  - [ ] Nad stycznikiem wstaw wyłącznik silnikowy 3P i oznacz jako **Q_P** (opis: `PKZM0-4 (3,15A)`).
- [ ] **Kolumny 4, 6, 8, 10 (4 Sekcje Grzałek – 2 kW, 4 kW, 8 kW, 16 kW):**
  - [ ] W **Kolumnie 4** wstaw od góry: wyłącznik nadprądowy 3P **F1** (opis: `B6A`), pod nim zestyk 3P mocy stycznika **KM1**, niżej zaciski **X1** (5, 6, 7, 8) i na dole symbol odbiornika grzewczego 3-fazowego **E1** (opis: `Sekcja 1: 1x2kW = 2kW`).
  - [ ] Zaznacz całą Kolumnę 4 (od F1 do E1), przytrzymaj `Ctrl` i skopiuj ją do **Kolumny 6**, **Kolumny 8** oraz **Kolumny 10**.
  - [ ] Zmień oznaczenia i opisy w skopiowanych kolumnach[:
    * Kolumna 6: **F2** (`B10A`), **KM2**, **E2** (`Sekcja 2: 2x2kW = 4kW`).
    * Kolumna 8: **F3** (`B16A`), **KM3**, **E3** (`Sekcja 3: 4x2kW = 8kW`).
    * Kolumna 10: **F4** (`B25A`), **KM4**, **E4** (`Sekcja 4: 8x2kW = 16kW`).
- [ ] **Kable:** Użyj `Schematyka -> Kable -> Kabel` i wstaw kable pod zaciskami `X1` dla pompy (`W1`: `YKY 4x1,5`) oraz 4 sekcji grzałek (`W2`: `5x1,5`, `W3`: `5x1,5`, `W4`: `5x2,5`, `W5`: `5x4,0`).

---

## Krok 2: Arkusz 0002 – Obwód Bezpieczeństwa (AL) oraz Sterowanie Ręczne i Auto
- [ ] Utwórz drugi schemat: `Schematy zasadnicze -> Nowy`, opis: `Sterowanie 24V DC, Obwód AL i Tryb Ręczny/Auto`.
- [ ] Wstaw u góry linię potencjałową **+24V** (`F11`) oraz na dole linię **0V** (`F12`) zasilane z zasilacza **U1** ($230\text{ V AC} / 24\text{ V DC}$, zabezpieczonego wyłącznikami **F5** i **F6**).
- [ ] **Kolumna 2 – Sterowanie Pompą Obiegową (KM0):**
  - [ ] Wstaw szeregowo od góry: styk pomocniczy wyłącznika silnikowego **Q_P** (13-14) -> przycisk rozwierny **S_P_STOP** (`Norma EN60617 -> Rozłączniki 1P -> ZR przycisk powrotny`) -> przycisk zwierny **S_P_START** (`ZZ przycisk powrotny`) -> cewka stycznika **KM0** (`Cewki przekaźników -> 1P przekaźnik`).
  - [ ] Równolegle do przycisku `S_P_START` dodaj styk podtrzymujący `ZZ pomocniczy` o oznaczeniu **KM0** (13-14).
- [ ] **Kolumna 4 – Obwód Przekaźnika Alarmowego AL (Zabezpieczenia):**
  - [ ] Wstaw szeregowo od góry:
    1. Przycisk awaryjny **S_STOP_AW** (NC),
    2. Styk pomocniczy zwierny stycznika pompy **KM0** (23-24) – *gwarantuje, że bez pompy nie załączymy grzałek!*,
    3. Styk presostatu ciśnienia minimalnego **B_pmin**,
    4. Styk presostatu ciśnienia maksymalnego **B_pmax**,
    5. Styk termostatu przegrzania **B_Tmax**,
    6. Przycisk **S_STOP** (NC),
    7. Przycisk **S_START** (NO) z równoległym stykiem podtrzymania **AL** (13-14),
    8. Cewkę przekaźnika **AL**.
- [ ] **Kolumny 6–9 – Wybór Trybu (Ręczny bez PLC / Automatyczny z PLC) i Styczniki KM1–KM4:**
  - [ ] Pod linią `+24V` wstaw styk zwierny **AL** (23-24) – *odcina zasilanie wszystkich styczników grzałek w razie awarii zarówno w trybie Auto, jak i Ręcznym!*.
  - [ ] Pod stykiem `AL` wstaw przełącznik wyboru trybu **SA1** (rozdzielający napięcie na linię `TRYB_AUTO` oraz linię `TRYB_RECZNY`).
  - [ ] W kolumnach 6, 7, 8, 9 na dole umieść cewki styczników mocy **KM1**, **KM2**, **KM3**, **KM4** (przypisz im kody katalogowe z tabeli BOM, aby wygenerowały się pod nimi krzyże zestyków).
  - [ ] Nad każdą cewką `KM1`–`KM4` narysuj dwie równoległe gałęzie:
    * Gałąź z linii `TRYB_AUTO`: przez styk zwierny przekaźnika PLC odpowiednio **K1** (13-14), **K2** (13-14), **K3** (13-14), **K4** (13-14).
    * Gałąź z linii `TRYB_RECZNY`: przez przycisk bistabilny ręcznego załączania odpowiednio **S_R1**, **S_R2**, **S_R3**, **S_R4** *(dzięki temu w trybie ręcznym grzałki są sterowane z pominięciem PLC!)*.

---

## Krok 3: Arkusz 0003 – Sterownik PLC oraz Sygnalizacja Optyczna i Akustyczna
- [ ] Utwórz trzeci schemat: `Schematy zasadnicze -> Nowy`, opis: `Sterownik PLC oraz Sygnalizacja`.
- [ ] **Część lewa – Sterownik PLC (wg instrukcji `PLC.pdf`):**
  - [ ] Przejdź do `Symbole -> PLC -> Moduły cyfrowe - DI` i wstaw `Etykieta karty, 08-1` (nadaj oznaczenie **A1-I01** i dobierz z bazy `Db` kod karty wejść np. `TSX DEY 08D2`). Obok wstaw kartę wyjść `Moduły cyfrowe - DO` (oznaczenie **A1-Q01**).
  - [ ] Wstaw 3 symbole wejść (`Kanał - 1, góra`) i powiąż je z kartą **A1-I01**[:
    * **Kanał 1 (I1):** podłącz styk 1 przetwornika temperatury **U_T1** (z czujnikiem **B_T1** Pt100). Wpisz komentarz: `PROG T1 - TEMP PONIZEJ T1`.
    * **Kanał 2 (I2):** podłącz styk 2 przetwornika **U_T1**. Wpisz komentarz: `PROG T2 - TEMP POWYZEJ T2`.
    * **Kanał 3 (I3):** podłącz **styk rozwierny (NC)** przekaźnika **AL** (31-32) (`Zestyki przekaźników ZR -> ZR pomocniczy`). Wpisz komentarz: `STAN AL (0=OK, 1=AWARIA/RESET)` *(zgodnie z wymogiem zadania: gdy AL działa, rozwiera styk i daje "0" na I3; gdy AL odpadnie, styk zwiera się i podaje "1" na I3!)*.
  - [ ] Wstaw 4 symbole wyjść (`Moduły cyfrowe - DO -> Kanał - 1, dół`) powiązane z kartą **A1-Q01** (kanały 1, 2, 3, 4 jako **Q1, Q2, Q3, Q4**) i podłącz pod nie cewki przekaźników pośredniczących **K1, K2, K3, K4**.
- [ ] **Część prawa – Sygnalizacja optyczna i akustyczna:**
  - [ ] Z folderu `Norma EN60617 -> Lampki` wstaw lampki sygnalizacyjne:
    * **H1 (Zielona):** za stykiem pomocniczym `KM0` (opis: `Praca pompy`).
    * **H2 (Zielona):** za stykiem zwiernym `AL` (opis: `Układ sprawny - Praca`).
    * **H3 (Czerwona)** oraz równolegle buczek **HA1** (`Sygnalizatory akustyczne`): za stykiem rozwiernym (NC) przekaźnika **AL** (41-42) (opis: `ALARM / AWARIA UKŁADU`).
    * **H4, H5, H6, H7 (Żółte):** za stykami pomocniczymi styczników `KM1, KM2, KM3, KM4` (sygnalizacja pracy sekcji 2 kW, 4 kW, 8 kW, 16 kW).

---

## Krok 4: Zabudowa Aparatury 2D (Rozdzielnica + Drzwi z sygnalizacją)
- [ ] Kliknij prawym przyciskiem myszy na `Zabudowa aparatury 2D -> Nowy`, wpisz opis: `Rozmieszczenie elementów w rozdzielnicy i sygnalizacji`.
- [ ] Z zakładki `Zabudowa aparatury` kliknij **Szafa**, kliknij na arkuszu, naciśnij `Spację` i podaj wymiary: `Dx = 600`, `Dy = 800`. Wstaw obok drugi taki sam prostokąt (`600 x 800`) – lewy przedstawia płytę montażową wewnątrz szafy, a prawy drzwi rozdzielnicy (pulpit operatorski).
- [ ] W lewej szafie wstaw **Korytka kablowe** (szerokość `40 mm`) oraz 4 poziome **Szyny montażowe** (szerokość `35 mm`, długość `500 mm`).
- [ ] Kliknij `Przetwarzanie -> Lista symboli do wstawienia`:
  - [ ] **Szyna 1 (góra):** Rozłącznik `Q0`, różnicówka `F0`, wyłącznik pompy `Q_P`, wyłączniki nadprądowe `F1, F2, F3, F4, F5, F6`.
  - [ ] **Szyna 2 (środek-góra):** Zasilacz `U1`, sterownik PLC `A1` (`A1-I01`, `A1-Q01`), przekaźniki `AL, K1, K2, K3, K4`.
  - [ ] **Szyna 3 (środek-dół):** Styczniki mocy `KM0, KM1, KM2, KM3, KM4`.
  - [ ] **Szyna 4 (dół):** Listwa zaciskowa `X1`.
  - [ ] **Drzwi szafy (prawy prostokąt):** Wstaw w równych rzędach lampki `H1–H7`, sygnalizator akustyczny `HA1`, przetwornik tablicowy `U_T1`, przełącznik trybu `SA1`, przyciski `S_P_START`, `S_P_STOP`, `S_START`, `S_STOP`, przyciski ręczne `S_R1–S_R4` oraz grzybek `S_STOP_AW`.
- [ ] Zwymiaruj szafę narzędziem `Rysuj -> Wymiar -> Między 2 liniami`.

---

## Krok 5: Generowanie Zestawień i Eksport do PDF
- [ ] W drzewie projektu rozwiń `Zestawienia`.
- [ ] Kliknij prawym przyciskiem myszy i wybierz **Generuj** kolejno dla:
  - [ ] `Zestawienie materiałów` (lub `Zestawienie aparatury`),
  - [ ] `Zestawienie zacisków`,
  - [ ] `Zestawienie kanałów PLC`,
  - [ ] `Zestawienie dokumentów` (zawsze jako ostatnie!).
- [ ] Wybierz `Plik -> Drukowanie -> Drukuj`, wybierz drukarkę `Microsoft Print to PDF`, zaznacz `Wszystko` i wygeneruj gotowy plik PDF z dokumentacją.
