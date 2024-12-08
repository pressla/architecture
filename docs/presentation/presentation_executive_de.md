---
title: BIM KI-Architektur excutive summary
author: Architektur-Team
date: 2024
theme: metropolis
---

# Zielsetzung und Umfang

- Entwicklung eines Systems zur Identifizierung, Klassifizierung und Extraktion von Bauelementen aus vektorisierten Grundrissen
- Erstellung von 3D-BIM-Modellen und Generierung von Produktionsstücklisten
- Erkennung von Wänden, Öffnungen und Abmessungen
- Zuordnung zu BIM-Elementen mit Standardwerten für Höhe/Fensterposition
- Automatisierte Pipeline für skalierbare Verarbeitung architektonischer Datensätze
- Reduzierung manueller Arbeit bei gleichzeitiger Präzisionssicherung

---

# Funktionen und Herausforderungen

**Wichtige zu extrahierende Merkmale:**
- Wandsegmente (Umfang und Innenwände)
- Öffnungen (Türen und Fenster)
- Geometrische Beziehungen

**Zentrale Herausforderungen:**
- Grundrisse enthalten keine 3D-Daten
- Zeichnungsunregelmäßigkeiten erfordern Vorverarbeitung
- Saubere Dateneingabe für Modelltraining erforderlich
- Optimierung des automatisierten Workflows

---

# Vorverarbeitung mit Shapely

**Wandverarbeitung:**
- Extraktion von Linien/Polylinien
- Bestimmung der Wandstärke
- Rechteckige Ausrichtung der Segmente

**Öffnungserkennung:**
- Identifizierung von Lücken
- Approximation von Standardabmessungen
- Klassifizierung von Fenstern und Türen

**Beziehungszuordnung:**
- Verknüpfung von Öffnungen mit Wänden
- Klassifizierung von Wandtypen
- Sicherstellung der Datenqualität für ML-Modelle

---

# Lernphase-Architektur

**Datenverarbeitung:**
- Merkmalsextraktion mit ezdxf und Shapely
- Augmentierungen für Modellrobustheit
- Skalierungs- und Rotationsvariationen

**Modelltraining:**
- PyTorch-Implementierung
- NVIDIA DALI für beschleunigte Datenverarbeitung
- TensorBoard/WandB Metrik-Tracking

**Evaluierung:**
- F1-Score Metriken
- Konfusionsmatrizen
- Bewertung der Modellzuverlässigkeit

---

# Inferenz-Pipeline

**Echtzeit-Verarbeitung:**
- Grundriss-Eingabeverarbeitung
- Element-Erkennung und Klassifizierung
- Optimierte Modellinferenz

**Technischer Stack:**
- ONNX-Optimierung
- NVIDIA TensorRT-Deployment
- System mit niedriger Latenz und hohem Durchsatz

**Ausgabegenerierung:**
- 3D-Modellerstellung
- Stücklistengenerierung
- BIM-Workflow-Integration

---

# Rechen- und Speicheranforderungen

**Trainingsanforderungen:**
- ~2,5 PFLOPs für 1000 Pläne
- 10M-Parameter-Modell
- 50 Epochen Standardtraining
- NVIDIA A100 GPU (~312 TFLOPs)

**Inferenz-Spezifikationen:**
- 50 GFLOPs pro Vorwärtsdurchlauf
- NVIDIA T4/Jetson-Verarbeitung
- 6240 Pläne/Sekunde Durchsatz
- Optimierte Ressourcennutzung

---

# Geschäftsauswirkungen und ROI

**Hauptvorteile:**
- Reduzierung des automatisierten Workflows
- Verringerte manuelle Arbeitskosten
- Minimierte Fehlerquoten
- Verbesserte Skalierbarkeit

**Wettbewerbsvorteile:**
- Schnellere Projektabwicklung
- Reduzierte Betriebskosten
- Verbesserte Kundenzufriedenheit
- Führungsposition in der Branche

---

# Nächste Schritte

1. Aufbau der Vorverarbeitungs-Pipeline
2. Training des CNN-Transformer-Modells
3. Deployment des optimierten Inferenzsystems
4. Messung der ROI-Metriken:
   - Zeitersparnis
   - Kostenreduzierung
   - Genauigkeitsverbesserungen
   - Projektskalierbarkeit
