# Opis Techniczny Stanowiska, Obliczenia i Algorytm PLC

*Ten plik stanowi gotowy materiał tekstowy i obliczeniowy do wklejenia bezpośrednio do sprawozdania końcowego (Word / PDF).*

---

## 1. Opis technologiczny projektowanego układu

Projektowany układ służy do automatycznej oraz ręcznej regulacji temperatury wody w ciśnieniowym bojlerze przemysłowym o pojemności $V = 3000\text{ l}$. Woda podgrzewana jest do temperatury zadanej $T_{zad} = 130^\circ\text{C}$ i krąży w zamkniętym obiegu grzewczym zasilającym trzy suszarki przemysłowe. Z uwagi na temperaturę pracy przekraczającą $100^\circ\text{C}$, w celu uniknięcia wrzenia czynnika układ pracuje przy nadciśnieniu roboczym w zakresie **3,0 – 4,5 bar** (ciśnienie nasycenia pary wodnej w $130^\circ\text{C}$ wynosi ok. $2,7\text{ bar}$ abs., co odpowiada ok. $1,7\text{ bar}$ nadciśnienia). Obieg wody wymuszany jest przez pompę obiegową napędzaną silnikiem indukcyjnym trójfazowym $M1$ o mocy $P_P = 1,5\text{ kW}$.

### Sekcjonowanie binarne grzałek
Elementem wykonawczym w bojlerze jest 15 trójfazowych grzałek rezystancyjnych ($3 \times 400\text{ V AC}$), każda o mocy znamionowej $2\text{ kW}$ (łączna moc zainstalowana wynosi $P_G = 30\text{ kW}$). Sterowanie mocą grzewczą odbywa się krokowo (w 16 krokach od $0\text{ kW}$ do $30\text{ kW}$ co $2\text{ kW}$) za pomocą 4 wyjść binarnych sterownika PLC ($Q1, Q2, Q3, Q4$). W celu optymalizacji aparatury łączeniowej grzałki połączono równolegle w **4 sekcje wykonawcze** o wagach odpowiadających kolejnym potęgom liczby 2:
* **Sekcja 1 (sterowana przez przekaźnik $K1$ i stycznik $KM1$):** 1 grzałka $\times 2\text{ kW} = 2\text{ kW}$ (waga 1 – wyjście $Q1$),
* **Sekcja 2 (sterowana przez przekaźnik $K2$ i stycznik $KM2$):** 2 grzałki $\times 2\text{ kW} = 4\text{ kW}$ (waga 2 – wyjście $Q2$),
* **Sekcja 3 (sterowana przez przekaźnik $K3$ i stycznik $KM3$):** 4 grzałki $\times 2\text{ kW} = 8\text{ kW}$ (waga 4 – wyjście $Q3$),
* **Sekcja 4 (sterowana przez przekaźnik $K4$ i stycznik $KM4$):** 8 grzałek $\times 2\text{ kW} = 16\text{ kW}$ (waga 8 – wyjście $Q4$).

### Pomiar temperatury i zasada regulacji
Pomiar temperatury realizowany jest za pomocą czujnika rezystancyjnego **Pt100** zamontowanego w osłonie ciśnieniowej wewnątrz płaszcza bojlera. Sygnał z czujnika trafia do dwuprogowego przetwornika-komparatora temperatury z ustawionymi progami:
* Próg dolny: $T1 = 128^\circ\text{C}$ ($T1 < T_{zad}$) – wyjście komparatora podaje stan wysoki na wejście $I1$ sterownika PLC, gdy $T_{rzecz} < T1$.
* Próg górny: $T2 = 132^\circ\text{C}$ ($T2 > T_{zad}$) – wyjście komparatora podaje stan wysoki na wejście $I2$ sterownika PLC, gdy $T_{rzecz} > T2$.

Przedział pomiędzy $T1$ a $T2$ ($128^\circ\text{C} \dots 132^\circ\text{C}$) tworzy **strefę nieczułości** regulatora. Gdy temperatura jest niższa od $T1$, wewnętrzny licznik rewersyjny w sterowniku PLC jest inkrementowany co **20 sekund** (generator impulsów zegarowych o wypełnieniu $10\text{ s} / 10\text{ s}$), zwiększając moc grzałek o $2\text{ kW}$ w każdym kroku. Po wejściu temperatury w strefę nieczułości ($T1 \le T_{rzecz} \le T2$) licznik zatrzymuje się, utrzymując dotychczas załączoną moc grzania. Przekroczenie progu $T2$ uruchamia zliczanie wsteczne („w tył”) co 20 s, odłączając kolejne stopnie mocy.

### Tabela sekwencji sterowania (poprawiony kod binarny)
*(Uwaga projektowa: W treści zadania w wierszach dla kroków 6, 7, 8 oraz 11 występowały błędy typograficzne w zapisie bitowym. Poniżej zastosowano właściwy, naturalny 4-bitowy kod binarny gwarantujący liniowy przyrost mocy co $2\text{ kW}$)*:

| Nr kroku | Q4 (16 kW) | Q3 (8 kW) | Q2 (4 kW) | Q1 (2 kW) | Liczba aktywnych grzałek | Moc wypadkowa [kW] |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | 0 | 0 | 0 | 0 | 0 | **0** |
| **1** | 0 | 0 | 0 | 1 | 1 | **2** |
| **2** | 0 | 0 | 1 | 0 | 2 | **4** |
| **3** | 0 | 0 | 1 | 1 | 3 | **6** |
| **4** | 0 | 1 | 0 | 0 | 4 | **8** |
| **5** | 0 | 1 | 0 | 1 | 5 | **10** |
| **6** | 0 | 1 | 1 | 0 | 6 | **12** |
| **7** | 0 | 1 | 1 | 1 | 7 | **14** |
| **8** | 1 | 0 | 0 | 0 | 8 | **16** |
| **9** | 1 | 0 | 0 | 1 | 9 | **18** |
| **10** | 1 | 0 | 1 | 0 | 10 | **20** |
| **11** | 1 | 0 | 1 | 1 | 11 | **22** |
| **12** | 1 | 1 | 0 | 0 | 12 | **24** |
| **13** | 1 | 1 | 0 | 1 | 13 | **26** |
| **14** | 1 | 1 | 1 | 0 | 14 | **28** |
| **15** | 1 | 1 | 1 | 1 | 15 | **30** |

---

## 2. Obliczenia elektroenergetyczne (Dobór zabezpieczeń i przewodów)

### 2.1. Parametry zasilania i wzory podstawowe
* Napięcie zasilania: $3 \times 400\text{ V} / 230\text{ V AC}$, $50\text{ Hz}$, układ sieci **TN-S**.
* Sposób ułożenia przewodów zasilających: **B2** (przewody wielożyłowe w korytkach i rurach instalacyjnych na ścianie), temperatura otoczenia $30^\circ\text{C}$, żyły miedziane (Cu) w izolacji PVC.
* Warunki doboru zabezpieczeń przeciążeniowych wg normy PN-HD 60364-4-43:
  $$I_B \le I_n \le I_z$$
  $$I_2 \le 1,45 \cdot I_z$$
  gdzie:
  * $I_B$ – prąd obliczeniowy (roboczy) obwodu,
  * $I_n$ – prąd znamionowy urządzenia zabezpieczającego,
  * $I_z$ – obciążalność prądowa długotrwała przewodu,
  * $I_2$ – prąd zadziałania zabezpieczenia w określonym czasie (dla wyłączników nadprądowych o charakterystykach B, C, D przyjmuje się $I_2 = 1,45 \cdot I_n$, więc spełnienie warunku $I_n \le I_z$ automatycznie gwarantuje spełnienie drugiego warunku).

### 2.2. Obliczenia dla poszczególnych obwodów odbiorczych

1. **Pojedyncza grzałka trójfazowa ($P_g = 2\text{ kW}$, $\cos\varphi = 1,0$):**
   $$I_{B\_1g} = \frac{P_g}{\sqrt{3} \cdot U_n \cdot \cos\varphi} = \frac{2000}{\sqrt{3} \cdot 400 \cdot 1,0} = 2,89\text{ A}$$

2. **Sekcja 1 grzałek (1 grzałka, $P_1 = 2\text{ kW}$):**
   * Prąd obliczeniowy: $I_{B1} = 2,89\text{ A}$.
   * Dobór zabezpieczenia `F1`: Wyłącznik instalacyjny nadprądowy 3-biegunowy o charakterystyce **B** (odbiornik czysto rezystancyjny, brak przetężenia rozruchowego) i prądzie znamionowym $I_{n1} = 6\text{ A}$ (**B6A**).
   * Dobór przewodu: Przewód miedziany $5 \times 1,5\text{ mm}^2$ (obciążalność dla ułożenia B2 wynosi $I_z = 15,0\text{ A}$).
   * Sprawdzenie warunku: $2,89\text{ A} \le 6\text{ A} \le 15,0\text{ A}$ – **warunek spełniony**.

3. **Sekcja 2 grzałek (2 grzałki równolegle, $P_2 = 4\text{ kW}$):**
   * Prąd obliczeniowy: $I_{B2} = 2 \cdot 2,89\text{ A} = 5,77\text{ A}$.
   * Dobór zabezpieczenia `F2`: Wyłącznik nadprądowy 3P **B10A** ($I_{n2} = 10\text{ A}$).
   * Dobór przewodu: Przewód miedziany $5 \times 1,5\text{ mm}^2$ ($I_z = 15,0\text{ A}$).
   * Sprawdzenie warunku: $5,77\text{ A} \le 10\text{ A} \le 15,0\text{ A}$ – **warunek spełniony**.

4. **Sekcja 3 grzałek (4 grzałki równolegle, $P_3 = 8\text{ kW}$):**
   * Prąd obliczeniowy: $I_{B3} = 4 \cdot 2,89\text{ A} = 11,55\text{ A}$.
   * Dobór zabezpieczenia `F3`: Wyłącznik nadprądowy 3P **B16A** ($I_{n3} = 16\text{ A}$).
   * Dobór przewodu: Przewód miedziany $5 \times 2,5\text{ mm}^2$ ($I_z = 20,0\text{ A}$).
   * Sprawdzenie warunku: $11,55\text{ A} \le 16\text{ A} \le 20,0\text{ A}$ – **warunek spełniony**.

5. **Sekcja 4 grzałek (8 grzałek równolegle, $P_4 = 16\text{ kW}$):**
   * Prąd obliczeniowy: $I_{B4} = 8 \cdot 2,89\text{ A} = 23,09\text{ A}$.
   * Dobór zabezpieczenia `F4`: Wyłącznik nadprądowy 3P **B25A** ($I_{n4} = 25\text{ A}$).
   * Dobór przewodu magistralnego sekcji: Przewód miedziany $5 \times 4,0\text{ mm}^2$ ($I_z = 27,0\text{ A}$).
   * Sprawdzenie warunku: $23,09\text{ A} \le 25\text{ A} \le 27,0\text{ A}$ – **warunek spełniony**.

6. **Silnik pompy obiegowej $M1$ ($P_P = 1,5\text{ kW}$, $U_n = 400\text{ V}$, $\cos\varphi = 0,82$, sprawność $\eta = 0,84$):**
   $$I_{BP} = \frac{P_P}{\sqrt{3} \cdot U_n \cdot \cos\varphi \cdot \eta} = \frac{1500}{\sqrt{3} \cdot 400 \cdot 0,82 \cdot 0,84} = 3,14\text{ A}$$
   * Prąd rozruchowy silnika klatkowego ($k_r \approx 6,5$): $I_{rozr} = 6,5 \cdot 3,14\text{ A} \approx 20,4\text{ A}$.
   * Dobór zabezpieczenia `Q_P`: Zastosowano trójfazowy wyłącznik silnikowy termomagnetyczny **PKZM0-4** o zakresie nastawy wyzwalacza przeciążeniowego $2,5 \dots 4,0\text{ A}$ (nastawa ustawiona dokładnie na prąd znamionowy $I_r = 3,15\text{ A}$) oraz wyzwalaczu zwarciowym bezzwłocznym $I_{rm} = 56\text{ A}$ (nie zadziała błędnie podczas rozruchu silnika).
   * Dobór przewodu: Przewód miedziany $4 \times 1,5\text{ mm}^2$ ($I_z = 15,0\text{ A}$).

7. **Obwód sterowania i zasilacza $24\text{ V DC}$:**
   * Pobór mocy aparatury sterowniczej (PLC ok. $15\text{ W}$, 5 styczników po $4\text{ W}$, 5 przekaźników po $1\text{ W}$, przetworniki i sygnalizacja ok. $15\text{ W}$): łącznie $P_{ster} \approx 55\text{ W}$.
   * Dobrano zasilacz impulsowy na szynę DIN: $230\text{ V AC} / 24\text{ V DC}$, $I_{wyj} = 5\text{ A}$ ($120\text{ W}$).
   * Zabezpieczenie po stronie pierwotnej $230\text{ V AC}$ (`F5`): Wyłącznik nadprądowy 1P **B6A** (przewód $1,5\text{ mm}^2$).
   * Zabezpieczenie po stronie wtórnej $24\text{ V DC}$ (`F6`): Wyłącznik nadprądowy 1P **C2A** (prąd obliczeniowy po stronie $24\text{ V}$: $I_{24} = \frac{55\text{ W}}{24\text{ V}} \approx 2,29\text{ A}$ maks., podczas typowej pracy ok. $1,5\text{ A}$; przewody sterownicze w szafie **LgY $0,75\text{ mm}^2$** dla sygnałów PLC oraz **LgY $1,0\text{ mm}^2$** dla cewek styczników).

8. **Zasilanie główne rozdzielnicy:**
   * Maksymalny prąd jednoczesny całej rozdzielnicy:
     $$I_{B\_max} = (2,89 + 5,77 + 11,55 + 23,09) + 3,14 + 0,25 = 46,69\text{ A}$$
   * Dobrano rozłącznik główny `Q0` o prądzie znamionowym $I_n = 63\text{ A}$ (z wkładkami topikowymi gG 63A na odejściu zasilającym) oraz kabel zasilający **YKY $5 \times 16\text{ mm}^2$** ($I_z = 62\text{ A}$).

---

## 3. Ochrona przeciwporażeniowa i zwarciowa

1. **Ochrona podstawowa (przed dotykiem bezpośrednim):**
   * Izolacja podstawowa wszystkich przewodów i kabli dostosowana do napięcia znamionowego $450/750\text{ V}$,
   * Zamknięcie aparatury wewnątrz metalowej rozdzielnicy sterowniczej o stopniu ochrony **IP54**, otwieranej wyłącznie za pomocą klucza serwisowego,
   * Zastosowanie osłon izolacyjnych na zaciskach prądowych (IP2X).

2. **Ochrona przy uszkodzeniu (przed dotykiem pośrednim):**
   * **Samoczynne Wyłączenie Zasilania (SWZ)** w układzie sieci **TN-S** w czasie $t \le 0,4\text{ s}$ (zgodnie z normą PN-HD 60364-4-41). Wszystkie części przewodzące dostępne (korpus bojlera, obudowa silnika pompy, konstrukcja i drzwi rozdzielnicy) są połączone z główną szyną uziemiającą `PE`.
   * **Sprawdzenie warunku SWZ dla największego zabezpieczenia grzałek (B25A):**
     Dla wyłącznika o charakterystyce B krotność prądu wyzwalacza elektromagnetycznego wynosi $k = 5$. Prąd gwarantujący wyłączenie w czasie $< 0,1\text{ s}$ wynosi:
     $$I_a = 5 \cdot I_n = 5 \cdot 25\text{ A} = 125\text{ A}$$
     Dopuszczalna impedancja pętli zwarcia $Z_s$:
     $$Z_s \le \frac{U_0}{I_a} = \frac{230\text{ V}}{125\text{ A}} = 1,84\ \Omega$$
     (W typowej rozdzielnicy przemysłowej impedancja pętli zwarcia wynosi ok. $0,25\text{–}0,45\ \Omega$, zatem warunek SWZ jest spełniony z dużym zapasem).
   * **Ochrona uzupełniająca:** Zastosowano wyłącznik różnicowoprądowy `F0` czterobiegunowy $63\text{ A} / 30\text{ mA}$ (typ A), który wymusza natychmiastowe wyłączenie przy prądzie upływowym $I_{\Delta n} \ge 30\text{ mA}$.
   * **Obniżone napięcie bezpieczne (PELV $24\text{ V DC}$):** Obwody przycisków na drzwiach szafy, czujników obiektowych ($p_{min}, p_{max}, Pt100$) oraz wejść/wyjść PLC pracują przy bezpiecznym napięciu $24\text{ V DC}$ separowanym galwanicznie od sieci $230\text{ V AC}$.

3. **Zabezpieczenie zwarciowe:**
   Spodziewany prąd zwarciowy trójfazowy na szynach rozdzielnicy szacowany jest na poziomie $I_k'' \approx 3,5\text{ kA}$. Wszystkie dobrane wyłączniki nadprądowe i silnikowe posiadają zwarciową zdolność łączeniową $I_{cn} = 6\text{ kA}$ (lub $10\text{ kA}$), co zapewnia pełną odporność zwarciową aparatury.

---

## 4. Algorytm i Program Sterownika PLC (IEC 61131-3 ST)

```pascal
PROGRAM Regulacja_Bojlera_PLC
VAR_INPUT
    I1_Komparator_T1 : BOOL; (* 1 = Temp. rzeczywista < T1 (za niska) *)
    I2_Komparator_T2 : BOOL; (* 1 = Temp. rzeczywista > T2 (za wysoka) *)
    I3_Przekaznik_AL : BOOL; (* 0 = Praca prawidłowa (AL trzyma), 1 = Awaria/Stop (AL odpadł) *)
END_VAR
VAR_OUTPUT
    Q1_Waga1_2kW  : BOOL; (* Przekaźnik K1 -> Stycznik KM1 -> 1 grzałka (2 kW) *)
    Q2_Waga2_4kW  : BOOL; (* Przekaźnik K2 -> Stycznik KM2 -> 2 grzałki (4 kW) *)
    Q3_Waga4_8kW  : BOOL; (* Przekaźnik K3 -> Stycznik KM3 -> 4 grzałki (8 kW) *)
    Q4_Waga8_16kW : BOOL; (* Przekaźnik K4 -> Stycznik KM4 -> 8 grzałek (16 kW) *)
END_VAR
VAR
    T_Generator_Low  : TON;  (* Timer 10s stanu niskiego *)
    T_Generator_High : TON;  (* Timer 10s stanu wysokiego *)
    R_Trig_Krok      : R_TRIG; (* Detektor zbocza co 20 sekund *)
    Licznik_Krokow   : INT := 0; (* Rewersyjny licznik binarny 0..15 *)
    Slowo_Wyj        : WORD;
END_VAR

(* 1. GENERATOR IMPULSÓW ZEGAROWYCH (Nastawa 10s, Czas kroku = 20s) *)
T_Generator_Low(IN := NOT T_Generator_High.Q AND NOT I3_Przekaznik_AL, PT := T#10s);
T_Generator_High(IN := T_Generator_Low.Q, PT := T#10s);
R_Trig_Krok(CLK := T_Generator_Low.Q);

(* 2. REWERSYJNY LICZNIK BINARNY (0..15) Z RESETEM PRIORYTETOWYM OD I3 *)
IF I3_Przekaznik_AL THEN
    (* Gdy AL odpadnie (I3 = 1) -> natychmiastowe wyzerowanie licznika *)
    Licznik_Krokow := 0;
ELSIF R_Trig_Krok.Q THEN
    (* Zliczanie "do przodu" gdy T < T1 *)
    IF I1_Komparator_T1 AND NOT I2_Komparator_T2 AND (Licznik_Krokow < 15) THEN
        Licznik_Krokow := Licznik_Krokow + 1;
    (* Zliczanie "w tył" gdy T > T2 *)
    ELSIF I2_Komparator_T2 AND (Licznik_Krokow > 0) THEN
        Licznik_Krokow := Licznik_Krokow - 1;
    END_IF;
END_IF;

(* 3. WYSTEROWANIE WYJŚĆ BINARNYCH Q1..Q4 *)
Slowo_Wyj := INT_TO_WORD(Licznik_Krokow);

Q1_Waga1_2kW  := Slowo_Wyj.0 AND NOT I3_Przekaznik_AL;
Q2_Waga2_4kW  := Slowo_Wyj.1 AND NOT I3_Przekaznik_AL;
Q3_Waga4_8kW  := Slowo_Wyj.2 AND NOT I3_Przekaznik_AL;
Q4_Waga8_16kW : = Slowo_Wyj.3 AND NOT I3_Przekaznik_AL;

END_PROGRAM
```
