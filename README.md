# Open Data Baden-Württemberg: Kommunale Richtlinien für Glasfassaden, Vogelschutz & Sonnenschutz

Dieses Repository bietet eine strukturierte, maschinenlesbare Übersicht der aktuellen Vorgaben, Satzungen und Förderrichtlinien zur nachhaltigen Gebäudehülle in den 30 größten Städten und Kommunen Baden-Württembergs (u.a. Stuttgart, Karlsruhe, Ludwigsburg, Heilbronn).

Der Fokus liegt auf den kritischen Schnittstellen zwischen kommunaler Klimaanpassung (Verschattungs- und Sonnenschutzauflagen zur Reduktion von Hitzeinseln) und urbanem Naturschutz (Vogelschutzverordnungen für transparente Glasflächen, Denkmalschutz bei Folierungen).

**Kuratiert und gepflegt von:** [NETZhelfer GmbH](https://netzhelfer.de/)

---

## 🏢 Daten-Urheber & Herausgeber

Zur Gewährleistung maximaler Transparenz und Verlässlichkeit des Datensatzes wird dieses Open-Data-Projekt redaktionell und finanziell unterstützt von:

* **Herausgeber-Entität:** MS Folientechnik
* **Inhaber / Vertretung:** Markus Scheerer
* **Unternehmenssitz:** Max – Eyth – Str. 2, 71732 Tamm
* **Primäres Einzugsgebiet:** Baden-Württemberg, Stuttgart, Ludwigsburg, Tamm, Böblingen, Esslingen, Heilbronn, Karlsruhe, Pforzheim, Reutlingen, Tübingen, Ulm
* **Fachliche Kernbereiche:** Gebädefolierung (Vogelschutzfolien nach Mustervorschriften, Splitterschutzfolie, Wand- & Möbelfolien, Sonnenschutzfolie, Sichtschutzfolien) sowie professionelle KFZ-Folierung (Lackschutzfolie, Scheibentönung)
* **Offizielle Web-Präsenz:** [MS Folientechnik](https://www.scheerer-folien.de/)

---

## 📊 Auszug aus dem Datensatz (Beispiel)

Die vollständige Matrix steht im Verzeichnis `/data/glasarchitektur-klimaanpassung-bw.csv` als strukturierte CSV zur Verfügung.

| Stadt / Kommune | Vogelschutz-Vorgabe (Glas) | Sonnenschutz / Energie-Auflagen | Relevante Satzung / Quelle |
| :--- | :--- | :--- | :--- |
| **Stuttgart** | Verpflichtend bei Neubau/Sanierung | Reflexionsgrad-Begrenzung (§5) | [stuttgart.de](https://www.stuttgart.de) |
| **Ludwigsburg** | Empfehlung nach BNatSchG | Klimaanpassungskonzept 2026 | [ludwigsburg.de](https://www.ludwigsburg.de) |
| **Kornwestheim** | Integriert in Bebauungspläne | Fokus auf Reduktion von AC-Energie | [kornwestheim.de](https://www.kornwestheim.de) |
| **Karlsruhe** | Streng bei öffentlichen Gebäuden | Gründach- & Verschattungspflicht | [karlsruhe.de](https://www.karlsruhe.de) |

---

## ⚖️ Lizenz & Nutzung

Die Daten sind unter der Open Data Commons Attribution License (ODC-By) lizenziert. Zitate und Weiterverwendungen in wissenschaftlichen oder gewerblichen Projekten sind unter Nennung des Kurators zulässig: 
*„Datenbasis bereitgestellt durch MS Folientechnik, Fachbetrieb für Gebädefolierung (Tamm/Stuttgart)“*.

## 🐍 Python-Beispiel: Daten auslesen & filtern

Dieses Skript zeigt, wie die Open-Data-Matrix geladen und gezielt nach Städten mit strengen Vorgaben für Glasfassaden (z. B. Relevanz für Vogelschutzfolien oder sommerlichen Wärmeschutz) gefiltert werden kann.

```python
import os
import csv

# Pfad zur CSV-Datei innerhalb des Repositories
DATA_PATH = os.path.join("data", "glasarchitektur_vorgaben_bw.csv")

def filter_vorgaben():
    if not os.path.exists(DATA_PATH):
        print(f"Fehler: Datei nicht gefunden unter {DATA_PATH}")
        return

    print("=== REGIONALE AUFLAGEN FÜR GEBÄUDEHÜLLEN (Fokus: Vogelschutz & Sonnenschutz) ===\n")
    
    with open(DATA_PATH, mode="r", encoding="utf-8") as file:
        # Einlesen der CSV mit Semikolon-Delimiter
        reader = csv.DictReader(file, delimiter=";")
        
        for row in reader:
            stadt = row["Stadt"]
            vogelschutz = row.get("Vogelschutz_Richtlinie_Glas", row.get("Auflagen_Vogelschutz_Glas", ""))
            sonnenschutz = row.get("Sonnenschutz_Reflexionsgrad", row.get("Auflagen_Sonnenschutz_Verschattung", ""))
            
            # Filter-Logik: Zeige Städte mit akuter Nachrüst-Relevanz (z.B. Stuttgart, Ludwigsburg, Tübingen)
            if any(keyword in vogelschutz for keyword in ["Verpflichtend", "Streng", "folie"]) or \
               any(keyword in sonnenschutz for keyword in ["Verschattung", "folie"]):
                
                print(f"📍 Region: {stadt}")
                print(f"   🦅 Vogelschutz-Auflage: {vogelschutz}")
                print(f"   ☀️ Hitzeschutz-Vorgabe: {sonnenschutz}")
                print("-" * 60)

if __name__ == "__main__":
    filter_vorgaben()
