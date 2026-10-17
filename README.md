# Smart Campus Energy Monitoring
Mini-aplicație IoT – Simularea monitorizării inteligente a consumului de energie într-o clădire publică
Membri echipă: Codreanu Tudor, Vinț Alexandru

Repository GitHub: https://github.com/tudorghoust/Proiect---Tehnici-de-programare-IOT/tree/main

Scenariu: Smart Building / Smart Energy
Status: Propunere / Studiu preliminar
1. Descrierea scenariului
   
Proiectul urmărește simularea unui sistem IoT pentru monitorizarea consumului de energie electrică și termică dintr-o clădire publică (campus universitar). Clădirea are mai multe etaje și săli de curs.
Sistemul colectează date simulate de la senzori: consum electric pe etaj, prezență în săli, stare iluminat, temperatură interioară și senzori de fereastră deschisă/închisă. Datele sunt transmise către un gateway/edge la nivel de etaj, apoi către un serviciu cloud, unde sunt stocate, analizate și prezentate utilizatorului (administratorul clădirii) printr-o interfață web.
Aplicația trebuie să permită identificarea și semnalarea unor anomalii care indică risipă de energie, de exemplu:
- iluminat sau încălzire pornite în săli fără prezență;
- consum ridicat în afara programului (noaptea, în weekend);
- fereastră deschisă cu încălzirea pornită;
- creștere bruscă a consumului pe un etaj față de media istorică.
Pe baza datelor, sistemul estimează energia risipită și costul aferent, oferind un raport periodic cu economiile posibile.

Întrebarea principală: Cum poate un sistem IoT să identifice automat consumul inutil de energie într-o clădire publică, utilizând date provenite de la senzori de consum și prezență?

Întrebarea de cercetare (non-tehnică): Ce pondere din consumul de energie al unei clădiri universitare este cauzată de comportamentul utilizatorilor (echipamente și iluminat lăsate pornite, ferestre deschise cu încălzirea pornită) și cât din acest consum poate fi eliminat doar prin monitorizare și alertare, fără investiții în renovare?

Ipoteză de lucru: în spațiile cu utilizare intermitentă (săli de curs, laboratoare), consumul în afara orelor de ocupare reprezintă o parte semnificativă din total, iar alertarea în timp real poate reduce acest consum cu cel puțin jumătate.

2. Scenariul concret – Etajul 1, Corpul B

Pentru MVP se simulează un singur etaj cu 3 încăperi, cu profiluri de utilizare diferite:

Încăpere	Tip	Echipamente monitorizate	Putere instalată (estimată)
B101	Sală de curs (60 locuri)	12 corpuri LED × 36 W, videoproiector 300 W, aer condiționat (încălzire/răcire) 2,5 kW	~3,2 kW
B102	Laborator informatică	20 PC × ~150 W (în lucru) / ~60 W (inactiv), 8 corpuri LED × 36 W, AC 3,5 kW	~6,8 kW
B103	Birou secretariat	2 PC, imprimantă multifuncțională, 4 corpuri LED × 36 W, convector electric 2 kW	~2,8 kW
Program normal al clădirii: luni–vineri, 08:00–20:00. Weekend închis.

Exemplu de risipă simulată
Situație	Calcul	Energie	Cost (la 1,30 lei/kWh)
20 PC lăsate pornite peste noapte în B102	20 × 60 W × 12 h	14,4 kWh/noapte	~18,7 lei/noapte
Același lucru, o lună (22 nopți)	14,4 × 22	~317 kWh	~412 lei/lună
Lumini lăsate aprinse în B101 peste noapte	12 × 36 W × 12 h	5,2 kWh	~6,7 lei/noapte
Convector pornit în B103 tot weekendul	2 kW × 48 h × 50% ciclu	48 kWh	~62 lei/weekend

Povestea demonstrativă
08:00–14:00 – curs în B101 (prezență, lumini, proiector), laborator în B102 → consum normal.
14:30 – cursul se termină, sala B101 se golește, dar luminile și proiectorul rămân pornite → alertă LIGHTS_ON_EMPTY_ROOM după 15 minute.
16:00 – în B103 se deschide fereastra, iar convectorul continuă să funcționeze → alertă WINDOW_OPEN_HEATING_ON.
20:00–08:00 – 15 din 20 de PC-uri rămân pornite în B102 → alertă OUT_OF_HOURS_CONSUMPTION.
Sâmbătă – consumul etajului este de 3× media pentru acea oră → alertă FLOOR_CONSUMPTION_SPIKE.
Luni dimineață – raportul săptămânal arată: „Energie risipită: 112 kWh ≈ 146 lei. Sursa principală: B102, PC-uri lăsate pornite.”
