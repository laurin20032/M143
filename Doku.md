# Projekt TS für Dr. Med. Müller

## Übersicht 

Das Projekt TS für Dr. Med. Müller zielt darauf ab, eine zuverlässige und effiziente Backup- und Wiederherstellungsstrategie für den Terminal Server von Dr. Med. Müller zu implementieren. Die Sicherung und Wiederherstellung der Daten sind entscheidend, um einen reibungslosen Betrieb sowie den Schutz sensibler Informationen zu gewährleisten.

## Inhaltsverzeichnis

- [Projekt TS für Dr. Med. Müller](#projekt-ts-für-dr-med-müller)
  - [Übersicht](#übersicht)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Planung](#planung)
    - [RAID 5 Konfiguration](#raid-5-konfiguration)
    - [AWS Cloud Konfiguration](#aws-cloud-konfiguration)
- [Backupserver-Konzept](#backupserver-konzept)
  - [Einleitung](#einleitung)
  - [Backup-Lösung](#backup-lösung)
  - [Veeam Vorteile](#veeam-vorteile)
  - [Systemanforderungen für Veeam](#systemanforderungen-für-veeam)
  - [Backupserver-Konfiguration](#backupserver-konfiguration)
  - [Backup-Strategie](#backup-strategie)
  - [Backup-Plan](#backup-plan)
  - [Strategische Überlegungen](#strategische-überlegungen)
  - [Restore-Prozess](#restore-prozess)
- [Schritte meines Restore-Prozesses](#schritte-meines-restore-prozesses)
- [Datensicherungskonzept](#datensicherungskonzept)
- [Installation und Konfiguration](#installation-und-konfiguration)

## Planung





**Terminalserver Konzept:**  
Unser Terminalserver wird in einem anderen Netzwerk sein als der Backupserver. Die Entscheidung, den Terminalserver und den Backupserver in verschiedenen Netzwerken zu platzieren, dient der Sicherheit. Falls ein Netzwerk beeinträchtigt wird, bleibt das andere unberührt, was das Risiko von Datenverlust oder Betriebsunterbrechungen reduziert.

### RAID 5 Konfiguration
Um meinen Terminalserver für Herrn Müller in der AWS-Cloud einzurichten, plane ich eine RAID 5-Konfiguration auf dem Windows-Server, der speziell für seine Praxis betrieben wird. Dazu füge ich dem Server mehrere Amazon Elastic Block Store (EBS) Festplatten hinzu. Diese Festplatten werde ich dann in Windows so konfigurieren, dass sie zusammen als ein RAID 5-System arbeiten.

Durch die Einrichtung von RAID 5 werden die Daten von Herrn Müllers Praxis auf mehrere Festplatten verteilt, wobei Paritätsinformationen genutzt werden, um die Datenintegrität zu sichern. Falls eine der Festplatten ausfallen sollte, ermöglicht es mir diese Konfiguration, alle Daten ohne Verlust wiederherzustellen. Diese Methode bietet eine wichtige Sicherheitsebene für die sensiblen Patientendaten, die auf dem Terminalserver gespeichert sind.


### AWS Cloud Konfiguration
Da Herr Müller sein neues Netzwerk in der Cloud aufbauen möchte, werden wir den Terminalserver in der AWS Cloud konfigurieren. AWS ist kostengünstig und relativ leicht zu bedienen.

Folgende Anforderungen werden wir dort konfigurieren:
- IP-Adresse: 18.210.9.134 (Öffentlich für den SSH Zugriff)
- Subnetz: 172.31.120.0/28
- Instance-Typ: t2.large (8Gb Ram und 2 vCPUs)
- OS: Windows 2022 Server
- Port: SSH(22) RDP(3389)

# Backupserver-Konzept

## Einleitung
Der Backupserver ist das wichtigste bei unserem Projekt, da er die Grundlage für die Datensicherheit bildet. Durch regelmäßige automatisierte Backups gewährleistet er die Verfügbarkeit und Integrität unserer Daten. Im Falle eines Ausfalls oder einer Störung des Terminalservers ermöglicht der Backupserver eine schnelle Wiederherstellung, minimiert potenziellen Datenverlust und sorgt für eine kontinuierliche Geschäftskontinuität. Eine zuverlässige Backupstrategie ist daher essenziell für die Stabilität und Sicherheit unserer gesamten IT-Infrastruktur.

## Backup-Lösung
Für das Backup werden wir Veeam verwenden.

### Veeam Vorteile
- Schnelle Wiederherstellung
- Automatisierung und Planung
- Cloud-Integration
- Monitoring und Reporting
- Replikation für Hochverfügbarkeit
- SSH

## Systemanforderungen für Veeam
Min Windows Server 2016 und min 2Gb Speicher frei für die Installation.

## Backupserver-Konfiguration
Auf den folgenden Angaben bauen wir unseren Backupserver auf:
- IP-Adresse: 44.219.232.33
- Subnetz: 172.31.48.0/20
- Instance-Typ: t2.large (8Gb Ram und 2 vCPUs)
- OS: Windows 2022 Server
- Port: SSH(22) RDP(3389)

## Backup-Strategie
### Backup-Plan
| Backup-Typ          | Zeitplan            | Zweck/Beschreibung                                       |
|---------------------|---------------------|----------------------------------------------------------|
| Full Backup         | Donnerstag 22:00 Uhr| Wöchentliches vollständiges Backup außerhalb der Geschäftszeiten. |
| Full Backup         | Sonntag 22:00 Uhr   | Wochenend-Backup für zusätzliche Sicherheit und Datenintegrität.  |
| Inkrementelles Backup | Montag 22:00 Uhr    | Regelmäßige Datensicherung zu Beginn der Woche.          |
| Inkrementelles Backup | Dienstag 22:00 Uhr  | Datensicherung zur Mitte der Woche.                       |
| Inkrementelles Backup | Mittwoch 22:00 Uhr  | Sicherung vor dem wöchentlichen Full Backup.              |
| Inkrementelles Backup | Freitag 22:00 Uhr   | Abschluss der Arbeitswoche mit Datensicherung.            |

### Strategische Überlegungen
In der Praxis von Herrn Müller habe ich mich für eine spezifische Strategie für die Datensicherung entschieden, um den Schutz wichtiger Unternehmensdaten zu gewährleisten:

Für die **Backup-Benachrichtigungen** haben ich mich entschieden, eine Gmail-App zu verwenden und diese in Veeam zu integrieren. Dadurch können wir automatische E-Mail-Benachrichtigungen über den Status unserer Backups erhalten. Die Einrichtung ist unkompliziert: Wir erstellen eine Gmail-App, konfigurieren die SMTP-Einstellungen in Veeam mit den Anmeldeinformationen der Gmail-App und führen einen Test durch, um die Funktionalität zu gewährleisten. Mit dieser Lösung können wir den Backup-Status effektiv überwachen.

Um die Sicherheit der Backup-Daten zu erhöhen, habe ich entschieden, alle Backups zu verschlüsseln. Diese **Verschlüsselung** realisiere ich in Veeam unter Verwendung eines starken, sicheren Passworts. Dadurch stelle ich sicher, dass die Daten auch im Falle eines unbefugten Zugriffs geschützt sind

## Restore-Prozess
Ich habe einen genauen Restore-Prozess in Veeam entwickelt, der folgende Schritte umfasst:

### Schritte meines Restore-Prozesses

1. **Überprüfung des Backup-Status**
   - Ich öffne die Veeam Backup & Replication Konsole.
   - Im Abschnitt „Backups“ überprüfe ich den Status der letzten Backups und stelle sicher, dass sie erfolgreich abgeschlossen wurden.

2. **Auswahl des Wiederherstellungspunktes**
   - Ich wähle das gewünschte Backup aus und klicke auf „Restore“.
   - Je nach Bedarf wähle ich „Entire VM restore“ für eine vollständige VM-Wiederherstellung oder „Guest files restore“ für spezifische Dateien.

3. **Vorbereitung des Restore-Jobs**
   - Im Restore-Assistenten wähle ich den spezifischen Wiederherstellungspunkt basierend auf Datum und Uhrzeit.
   - Ich bestätige die Auswahl der VM oder Dateien, die wiederhergestellt werden sollen.

4. **Durchführung der Wiederherstellung**
   - Ich wähle das Ziel für die Wiederherstellung, entweder den ursprünglichen Standort oder einen neuen Standort.
   - Nach der Überprüfung der Einstellungen starte ich den Wiederherstellungsprozess.

5. **Überprüfung nach der Wiederherstellung**
   - Nach Abschluss überprüfe ich, ob die VM oder Dateien korrekt wiederhergestellt wurden.
   - Ich teste die Funktionalität, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

6. **Dokumentation und Reporting**
   - Ich dokumentiere jeden Schritt des Prozesses, einschließlich der Auswahl des Wiederherstellungspunktes.
   - Mit der „Reports“-Funktion in Veeam generiere ich einen Bericht über den Restore-Job.




Durch die Befolgung dieses Prozesses stelle ich sicher, dass die Wiederherstellung präzise und zuverlässig erfolgt, minimiere das Risiko von Datenverlusten und gewährleiste die Integrität unserer Systeme.


## Datensicherungskonzept


## Installation und Konfiguration

