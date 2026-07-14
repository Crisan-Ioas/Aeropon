# Aeropon
Sistem aeroponic autonom, alimentat solar, cu solar tracker și monitorizare Wi-Fi. Un ESP32-S3 controlează irigarea, ventilația și urmărirea soarelui, pe baza senzorilor de lumină și nivel de apă. Alimentare dublă: panouri solare pentru electronică, baterii 18650 pentru pompă și ventilator.

# Descriere
Sistem aeroponic autonom, alimentat solar, care cultivă plante fără substrat de sol — rădăcinile sunt pulverizate periodic cu apă și nutrienți direct în aer. Proiectul integrează irigare automată, monitorizare a nivelului de apă, ventilație și un solar tracker care rotește panourile spre sursa de lumină pentru randament maxim de încărcare. Întregul sistem este controlat de o placă ESP32-S3, care expune și o aplicație web pentru monitorizare și control de la distanță, prin Wi-Fi, direct din rețeaua locală.

# Descriere Tehnică

Alimentarea electronicii de control este solară: trei panouri fotovoltaice de 5V/1W, conectate în paralel, încarcă o baterie 18650 Li-Ion printr-o diodă de protecție Schottky (1N5819) și un modul de încărcare TP4056 cu protecție integrată DW01. Tensiunea variabilă a bateriei (3.7–4.2V) este ridicată la 5V stabil printr-un convertor boost MT3608, care alimentează placa ESP32-S3 și restul electronicii de control.

Un pachet separat de patru baterii 18650 conectate în serie (14.8–16.8V) alimentează un circuit de putere complet izolat, dedicat pompei de apă și ventilatorului, ambele de 12V DC, comutate printr-un modul releu dublu comandat digital de ESP32. Cele două circuite (control 5V și putere 12V) sunt separate electric, cu excepția masei comune (GND).

Solar tracker-ul funcționează pe baza a doi fotorezistori GL5537 (montați stânga-dreapta), citiți analogic de ESP32 prin divizoare de tensiune cu rezistoare de 10kΩ. Diferența de luminozitate dintre cei doi senzori determină rotația unui motor pas cu pas 28BYJ-48, acționat prin driverul ULN2003. Nivelul apei este monitorizat printr-un senzor analogic dedicat, alimentat controlat pentru economie de energie. Un ecran OLED SSD1306 (comunicare I²C) afișează local starea sistemului.

Toți senzorii analogici sunt alocați exclusiv pe pinii ADC1 ai ESP32-S3, singurii care oferă citiri stabile în timp ce modulul Wi-Fi este activ.

# Cerințe de sistem

**Hardware:**
- Placă ESP32-S3-DevKitC-1
- 3× panouri solare 5V/1W + diodă Schottky 1N5819
- Modul TP4056 (cu protecție DW01) + baterie 18650 Li-Ion
- Modul boost MT3608
- 4× baterii 18650 (pachet serie, circuit 12V)
- Modul releu dublu SRD-05VDC-SL-C
- Motor pas cu pas 28BYJ-48 + driver ULN2003
- 2× fotorezistor GL5537 + 2× rezistor 10kΩ
- Senzor analogic nivel apă
- Ecran OLED SSD1306 (I²C, 0.96")
- Pompă de apă 12V DC și ventilator 12V DC

**Software:**
- Arduino IDE sau PlatformIO, cu suport pentru plăci ESP32-S3
- Biblioteci: Adafruit_SSD1306, Adafruit_GFX (OLED), Stepper sau AccelStepper (motor pas cu pas), WebServer sau ESPAsyncWebServer (aplicație web)

**Rețea:**
- Acces la o rețea Wi-Fi locală (2.4GHz), pentru funcționarea aplicației web de monitorizare și control

**Instrumente pentru montaj:**
- Multimetru (reglarea MT3608 la exact 5.0V)
- Fier de lipit / fire jumper / breadboard sau placă perforată
