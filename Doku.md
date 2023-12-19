# Projekt TS für Dr. Med. Müller

## Übersicht

Das Projekt TS für Dr. Med. Müller zielt darauf ab, eine zuverlässige und effiziente Backup- und Wiederherstellungsstrategie für den Terminal Server von Dr. Med. Müller zu implementieren. Die Sicherung und Wiederherstellung der Daten sind entscheidend, um einen reibungslosen Betrieb sowie den Schutz sensibler Informationen zu gewährleisten.

## Inhaltsverzeichnis


- [Projekt TS für Dr. Med. Müller](#projekt-ts-für-dr-med-müller)
  - [Übersicht](#übersicht)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Planung](#planung)
  - [Restore-Prozess](#restore-prozess)

## Planung

1. **Vorbereitung:**
   - Überprüfung der Veeam Backup & Replication Installation 
   - Sicherstellung der Netzwerkkonnektivität zwischen dem Veeam-Server und dem Terminal Server.

2. **Backup-Job erstellen:**
   - Starten von Veeam Backup & Replication und Erstellung eines neuen Backup-Jobs.
   - Auswahl der virtuellen Maschine des Terminal Servers als Ziel für den Backup-Job.

3. **Backup-Konfiguration:**
   - Konfiguration des Backup-Jobs entsprechend den Anforderungen, inklusive des Sicherungsschemas (täglich)
   - Aktivierung der Option für Anwendungskonsistenz, um konsistente Daten während des laufenden Betriebs sicherzustellen.

4. **Backup-Speicherziel festlegen:**
   - Auswahl eines geeigneten Speicherziels für Backups, sei es ein lokales Laufwerk, Netzwerkspeicher oder ein externes Backup-Repository.


5. **Verschlüsselung und Sicherheit:**
   - Aktivierung der Verschlüsselungsoption zur Sicherung der Backup-Daten.
   - Backup Zugriff nur für die nötigen IPs und User zulassen und Konfigurieren.

6. **Test der Wiederherstellung:**
  - Regelmäßige Durchführung von Tests zur Wiederherstellung, um sicherzustellen, dass eine schnelle und zuverlässige Wiederherstellung im Falle eines Datenverlusts möglich ist.

1. **Überwachung und Benachrichtigungen:**
   - Konfiguration von Überwachungs- und Benachrichtigungsoptionen, um jederzeit über den Status der Backups informiert zu sein.

2. **Dokumentation:**
   - Ausführliche Dokumentation des Backup-Verfahrens, einschließlich aller Konfigurationen und Einstellungen, für Referenzen und im Fall von Systemänderungen.

 3. **Regelmäßige Überprüfung:**
   - Planung regelmäßiger Überprüfungen des Sicherungsplans, um sicherzustellen, dass er den sich ändernden Anforderungen entspricht und weiterhin effektiv ist.

## Restore-Prozess

1. **Überprüfung der Backups:**
   - Stelle sicher, dass meine gesicherten Backups auf dem Veeam-Server vorhanden und in Ordnung sind.

2. **Bestimmen des Wiederherstellungsziels:**
   - Entscheide, ob ich den Terminal Server am gleichen Ort oder auf einem anderen wiederherstellen möchte.

3. **Restore-Job erstellen:**
   - Öffne Veeam und lege einen neuen Restore-Job fest. Dabei wähle ich den Terminal Server als Quelle aus.

4. **Wählen des Wiederherstellungszeitpunkts:**
   - Bestimme den Zeitpunkt, zu dem ich den Server wiederherstellen möchte.

5. **Starten der Wiederherstellung:**
   - Beginne den Restore-Job und beobachte den Fortschritt.

6. **Überprüfen nach der Wiederherstellung:**
   - Prüfe, ob alles auf dem Terminal Server ordnungsgemäß funktioniert.

7. **Informieren meines Teams:**
   - Teile relevanten Teams mit, dass der Wiederherstellungsprozess abgeschlossen ist.

8. **Dokumentation aktualisieren:**
   - Aktualisiere die Dokumentation für zukünftige Referenzen.

9. **Informieren**
   - Den Kunden über die erfolgreiche wiederherstellungen Ihrer Daten informieren.