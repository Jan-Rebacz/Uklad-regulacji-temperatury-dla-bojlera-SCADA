# Projekt 11: Układ regulacji temperatury dla bojlera (SEE Electrical + AVEVA InTouch)

Kompletne repozytorium projektowe z przedmiotu **Systemy SCADA**. Projekt obejmuje dobór aparatury elektroenergetycznej i pomiarowej, obliczenia inżynierskie, schematy elektryczne oraz zabudowę rozdzielnicy w środowisku **SEE Electrical**, a także system wizualizacji i symulacji **SCADA w programie Wonderware / AVEVA InTouch**.

---

## 1. Opis obiektu regulacji
Obiektem sterowania jest ciśnieniowy bojler wodny (podgrzewacz) o pojemności **3000 litrów**, który podgrzewa wodę do temperatury roboczej $T_{zad} = 130^\circ\text{C}$. Gorąca woda krąży w zamkniętym obiegu pod nadciśnieniem, wymuszanym przez pompę obiegową, stanowiąc układ grzewczy dla trzech przemysłowych suszarek.

* **Układ wykonawczy grzania:** 15 trójfazowych grzałek rezystancyjnych o mocy **2 kW** każda (łączna moc **30 kW**).
* **Sekcjonowanie binarne (K1–K4):** Grzałki połączono w 4 sekcje sterowane kaskadowo-binarnie przez wyjścia PLC (`Q1`–`Q4`), przekaźniki pośredniczące (`K1`–`K4`) oraz styczniki mocy (`KM1`–`KM4`):
  * **Sekcja 1 (`Q1` / `K1` / `KM1`):** 1 grzałka = **2 kW** (waga 1)
  * **Sekcja 2 (`Q2` / `K2` / `KM2`):** 2 grzałki = **4 kW** (waga 2)
  * **Sekcja 3 (`Q3` / `K3` / `KM3`):** 4 grzałki = **8 kW** (waga 4)
  * **Sekcja 4 (`Q4` / `K4` / `KM4`):** 8 grzałek = **16 kW** (waga 8)
* **Pomiar i regulacja:** Czujnik rezystancyjny **Pt100** w osłonie współpracuje z przetwornikiem-komparatorem posiadającym dwa progi: $T1 < T_{zad}$ oraz $T2 > T_{zad}$. W sterowniku PLC zaimplementowano rewersyjny licznik binarny (0–15) taktowany generatorem zegarowym o kroku **20 s** (nastawa generatora **10 s**).
* **Układ bezpieczeństwa (`AL`):** Nadzór nad minimalnym ciśnieniem ($p_{min}$), maksymalnym ciśnieniem ($p_{max}$), przegrzaniem ($T_{max}$) oraz pracą pompy obiegowej realizuje sprzętowy przekaźnik bezpieczeństwa `AL`.

---

## 2. Struktura repozytorium

* [`WORKFLOW.md`](/MD_Notes/WORKFLOW.md) – Mapa myśli, podział prac na 2 osoby oraz checklista odbiorowa projektu.
* [`OBLICZENIA_I_OPIS.md`](/MD_Notes/OBLICZENIA_I_OPIS.md) – Obliczenia prądów roboczych, dobór zabezpieczeń zwarciowych/przeciążeniowych, dobór przewodów, ochrona przeciwporażeniowa oraz kod programu PLC.
* [`BOM_ELEMENTY.md`](/MD_Notes/BOM_ELEMENTY.md) – Zestawienie wszystkich użytych elementów (aparatura, czujniki, szafa) z kodami katalogowymi oraz lista wejść/wyjść PLC.
* [`SEE_ELECTRICAL_GUIDE.md`](/MD_Notes/SEE_ELECTRICAL_GUIDE.md) – Instrukcja krok po kroku rysowania 4 arkuszy schematów zasadniczych, zabudowy rozdzielnicy 2D oraz generowania raportów w SEE Electrical.
* [`INTOUCH_GUIDE.md`](/MD_Notes/INTOUCH_GUIDE.md) – Instrukcja wykonania aplikacji SCADA w InTouch (baza 21 tagów, pełny kod skryptu symulacyjnego, logowanie 2 użytkowników, obsługa alarmów bieżących i historycznych).

---

## 3. Status realizacji wymagań projektowych

- [x] Krótki opis projektowanego stanowiska z obliczeniami
- [x] Dobór czujników ($Pt100$, $T_{max}$, $p_{min}$, $p_{max}$)
- [x] Dobór zabezpieczeń i przewodów zasilających/sterujących
- [x] Tryb automatyczny (PLC) oraz tryb ręczny (funkcjonujący poza sterownikiem PLC)
- [ ] Schematy zasilania, sterowania, zabudowy szafy i sygnalizacji w SEE Electrical
- [ ] Aplikacja SCADA w InTouch (symulacja Ręczny/Auto, 2 użytkowników, Alarmy bieżące i historyczne)
