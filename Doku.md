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
  - [Veeam:](#veeam)
  - [Installation Probleme/Lösung](#installation-problemelösung)

## Planung

![PLAN](./Netzwerkplan%20143.png)



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

Hier die Berrechnung von den beiden Server was das alles bei AWS Kosten würde:
![PLAN](./Kosten%20144.png)
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

**AWS**
Die konfiguration in AWS ist relative Simple. Im Berreich VPC können wir unsere 2 Subnetze wie geplant Konfigurieren.
Das unsere 2 Instancen auch dauerhaft erreichbar sind und nicht ständig die IP-Adresse ändern, konfigurieren wir eine sogenannte Elastic IP in AWS. Diese können wir später den beiden Server zu weisen. Jetzt können wir aber unsere Server aufsetzen. Dazu klicken wir auf Instances Starten.
Dort bennen wir unsere Server und wählen unten unseren Windows 2022 Server aus.
Instance-TYP nehmen wir ja wie vorher geschrieben t2.large. Schlüsselpar nehmen wir SSH-1 den ich mal erstellt habe. Bei den Netzwerk einstellungen wählen wir Sicherheitsgruppen erstellen, und konfigurieren das ganze so:

TCP Port 22 (SSH)
TCP Port 3389 (RDP)
TCP Port 3389 für RDP (Remote Desktop Protocol)
TCP Port 443 (HTTPS)
TCP Port 9401 (Veeam Backup Service)

Für den Speicher müssen wir nicht Konfigurieren. Das tolle an einer Cloud ist nähmlich das man im laufenden betrieb Festplatten hinzufügen kann.
Anschliessen können wir noch unsere Festplatten den Servern zu weisen.
Und dann sind wir berreits ready für die Windows Konfigurationen.

**Windows Konfiguration**

Die Windows/Backup Konfiguration ist das wichtigste in unserem Projekt. Deswegen werde ich bei dieser Dokumentation jeden schritt genau erklären.

**RAID 5 Einrichtung:**
Um ein RAID 5-Array in Windows mit drei 52GB Festplatten zu konfigurieren, beginne ich zunächst damit, die Datenträgerverwaltung zu öffnen. Das mache ich, indem ich mit der rechten Maustaste auf das Startmenü klicke und "Datenträgerverwaltung" auswähle. Dort sehe ich alle angeschlossenen Laufwerke. Da RAID 5 eine Mindestanzahl von drei Festplatten benötigt, stelle ich sicher, dass meine drei 52GB Festplatten erkannt werden und als "Nicht zugeordnet" aufgeführt sind.

Als Nächstes initialisiere ich jede der drei Festplatten als dynamische Datenträger. Dazu klicke ich mit der rechten Maustaste auf jede Festplatte und wähle "Datenträger in dynamischen Datenträger konvertieren". Dynamische Datenträger sind für die Erstellung von RAID-Volumes erforderlich.

Nachdem alle Festplatten zu dynamischen Datenträgern konvertiert wurden, erstelle ich das RAID 5-Volume. Ich klicke mit der rechten Maustaste auf einen der dynamischen Datenträger und wähle die Option "Neues RAID-5-Volume erstellen". Ein Assistent führt mich durch den Prozess. Ich füge alle drei Festplatten zum Volume hinzu und folge den Anweisungen, um das Volume zu konfigurieren, einschließlich der Zuweisung eines Laufwerkbuchstabens und der Formatierung des Volumes mit dem Dateisystem NTFS.


**Nettime:**

Um die Zeitsynchronisation in Dr. med. Müllers Praxis zu gewährleisten, habe ich NetTime auf dem Terminalserver sowie dem Backupserver installiert. Dies stellt sicher, dass alle Systeme synchron laufen, was für die Konsistenz von Patientendaten und die zeitliche Abstimmung der Backups kritisch ist.

Ich startete mit dem Download der neuesten NetTime-Version von der offiziellen Webseite, die mit unseren Windows-Servern kompatibel ist. Die Installation führte ich auf beiden Servern durch, indem ich die heruntergeladene Datei als Administrator ausführte. Nach dem Akzeptieren der Lizenzvereinbarung und der Auswahl des Installationspfades komplettierte ich die Installation durch einen Neustart der Systeme.

Nach dem Neustart stellte ich auf beiden Servern sicher, dass NetTime läuft und die Zeit mit einem zuverlässigen Zeitserver abgleicht. Diese Schritte gewährleisten, dass sowohl der Terminal- als auch der Backupserver von Dr. Müller stets die exakte Zeit verwenden, was für die Präzision in der Datenverarbeitung und -sicherung unerlässlich ist.

## Veeam:
Die Installation von Veeam ist relative einfach gestaltet. Ich bin aber trozdem auf ein paar Probleme gestossen. Dazu aber später mehr.

Um Veeam zu Installieren bin ich wieder auf die Offiziele seite gegangen, um dort das Offiziele File Herunterladen zu können. Sobald dies Heruntergeladen wurde konnte ich das ISO File öffnen. Anschliessend konnte ich beim ISO Laufwerk dass Veeam Setup ausführen.
Dannach haben wir 3 Optionen:

Veeam Backup & Replication Installieren: Diese Option wählte ich, um die vollständige Veeam Backup & Replication-Lösung auf meinem Server zu installieren. Sie umfasst alle notwendigen Komponenten für das Backup und die Wiederherstellung von virtuellen, physischen und Cloud-basierten Workloads.

Veeam ONE Installieren: Veeam ONE bietet erweiterte Überwachungs- und Reporting-Funktionen für Backup-Infrastrukturen. Es handelt sich um eine separate Komponente, die für umfassende Einblicke und Analysen in die Backup-Umgebung sorgt. Obwohl es für die grundlegende Backup- und Wiederherstellungsfunktion nicht zwingend erforderlich ist, kann es wertvolle Informationen zur Leistung und Gesundheit der Backup-Infrastruktur liefern.

Veeam Backup & Replication Konsole Installieren: Die Installation der reinen Management-Konsole wäre eine Option gewesen, wenn ich die zentralisierte Verwaltung der Veeam-Infrastruktur von einem separaten Arbeitsplatz aus planen wollte, ohne die vollständige Veeam Backup & Replication Suite auf diesem System zu installieren. Diese Option ist besonders nützlich, wenn Administratoren von verschiedenen Standorten aus auf die Backup-Umgebung zugreifen müssen.
Für die Bedürfnisse der Praxis von Dr. med. Müller und um eine direkte und effiziente Verwaltung der Backups sicherzustellen, entschied ich mich für die erste Option: die vollständige Installation von Veeam Backup & Replication. Diese Wahl stellte sicher, dass ich Zugriff auf alle erforderlichen Tools für das Backup-Management direkt auf dem Backupserver hatte, was die Konfiguration und Überwachung der Backup-Jobs erleichtert.

Nachdem ich die Installationsroutine von Veeam Backup & Replication gestartet hatte, stand ich vor der Entscheidung, ein Ziel für die Backup-Daten zu wählen. Für die optimale Nutzung der verfügbaren Ressourcen und zur Gewährleistung der Datensicherheit entschied ich mich, das RAID 5-Laufwerk als Speicherort für die Backups zu verwenden. Dieses Laufwerk bietet dank seiner Konfiguration eine gute Balance zwischen Speicherkapazität und Datensicherheit.
Bezüglich der Lizenzierung von Veeam Backup & Replication stellte ich fest, dass für die aktuelle Konfiguration der Praxis keine Lizenz erforderlich ist. Veeam bietet eine Community Edition an, die für kleinere Umgebungen oder für eine begrenzte Anzahl von Workloads kostenlos genutzt werden kann. Da in Dr. med. Müllers Praxis weniger als 10 PCs in die Backup-Strategie einbezogen werden müssen, fiel die Entscheidung auf die Nutzung dieser kostenfreien Version.

Nachdem Veeam Backup & Replication erfolgreich Heruntergeladen ist, können wir mit der Konfiguration beginnen.


**Server Hinzufügen**
In Veeam Backup & Replication öffnete ich den Bereich „Backup Infrastructure“ und wählte „Add Backup Repository“. Im Assistenten entschied ich mich für den Speichertyp, der am besten zu meiner Konfiguration passt, basierend auf dem Speichermedium, das ich für die Backups nutzen wollte. Ich gab dem Repository einen eindeutigen Namen, der es mir erlaubt, es leicht zu identifizieren, z.B. „Backup Repository Dr. Müller“. Dann wies ich dem Repository einen Speicherort zu, in diesem Fall das RAID 5-Laufwerk, das ich bereits eingerichtet hatte, um eine hohe Datensicherheit und Ausfallsicherheit für die Backups zu gewährleisten. Nachdem ich den Speicherpfad ausgewählt und die Größe des Repositories festgelegt hatte, überprüfte Veeam die Konfiguration. Mit einem Klick auf „Finish“ schloss ich den Vorgang ab, und das neue Backup Repository war erfolgreich hinzugefügt und bereit für die Sicherung der Daten.

Das sieht nachher wie folgt aus:

![PLAN](./inventory.png)

Wichtig ist dabei das man einen Rescan macht. Dann sieht man nähmlich ob alle Ports offen sind und der Terminalserver erreichbar ist. Nicht das dies nachher Probleme gibt.


























## Installation Probleme/Lösung