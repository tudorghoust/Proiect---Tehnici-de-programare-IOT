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
