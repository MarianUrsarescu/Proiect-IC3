# Data Card – Proiect IC3 (H3)

## 1. Descriere
Proiectul urmărește estimarea unui indice de secetă agricolă cu aproximativ 4 săptămâni înainte, folosind date meteorologice și umiditatea solului dintr-o regiune agricolă din sudul Olteniei, în apropierea județului Vâlcea.

## 2. Sursa și perioada
- **Sursă:** Copernicus Climate Data Store (CDS), ERA5-Land Monthly Averaged Data.
- **Perioadă:** ianuarie 2015 – decembrie 2025 (132 luni).
- **Rezoluție temporală:** lunară.
- **Rezoluție spațială:** 0,1° × 0,1°.
- **Acoperire:** 44,5°–44,8° N și 23,8°–24,4° E; grilă de 4 × 7 = 28 puncte. Regiunea nu coincide exact cu limitele administrative ale județului Vâlcea.

## 3. Variabile și unități
| Variabilă | Descriere | Unitate originală |
|---|---|---|
| `t2m` | Temperatura aerului la 2 m | K |
| `tp` | Precipitații totale (medie zilnică în produsul lunar utilizat) | m |
| `swvl1` | Umiditatea volumetrică a solului, 0–7 cm | m³/m³ |
| `swvl2` | Umiditatea volumetrică a solului, 7–28 cm | m³/m³ |
| `pev` | Evaporare potențială | m |

Temperatura a fost convertită în °C. Precipitațiile lunare au fost calculate din media zilnică înmulțită cu numărul de zile din lună și cu 1000. Umiditatea solului pentru 0–28 cm a fost calculată ca medie ponderată a celor două straturi: `(7*swvl1 + 21*swvl2)/28`.

## 4. Calitatea datelor
- **Observații pe variabilă:** 132 luni × 28 puncte = 3.696.
- **Valori lipsă:** 0 din 3.696 (0%) pentru fiecare dintre cele cinci variabile.
- După media spațială, seria regională conține 132 observații lunare.

## 5. Împărțirea datelor
Împărțire cronologică, fără amestecarea lunilor:

| Set | Perioadă | Luni |
|---|---|---:|
| Train | 2015–2021 | 84 |
| Validation | 2022–2023 | 24 |
| Test | 2024–2025 | 24 |

## 6. Indicele de secetă
A fost calculat un indice exploratoriu de anomalie standardizată a umidității solului (SSMI): `(SM - media_lunii) / abaterea_standard_a_lunii`. Media și abaterea standard pentru fiecare lună calendaristică sunt calculate **numai din train (2015–2021)**, pentru a evita folosirea datelor de validare/test la stabilirea referinței. Valorile negative indică umiditate sub referința sezonieră.

## 7. Limitări
- Datele sunt lunare; predicția pentru luna următoare este doar o aproximare a unui orizont fix de 4 săptămâni (28 de zile).
- Referința SSMI cuprinde doar 7 ani, deci indicele și pragurile de severitate trebuie interpretate cu precauție.
- Datele ERA5-Land sunt reanalize, nu măsurători directe în fiecare punct.
- Media pe grilă include puncte dintr-o regiune mai largă decât sudul județului Vâlcea.
- Analiza SSMI este retrospectivă; modelul de prognoză urmează să fie implementat.

## 8. Licență și reproducere
- **Licență date:** Copernicus Data Licence; verificați condițiile aplicabile produsului descărcat din CDS.
- **Repository:** https://github.com/MarianUrsarescu/Proiect-IC3
- **Mediu de lucru:** Python, Kaggle Notebook (`xarray`, `pandas`, `numpy`, `matplotlib`).
- Pipeline-ul realizat: încărcare NetCDF, verificarea dimensiunilor și valorilor lipsă, conversii de unități, medie spațială, calcul exploratoriu SSMI și split temporal.
