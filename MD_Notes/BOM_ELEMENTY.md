# Spis Elementów (BOM) oraz Lista Wejść/Wyjść PLC

Dokument zawiera ujednolicone oznaczenia aparatów na schematach SEE Electrical wraz z kodami katalogowymi oraz przypisaniem adresów sterownika PLC.

---

## 1. Zestawienie aparatury elektrycznej i pomiarowej (BOM)

| Oznaczenie | Nazwa elementu | Parametry techniczne | Funkcja w układzie | Producent / Kod katalogowy | Ilość |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **Q0** | Rozłącznik główny | 3P, $63\text{ A}$, montaż tablicowy/szyna | Załączenie zasilania głównego $3\times 400\text{ V}$ | Apator `RAB 000 P3` (lub Eaton `P3-63/EA/SVB`) | 1 |
| **F0** | Wyłącznik różnicowoprądowy | 4P, $63\text{ A}$, $I_{\Delta n} = 30\text{ mA}$, typ A | Ochrona przeciwporażeniowa dodatkowa | ETI `EFI-4 63/0.03 A` | 1 |
| **Q_P** | Wyłącznik silnikowy | 3P, zakres $2,5\dots 4,0\text{ A}$, ze stykiem pom. 1NO+1NC | Zabezpieczenie zwarciowe i przeciążeniowe pompy `M1` | Eaton `PKZM0-4` + `NHI11-PKZ0` | 1 |
| **F1** | Wyłącznik nadprądowy | 3P, **B6A**, $6\text{ kA}$ | Zabezpieczenie Sekcji 1 grzałek ($2\text{ kW}$) | ETI `ETIMAT 6 3p B6` | 1 |
| **F2** | Wyłącznik nadprądowy | 3P, **B10A**, $6\text{ kA}$ | Zabezpieczenie Sekcji 2 grzałek ($4\text{ kW}$) | ETI `ETIMAT 6 3p B10` | 1 |
| **F3** | Wyłącznik nadprądowy | 3P, **B16A**, $6\text{ kA}$ | Zabezpieczenie Sekcji 3 grzałek ($8\text{ kW}$) | ETI `ETIMAT 6 3p B16` | 1 |
| **F4** | Wyłącznik nadprądowy | 3P, **B25A**, $6\text{ kA}$ | Zabezpieczenie Sekcji 4 grzałek ($16\text{ kW}$) | ETI `ETIMAT 6 3p B25` | 1 |
| **F5** | Wyłącznik nadprądowy | 1P, **B6A**, $6\text{ kA}$ | Zabezpieczenie zasilacza $230\text{ V AC}$ | ETI `ETIMAT 6 1p B6` | 1 |
| **F6** | Wyłącznik nadprądowy | 1P, **C2A**, $6\text{ kA}$ | Zabezpieczenie obwodów sterowania $24\text{ V DC}$ | ETI `ETIMAT 6 1p C2` | 1 |
| **KM0** | Stycznik mocy | 3P, $9\text{ A}$, cewka $24\text{ V DC}$, 2NO | Załączanie silnika pompy obiegowej `M1` | Eaton `DILM9-10(24VDC)` + styk pom. | 1 |
| **KM1, KM2** | Styczniki mocy | 3P, $12\text{ A}$, cewka $24\text{ V DC}$, 1NO | Załączanie Sekcji 1 ($2\text{ kW}$) i Sekcji 2 ($4\text{ kW}$) | Eaton `DILM12-10(24VDC)` | 2 |
| **KM3** | Stycznik mocy | 3P, $17\text{ A}$, cewka $24\text{ V DC}$, 1NO | Załączanie Sekcji 3 ($8\text{ kW}$) | Eaton `DILM17-10(24VDC)` | 1 |
| **KM4** | Stycznik mocy | 3P, $32\text{ A}$, cewka $24\text{ V DC}$, 1NO | Załączanie Sekcji 4 ($16\text{ kW}$) | Eaton `DILM32-10(24VDC)` | 1 |
| **K1 – K4** | Przekaźniki elektromagnetyczne | Cewka $24\text{ V DC}$, 2P (2CO), $8\text{ A}$ | Przekaźniki pośredniczące wyjść PLC `Q1`–`Q4` | Relpol `RM84-2012-35-1024` + `GZT80` | 4 |
| **AL** | Przekaźnik bezpieczeństwa/alarmowy | Cewka $24\text{ V DC}$, 4P (4CO), $6\text{ A}$ | Nadzór warunków bezpieczeństwa i reset PLC | Relpol `R4N-2014-23-1024` + `GZT4` | 1 |
| **U1** | Zasilacz impulsowy | $230\text{ V AC} / 24\text{ V DC}$, $5\text{ A}$ ($120\text{ W}$) | Zasilanie PLC, przekaźników, styczników i lampek | MeanWell `MDR-100-24` | 1 |
| **A1** | Sterownik programowalny PLC | $24\text{ V DC}$, 8 DI / 6 DO | Jednostka centralna sterownika (z licznikiem binarnym) | Siemens `6ES7212-1AE40-0XB0` (lub `TSX DEY 08D2`) | 1 |
| **B_T1** | Czujnik temperatury Pt100 | Pt100, klasa A, osłona gwintowana $G1/2''$, $L=160\text{ mm}$ | Pomiar aktualnej temperatury wody w bojlerze | Czaki `TP-202-Pt100` | 1 |
| **U_T1** | Przetwornik-komparator temp. | Wejście Pt100, 2 wyjścia przekaźnikowe ($T1, T2$) | Komparator progów $T1 < 130^\circ\text{C}$ i $T2 > 130^\circ\text{C}$ | Lumel `RE72` | 1 |
| **B_Tmax / U_Tmax**| Zabezpieczenie $T_{max}$ (STB) | Czujnik Pt100 + ogranicznik temperatury bezpieczeństwa | Niezależne odłączenie grzałek przy przegrzaniu ($>145^\circ\text{C}$) | Jumo `safetyM STB/STW` | 1 |
| **B_pmin** | Presostat ciśnienia minimalnego | Zakres $0,5 \dots 6,0\text{ bar}$, styk SPDT | Zabezpieczenie przed spadkiem ciśnienia ($p_{min} = 2,5\text{ bar}$) | Danfoss `KP 35 (060-113366)` | 1 |
| **B_pmax** | Presostat ciśnienia maksymalnego | Zakres $2,0 \dots 14,0\text{ bar}$, styk SPDT | Zabezpieczenie przed wzrostem ciśnienia ($p_{max} = 5,5\text{ bar}$) | Danfoss `KP 36 (060-110866)` | 1 |
| **M1** | Silnik pompy obiegowej | $3\times 400\text{ V}$, $1,5\text{ kW}$, $1420\text{ obr./min}$ | Wymuszenie obiegu wody pod ciśnieniem | Tamel `3Sg90L-4-IE2` | 1 |
| **E1 – E4** | Zespół 15 grzałek bojlera | $15 \times 2\text{ kW}$ ($3\times 400\text{ V}$), pogrupowane 1+2+4+8 | Podgrzewanie wody w bojlerze (łącznie $30\text{ kW}$) | Selfa `Zespół grzejny 3x400V` | 4 sek. |
| **SA1** | Przełącznik wyboru trybu | 3-pozycyjny (`RĘCZNY - 0 - AUTO`), $\varnothing 22\text{ mm}$ | Wybór trybu pracy (Ręczny funkcjonuje bez PLC) | Promet `NEF22-P3` / Eaton `M22-WRK3` | 1 |
| **S_P_START, S_START** | Przyciski sterownicze zielone | Napęd powrotny zwierny (1NO), zielony | Załączenie pompy obiegowej / Załączenie układu `AL` | Promet `NEF22H-Kz` | 2 |
| **S_P_STOP, S_STOP** | Przyciski sterownicze czerwone | Napęd powrotny rozwierny (1NC), czerwony | Wyłączenie pompy obiegowej / Wyłączenie układu `AL` | Promet `NEF22H-Kc` | 2 |
| **S_STOP_AW** | Wyłącznik awaryjny E-STOP | Przycisk grzybkowy ryglowany (2NC) | Awaryjne odcięcie układu sterowania | Promet `NEF22-Grzyb` / Eaton `M22-PV` | 1 |
| **S_R1 – S_R4** | Przyciski sterowania ręcznego | Przyciski bistabilne z podświetleniem (1NO) | Ręczne załączanie sekcji grzałek 1–4 poza PLC | Eaton `M22-DRL-W` | 4 |
| **H1 – H7** | Lampki sygnalizacyjne LED | $\varnothing 22\text{ mm}$, $24\text{ V DC}$ (zielone, żółte, czerwona) | Sygnalizacja optyczna pracy i stanów awaryjnych | Promet `D22MSZ` (ziel.) / `D22MSC` (czerw.) | 7 |
| **HA1** | Sygnalizator akustyczny | Buczek tablicowy $\varnothing 22\text{ mm}$, $24\text{ V DC}$, $85\text{ dB}$ | Sygnalizacja dźwiękowa awarii (odpadnięcie `AL`) | Eaton `M22-AMC` | 1 |
| **X1, X2** | Listwy zaciskowe | Złączki przelotowe śrubowe na szynę TS35 | Podłączenie przewodów obiektowych i zasilających | Phoenix Contact `3010123` (UK 5 N) | 35 |
| **SZ1** | Obudowa rozdzielnicy | $600 \times 800 \times 250\text{ mm}$, IP54, z płytą montażową | Szafa zasilająco-sterownicza układu bojlera | Apator `1362 129 014 T` / Rittal `AX` | 1 |

---

## 2. Lista Wejść i Wyjść Sterownika PLC (`A1`)

| Adres PLC | Typ | Oznaczenie karty w SEE | Podłączony element | Opis działania (Komentarz PLC) |
| :---: | :---: | :---: | :--- | :--- |
| **I1** (`I0.0`) | DI | `A1-I01` (Kanał 1) | Styk 1 komparatora `U_T1` | `PROG T1 (1 = TEMP < T1, LICZ W PRZOD)`|
| **I2** (`I0.1`) | DI | `A1-I01` (Kanał 2) | Styk 2 komparatora `U_T1` | `PROG T2 (1 = TEMP > T2, LICZ W TYL)`|
| **I3** (`I0.2`) | DI | `A1-I01` (Kanał 3) | Styk rozwierny (NC) `AL:31-32` | `PRZEKAZNIK AL (0 = PRACA OK, 1 = AWARIA/RESET)`|
| **I4** (`I0.3`) | DI | `A1-I01` (Kanał 4) | Styk przełącznika `SA1` (Auto) | `POTWIERDZENIE TRYBU AUTOMATYCZNEGO` |
| **Q1** (`Q0.0`) | DO | `A1-Q01` (Kanał 1) | Cewka przekaźnika `K1` | `WYJSCIE WAGA 1 - SEKCJA 1 (1 GRZALKA = 2kW)`|
| **Q2** (`Q0.1`) | DO | `A1-Q01` (Kanał 2) | Cewka przekaźnika `K2` | `WYJSCIE WAGA 2 - SEKCJA 2 (2 GRZALKI = 4kW)`|
| **Q3** (`Q0.2`) | DO | `A1-Q01` (Kanał 3) | Cewka przekaźnika `K3` | `WYJSCIE WAGA 4 - SEKCJA 3 (4 GRZALKI = 8kW)`|
| **Q4** (`Q0.3`) | DO | `A1-Q01` (Kanał 4) | Cewka przekaźnika `K4` | `WYJSCIE WAGA 8 - SEKCJA 4 (8 GRZALEK = 16kW)`|
