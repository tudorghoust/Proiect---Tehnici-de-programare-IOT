# ⚡ Smart Campus Energy Monitoring

### Mini-aplicație IoT – Simularea monitorizării inteligente a consumului de energie într-o clădire publică

**Membri echipă:** Codreanu Tudor, Vinț Alexandru  
**Repository GitHub:** `https://github.com/tudorghoust/Proiect---Tehnici-de-programare-IOT`  
**Scenariu:** Smart Building / Smart Energy  
**Status:** Propunere / Studiu preliminar  

---

## 1. Descrierea scenariului

Proiectul urmărește simularea unui sistem IoT pentru monitorizarea consumului de energie electrică și termică dintr-o clădire publică (campus universitar).

Sistemul colectează date simulate de la senzori (consum electric, prezență, stare iluminat, temperatură interioară, fereastră deschisă/închisă), le transmite către un **gateway/edge la nivel de etaj**, apoi către un serviciu cloud, unde sunt stocate, analizate și prezentate administratorului clădirii printr-o interfață web.

Aplicația trebuie să permită **identificarea și semnalarea anomaliilor care indică risipă de energie**:

* iluminat sau încălzire pornite în săli fără prezență;
* consum ridicat în afara programului (noaptea, în weekend);
* fereastră deschisă cu încălzirea pornită;
* creștere bruscă a consumului pe un etaj față de media istorică.

Pe baza datelor, sistemul estimează **energia risipită și costul aferent** (lei) și generează un raport periodic cu economiile posibile.

### Contextul problemei

* La nivelul UE, clădirile sunt responsabile pentru aproximativ **40% din consumul de energie** și **36% din emisiile de gaze cu efect de seră**, iar circa **75% din fondul construit este ineficient energetic** (Comisia Europeană).
* În România, după liberalizarea completă a pieței (1 iulie 2025), prețul energiei pentru consumatorii **non-casnici** (firme, instituții publice) se situează între aproximativ **1,26 și 2 lei/kWh**, în funcție de furnizor. Pentru o instituție publică, orice kWh risipit are deci un cost direct și semnificativ.
* Renovarea clădirilor este scumpă și lentă. O parte importantă a risipei vine însă din **comportament** (lumini, calculatoare, aer condiționat lăsate pornite) și poate fi redusă fără investiții mari, doar prin vizibilitate și alertare.

---

**Întrebarea principală (tehnică):** *Cum poate un sistem IoT să identifice automat consumul inutil de energie într-o clădire publică, utilizând date provenite de la senzori de consum și prezență?*

**Întrebarea de cercetare (non-tehnică):** *Ce pondere din consumul de energie al unei clădiri universitare este cauzată de comportamentul utilizatorilor (echipamente și iluminat lăsate pornite, ferestre deschise cu încălzirea pornită) și cât din acest consum poate fi eliminat doar prin monitorizare și alertare, fără investiții în renovare?*

**Ipoteză de lucru:** în spațiile cu utilizare intermitentă (săli de curs, laboratoare), consumul în afara orelor de ocupare reprezintă o parte semnificativă din total, iar alertarea în timp real poate reduce acest consum cu cel puțin jumătate.

---

## 2. Scenariul concret – Etajul 1, Corpul B

Pentru MVP se simulează **un singur etaj cu 3 încăperi**, cu profiluri de utilizare diferite:

| Încăpere | Tip | Iluminat | Echipamente | Încălzire / AC | Putere totală |
| --- | --- | --- | --- | --- | --- |
| **B101** | Sală de curs | 12 × 36 W | Videoproiector 300 W | AC 2,5 kW | ~3,2 kW |
| **B102** | Laborator | 8 × 36 W | 20 PC × 150 W | AC 3,5 kW | ~6,8 kW |
| **B103** | Secretariat | 4 × 36 W | 2 PC + imprimantă | Convector 2 kW | ~2,8 kW |

**Program normal:** luni–vineri, 08:00–20:00. Weekend închis.

### Exemplu de risipă simulată

| Situație | Calcul | Energie | Cost (1,30 lei/kWh) |
| --- | --- | --- | --- |
| PC-uri pornite peste noapte (B102) | 20 × 60 W × 12 h | 14,4 kWh | ~19 lei/noapte |
| Același lucru, o lună | 14,4 kWh × 22 nopți | ~317 kWh | **~412 lei/lună** |
| Lumini aprinse peste noapte (B101) | 12 × 36 W × 12 h | 5,2 kWh | ~7 lei/noapte |
| Convector pornit în weekend (B103) | 2 kW × 48 h × 50% | 48 kWh | **~62 lei/weekend** |

*Valorile sunt estimative, folosite pentru calibrarea simulatorului. Prețul/kWh este configurabil în aplicație.*

### Povestea demonstrativă (o zi simulată)

1. **08:00–14:00** – curs în B101 (prezență, lumini, proiector), laborator în B102 → consum normal.
2. **14:30** – cursul se termină, sala B101 se golește, dar luminile și proiectorul rămân pornite → **alertă `LIGHTS_ON_EMPTY_ROOM`** după 15 minute.
3. **16:00** – în B103 se deschide fereastra, iar convectorul continuă să funcționeze → **alertă `WINDOW_OPEN_HEATING_ON`**.
4. **20:00–08:00** – 15 din 20 de PC-uri rămân pornite în B102 → **alertă `OUT_OF_HOURS_CONSUMPTION`**.
5. **Sâmbătă** – consumul etajului este de 3× media pentru acea oră → **alertă `FLOOR_CONSUMPTION_SPIKE`**.
6. **Luni dimineață** – raportul săptămânal arată: *„Energie risipită: 112 kWh ≈ 146 lei. Sursa principală: B102, PC-uri lăsate pornite.”*

---

## 3. Obiectivele proiectului

### Obiective obligatorii (MVP)

- [ ] Simularea senzorilor pentru 3 încăperi (consum, prezență, iluminat, temperatură, fereastră)
- [ ] Transmiterea periodică a datelor către un gateway de etaj
- [ ] Prelucrare simplă la nivel edge (validare, agregare pe etaj)
- [ ] Transmiterea și stocarea datelor în backend
- [ ] Interfață web pentru vizualizarea consumului pe încăpere și pe etaj
- [ ] Detecția a minimum 2 tipuri de anomalii de risipă
- [ ] Afișarea alertelor și a istoricului acestora
- [ ] Calculul energiei risipite și al costului în lei

### Obiective opționale

- [ ] Utilizarea unui dataset public cu date reale
- [ ] Compararea a două metode de detecție (reguli vs. statistic)
- [ ] Raport săptămânal automat (energie risipită, cost, încăperea cu cele mai multe probleme)
- [ ] Simularea unei acțiuni automate (stingerea luminilor după 15 minute fără prezență)
- [ ] Simularea comparativă „fără alerte” vs. „cu alerte” pentru răspunsul la întrebarea de cercetare

### Obiectiv AI / LLM

Asistent conversațional pentru administratorul clădirii. Exemple:

- „Cât a consumat etajul 1 săptămâna trecută și cât a costat?”
- „Care încăpere a risipit cea mai multă energie luna aceasta?”
- „Au existat alerte în weekend?”
- „Cât am economisi lunar dacă PC-urile din laborator ar fi oprite peste noapte?”

*Răspunsurile trebuie să fie fundamentate pe datele aplicației, nu pe cunoștințele generale ale modelului.*

---

## 4. Arhitectura propusă

**Simulator senzori → Edge/Gateway (etaj) → Cloud/Backend → Frontend**

| Componentă | Responsabilitate |
| --- | --- |
| Sensor Simulator | Generează date pentru fiecare încăpere după un program de ocupare, inclusiv scenarii de risipă |
| Edge/Gateway (etaj) | Primește datele, le validează, agregă consumul pe etaj și aplică regulile simple (prezență vs. iluminat) |
| Cloud/Backend | Expune API-uri, stochează măsurătorile, calculează costurile, gestionează alertele |
| Frontend | Dashboard cu consum curent, grafice, alerte și raport de economii |
| LLM (opțional) | Răspunde la întrebări în limbaj natural pe baza datelor stocate |

```text
[B101: meter, PIR, light, temp, window] ─┐
[B102: meter, PIR, light, temp, window] ─┼──► [Edge Gateway – Etaj 1]
[B103: meter, PIR, light, temp, window] ─┘          │
                                                    │ MQTT
                                                    ▼
                                             [Cloud Backend]
                                             ├── REST API
                                             ├── Database
                                             ├── Cost Calculator
                                             └── Anomaly Events
                                                    │
                                                    ▼
                                             [Web Dashboard]
                                             ├── Consum pe încăpere/etaj
                                             ├── Alerte
                                             └── Raport economii

                       [LLM Assistant – opțional] ── Backend API / DB
```

**Topicuri MQTT propuse:**

```text
campus/corpB/etaj1/B101/telemetry
campus/corpB/etaj1/B102/telemetry
campus/corpB/etaj1/B103/telemetry
campus/corpB/etaj1/aggregate
campus/corpB/etaj1/alerts
```

*Nu sunt necesare dispozitive hardware reale. Toate componentele rulează local sau în containere Docker.*

---

## 5. Datele și simularea senzorilor

### Senzori propuși (în fiecare încăpere)

| Senzor | Mărime măsurată | Unitate | Interval normal | Frecvență |
| --- | --- | --- | --- | --- |
| **Contor electric** | Tensiune (U) | V | 207–253 (230 ±10%) | 10 s |
| **Contor electric** | Intensitate curent (I) | A | 0–30 | 10 s |
| **Contor electric** | Putere activă (P) | W | 0–7000 | 10 s |
| **Contor electric** | Factor de putere (cos φ) | – | 0,85–1 | 10 s |
| **Contor electric** | Energie consumată (E) | kWh | crescătoare | 10 s |
| **Senzor prezență (PIR)** | Prezență | 0/1 | 0 = gol, 1 = ocupat | 10 s |
| **Senzor lumină** | Iluminare | lx | < 50 stins, 300–500 aprins | 30 s |
| **Senzor temperatură** | Temperatură | °C | 20–24 | 30 s |
| **Contact fereastră** | Stare fereastră | 0/1 | 0 = închisă, 1 = deschisă | la schimbare |

Starea încălzirii/AC (ON/OFF) este citită de la echipament.

**Formule folosite:** `P = U × I × cos φ` · `E = P × t` · `Cost = E × preț/kWh`

*Exemplu: 230 V × 4,2 A × 0,95 ≈ 918 W.*

### Exemplu de mesaj transmis

```json
{
  "device_id": "corpB-etaj1-B102",
  "room_type": "laborator",
  "timestamp": "2026-11-12T22:15:00Z",
  "voltage_v": 230.4,
  "current_a": 4.18,
  "active_power_w": 914.9,
  "power_factor": 0.95,
  "energy_kwh": 1532.87,
  "occupancy": 0,
  "illuminance_lx": 12,
  "temperature_c": 21.6,
  "window_open": 0,
  "hvac_on": false
}
```

*Aici: ora 22:15, 0 persoane, luminile stinse (12 lx), dar contorul arată 4,18 A / ~915 W → PC-uri lăsate pornite.*

### Generarea datelor

Simulator Python care:

1. Folosește un **orar de ocupare** pe încăpere (ex. curs B101 luni 08–14, laborator B102 zilnic 10–18).
2. Calculează consumul din echipamentele pornite + zgomot aleator.
3. Simulează temperatura în funcție de încălzire, fereastră și temperatura exterioară.
4. Injectează controlat **scenarii de risipă** (lumini uitate, PC-uri pornite, fereastră deschisă) și le **etichetează** pentru evaluare.
5. Publică datele către gateway prin MQTT.

### Dataset-uri externe (opțional)

| Dataset | Ce conține | Utilizare în proiect | Licență |
| --- | --- | --- | --- |
| **UCI Occupancy Detection** | ~20.560 înregistrări: temperatură, umiditate, lumină | Profiluri de prezență | CC BY 4.0 |
| **UCI Room Occupancy** | Cameră, 4 zile, temperatură, lumină, sunet, PIR | Număr de persoane | CC BY 4.0 |
| **Building Data Genome 2** | 3.053 contoare, date orare din clădiri non-rezidențiale | Curbe reale de consum | Open data |

---

## 6. Identificarea anomaliilor

### Regulile inițiale

```text
R1 – LIGHTS_ON_EMPTY_ROOM
IF illuminance_lx > 150 AND occupancy = 0
FOR 15 minutes
THEN alert (WARNING)

R2 – WINDOW_OPEN_HEATING_ON
IF window_open = 1 AND hvac_on = true
FOR 10 minutes
THEN alert (WARNING)

R3 – OUT_OF_HOURS_CONSUMPTION
IF time NOT IN program (L–V 08:00–20:00)
AND active_power_w > baseline_standby(room) × 1.5
FOR 30 minutes
THEN alert (CRITICAL)

R4 – FLOOR_CONSUMPTION_SPIKE
IF floor_active_power_w > mean(same hour-of-week, last 4 weeks) + 3σ
THEN alert (WARNING)

R5 – VOLTAGE_OUT_OF_RANGE
IF voltage_v < 207 OR voltage_v > 253      (230 V ± 10%)
FOR 3 consecutive readings
THEN alert (CRITICAL)

R6 – OVERCURRENT
IF current_a > 0.9 × rated_current(room circuit)   (ex. 0.9 × 16 A)
FOR 3 consecutive readings
THEN alert (CRITICAL)

R7 – SENSOR_OFFLINE / IMPOSSIBLE_VALUE
IF no message FOR 2 minutes
OR power_factor > 1 OR current_a < 0 OR active_power_w > voltage_v × current_a
THEN alert (INFO)
```

R1, R2, R5, R6 și R7 rulează pe **edge** (decizie locală, rapidă). R3 și R4 rulează în **cloud** (au nevoie de istoric).

R1–R4 detectează **risipa** (obiectivul principal). R5–R7 detectează **probleme electrice și de senzori** (siguranță și calitatea datelor).

### Calculul risipei

Pentru fiecare alertă se calculează:

```text
energie_risipită (kWh) = Σ (active_power_w − baseline_standby_w) × Δt_ore / 1000
cost (lei) = energie_risipită × preț_kWh
```

Prețul implicit: **1,30 lei/kWh** (configurabil, în intervalul ofertelor pentru non-casnici).

### Evaluarea detecției

- numărul de anomalii detectate față de cele injectate;
- alarme false (ex. sală goală, dar lumina aprinsă pentru curățenie);
- Precision / Recall pe baza etichetelor din simulator;
- latența dintre producerea anomaliei și afișarea alertei.

### Răspunsul la întrebarea de cercetare

Se rulează simulatorul pe **4 săptămâni** în două variante:

1. **Fără sistem** – risipa comportamentală apare cu frecvența din scenariu.
2. **Cu sistem** – la fiecare alertă, cu o probabilitate configurabilă (ex. 70%), utilizatorul/administratorul reacționează în X minute.

Se compară: consumul total, ponderea consumului în afara programului, energia și costul economisite.

---

## 7. Interfața frontend – schiță preliminară

```text
+---------------------------------------------------------------+
|          SMART CAMPUS ENERGY – Corp B, Etaj 1                 |
+---------------------------------------------------------------+
| Putere etaj: 4.82 kW | Azi: 38.4 kWh | Cost azi: 49.9 lei     |
+---------------------------------------------------------------+
|  B101 Sală curs    |  B102 Laborator   |  B103 Secretariat    |
|  229.8 V | 1.9 A   |  230.4 V | 4.18 A |  231.0 V | 9.1 A    |
|  0 pers. | 💡 ON   |  0 pers. | 915 W  |  1 pers. | 🪟 OPEN  |
|  ⚠ GOL + LUMINI    |  ⚠ ÎN AFARA ORAR. |  ⚠ GEAM + ÎNCĂLZIRE  |
+---------------------------------------------------------------+
|  Consum etaj – ultimele 24h       [ LINE CHART ]              |
|  (zonă gri = în afara programului)                            |
+---------------------------------------------------------------+
| Alerte recente                                                |
| 22:15  B102  OUT_OF_HOURS_CONSUMPTION   CRITICAL   ~1.2 lei/h |
| 16:10  B103  WINDOW_OPEN_HEATING_ON     WARNING               |
| 14:45  B101  LIGHTS_ON_EMPTY_ROOM       WARNING               |
+---------------------------------------------------------------+
| Raport săptămână: 112 kWh risipiți ≈ 146 lei | Top: B102      |
+---------------------------------------------------------------+
| Întreabă clădirea (opțional)                                  |
| [ Cât am risipit în weekend?                         ] [Send] |
+---------------------------------------------------------------+
```

**Implementare propusă:** React + Recharts.

---

## 8. Tehnologii și API-uri

| Componentă | Tehnologie propusă |
| --- | --- |
| Sensor Simulator | Python |
| Edge/Gateway | Python (paho-mqtt) |
| Comunicare IoT | MQTT |
| Broker MQTT | Mosquitto |
| Backend API | FastAPI |
| Database | SQLite (MVP) / PostgreSQL + TimescaleDB (opțional) |
| Frontend | React + Recharts |
| Deployment | Docker Compose |
| LLM (opțional) | API LLM sau model local |

### API-uri propuse

| Endpoint | Metodă | Descriere |
| --- | --- | --- |
| `/api/rooms` | GET | Lista încăperilor și starea curentă |
| `/api/rooms/{id}/measurements` | GET | Istoricul măsurătorilor pentru o încăpere |
| `/api/floor/consumption` | GET | Consumul agregat al etajului pe interval |
| `/api/alerts` | GET | Alertele detectate (filtrabile după încăpere/tip) |
| `/api/reports/waste` | GET | Energie risipită și cost pe perioadă |
| `/api/config/price` | POST | Setează prețul/kWh și programul clădirii |
| `/api/simulator/config` | POST | Configurează simulatorul și injectarea scenariilor de risipă |
| `/api/ask` | POST | Întrebări în limbaj natural (opțional) |

## 9. Integrare LLM – extensie de excelență

Flux:

1. Frontend-ul trimite întrebarea către backend.
2. Backend-ul identifică intervalul și încăperile relevante și interoghează baza de date.
3. Rezultatele (consum, alerte, costuri) sunt trimise LLM-ului ca context.
4. LLM-ul formulează răspunsul.
5. Frontend-ul afișează răspunsul împreună cu datele folosite.

Exemplu: *„În weekend au fost 2 alerte. Laboratorul B102 a consumat 31 kWh în afara programului, aproximativ 40 lei, din cauza PC-urilor rămase pornite.”* (valori ilustrative)

LLM-ul nu inventează măsurători, spune explicit când nu are date, iar cheile API rămân pe backend.

---

## 10. Etape de implementare (sprinturi)

| Sprint | Activități principale | Livrabil |
| --- | --- | --- |
| Sprint 0 | Scenariu, arhitectură, README, repository | Propunerea proiectului |
| Sprint 1 | Simulator cu orar de ocupare pentru 3 încăperi | Flux de date simulate |
| Sprint 2 | Gateway de etaj, MQTT, backend și stocare | Pipeline funcțional |
| Sprint 3 | Dashboard, grafice, calcul cost | Interfață demonstrabilă |
| Sprint 4 | Reguli de anomalii, alerte, raport risipă, testare | MVP complet |
| Sprint 5 (opțional) | Comparație fără/cu sistem, LLM | Răspuns la întrebarea de cercetare + funcții avansate |

---

## 11. Repartizarea responsabilităților

| Membru | Responsabilități principale |
| --- | --- |
| Codreanu Tudor | Simulator senzori, gateway edge, MQTT, reguli de anomalii pe edge, Docker |
| Vinț Alexandru | Backend FastAPI, baza de date, calcul cost/risipă, frontend și dashboard |

Ambii membri participă la arhitectură, integrare, testare, documentare și la analiza pentru întrebarea de cercetare. Contribuțiile sunt vizibile prin commit-uri și pull request-uri.

---
## 12. Structura preliminară a repository-ului

```
smart-campus-energy/
├── README.md
├── simulator/
│   ├── sensor_simulator.py
│   ├── schedules.yaml        # orar de ocupare pe încăpere
│   └── scenarios.yaml        # scenarii de risipă injectate
├── edge/
│   └── floor_gateway.py
├── backend/
│   ├── api/
│   ├── database/
│   └── services/             # cost, reguli, rapoarte
├── frontend/
│   └── src/
├── datasets/
├── docs/
│   ├── architecture.png
│   └── dashboard-sketch.png
├── tests/
└── docker-compose.yml
```

---

## 13. Testare și criterii de succes

Scenariul end-to-end demonstrat:

1. Simulatorul generează date normale pentru B101–B103.
2. Datele trec prin gateway-ul de etaj și ajung în backend.
3. Dashboard-ul afișează consumul pe încăpere și pe etaj.
4. Simulatorul injectează „sala goală cu luminile aprinse” în B101.
5. Sistemul generează alerta, cu ora, cauza și costul estimat.
6. Raportul arată energia risipită și costul total pe perioada simulată.

**Criteriu minim de succes:** flux demonstrabil senzor simulat → edge → backend → frontend, cu detectarea și afișarea a cel puțin 2 tipuri de risipă și calculul costului.

**Varianta de excelență:** comparația „fără/cu sistem” care răspunde la întrebarea de cercetare și răspunsuri LLM bazate pe date.

---

## 14. Riscuri și limitări

- Datele simulate nu reproduc exact comportamentul real al utilizatorilor.
- Pragurile fixe pot genera alarme false (ex. personal de curățenie seara).
- Senzorul PIR nu detectează persoanele nemișcate → risc de alertă falsă.
- Prețul energiei variază după furnizor și contract; costul calculat este o estimare.
- Rezultatul întrebării de cercetare depinde de parametrii scenariului și trebuie prezentat ca simulare, nu ca măsurătoare reală.

**Strategie:** prioritizarea MVP-ului; funcțiile opționale se adaugă doar după validarea fluxului end-to-end.

---


## 15. Referințe și resurse

- Comisia Europeană – *In focus: Energy efficiency in buildings*: https://commission.europa.eu/news/focus-energy-efficiency-buildings-2020-02-17_en
- Economedia – prețul energiei pentru consumatorii non-casnici după liberalizare: https://economedia.ro/?p=271300
- UCI Occupancy Detection Dataset: https://archive.ics.uci.edu/dataset/357/occupancy+detection
- UCI Room Occupancy Estimation Dataset: https://archive.ics.uci.edu/dataset/864/room+occupancy+estimation
- Building Data Genome Project 2: https://github.com/buds-lab/building-data-genome-project-2 (articol: Miller et al., *Scientific Data* 7, 368, 2020)
- Documentație: Mosquitto, paho-mqtt, FastAPI, React, Recharts

---

## 16. Întrebări deschise

- Ce reguli rulează pe edge și ce reguli în cloud?
- Cum stabilim consumul de bază (standby) pentru fiecare încăpere?
- Cât timp păstrăm datele brute față de cele agregate?
- Cum reducem alarmele false cauzate de PIR?
- Ce parametri de reacție a utilizatorilor folosim în simularea „cu sistem”?

---

**Notă:** Acest README constituie propunerea inițială a proiectului și va fi actualizat pe măsură ce arhitectura și funcționalitățile sunt validate.
