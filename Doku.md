# Projekt TS für Dr. Med. Müller

## Übersicht 

Das Projekt TS für Dr. Med. Müller zielt darauf ab, eine zuverlässige und effiziente Backup- und Wiederherstellungsstrategie für den Terminal Server von Dr. Med. Müller zu implementieren. Die Sicherung und Wiederherstellung der Daten sind entscheidend, um einen reibungslosen Betrieb sowie den Schutz sensibler Informationen zu gewährleisten.

## Inhaltsverzeichnis


- [Projekt TS für Dr. Med. Müller](#projekt-ts-für-dr-med-müller)
  - [Übersicht](#übersicht)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Planung](#planung)
  - [Restore-Prozess](#restore-prozess)
  - [Datensicherungskonzept](#datensicherungskonzept)
## Planung
Für die Installation von mehreren Servern braucht man eine gute Planung. Deswegen sollte man Hardware anforderungen und Software anforderungen genau planen. 

**Terminalserver:**
Unser Terminalserver wird in einem anderen Netzwerk sein als der Backupserver.
Die Entscheidung, den Terminalserver und den Backupserver in verschiedenen Netzwerken zu platzieren, dient der Sicherheit. Falls ein Netzwerk beeinträchtigt wird, bleibt das andere unberührt, was das Risiko von Datenverlust oder Betriebsunterbrechungen reduziert.

Für den Terminalserver setzen wir strenge Zugriffsbeschränkungen und Sicherheitsmaßnahmen ein, um unbefugten Zugriff zu verhindern. Der Backupserver in einem separaten Netzwerk sorgt dafür, dass im Falle eines Ausfalls reibungslos auf gesicherte Daten zugegriffen werden kann.

Diese einfache, aber effektive Architektur ermöglicht es uns, die Sicherheit unserer Daten und die Verfügbarkeit unserer Dienste zu gewährleisten.


Außerdem werde ich ein RAID 5 einrichten, um Daten sicherer zu speichern. Bei RAID 5 werden Daten über mehrere Festplatten verteilt und durch Paritätsinformationen geschützt. Das ermöglicht eine gewisse Ausfallsicherheit – selbst wenn eine Festplatte ausfällt, bleiben die Daten erhalten. Dies schafft eine zusätzliche Sicherheitsebene für den Terminalserver. Regelmäßige Überprüfungen des RAID-Arrays werden dafür sorgen, dass die Datenintegrität aufrechterhalten bleibt.

Da Herr Müller sein neues Netzwerk in der Cloud aufbauen möchte werden wir den Terminalserver in der AWS Cloud Konfigurieren. AWS ist Kostengünstig und relative leicht zu bedienen.

Folgende anforderungen werden wir dort Konfigurieren:

IP-Adresse: 18.210.9.134(Öffentlich für den SSH Zugriff)
Subnetz: 172.31.120.0/28
Instance-Typ: t2.large (8Gb Ram und 2 vCPUs)
OS: Windows 2022 Server
Port: SSH(22) RDP(3389)
Dass t2.large würde logischerweise für den Kunden mit seinen 4MPAs nicht reichen. Da wir aber nur begrenz Guthaben haben werde ich dies verwenden.
Wir verwenden Windows 2022, weil es unseren Anforderungen an eine stabile und leistungsfähige Serverplattform entspricht. Klar könnten wir Ubuntu verwenden da aber die ganze Praxis mit Windows arbeitet und einen RDP zugriff auf den Terminalserver braucht werden wir Windows verwenden. Ausserdem muss der Backup Server natürlich per SSH auf den Terminalserver zugreifen können. Für das Konfiguriere ich den Port 22 auf AWS und in den Windows Firewall einstellungen.

**Backupserver:**
Der Backupserver ist das wichtigste bei unserem Projekt, da er die Grundlage für die Datensicherheit bildet. Durch regelmäßige automatisierte Backups gewährleistet er die Verfügbarkeit und Integrität unserer Daten. Im Falle eines Ausfalls oder einer Störung des Terminalservers ermöglicht der Backupserver eine schnelle Wiederherstellung, minimiert potenziellen Datenverlust und sorgt für eine kontinuierliche Geschäftskontinuität. Eine zuverlässige Backupstrategie ist daher essenziell für die Stabilität und Sicherheit unserer gesamten IT-Infrastruktur.

Da wir bei dem Terminalserver auch Windows verwenden werden wir diesmal auch wieder Windows verwenden. Für das Backup werden wir Veeam verwenden. 
Veeam Vorteile:

- Schnelle Wiederherstellung: Veeam ermöglicht schnelle Wiederherstellungen von Dateien, virtuellen Maschinen oder sogar ganzen Servern. Dies trägt dazu bei, Ausfallzeiten zu minimieren und die Betriebskontinuität sicherzustellen.

- Automatisierung und Planung: Durch die Automatisierung von Sicherungsprozessen und die Möglichkeit zur Planung von Backups kann Veeam dazu beitragen, menschliche Fehler zu reduzieren und sicherzustellen, dass regelmäßige Sicherungen durchgeführt werden.

- Cloud-Integration: Veeam unterstützt die Integration mit verschiedenen Cloud-Plattformen, was eine flexible Datenspeicherung und die Möglichkeit zur Nutzung von Cloud-Ressourcen für die Wiederherstellung bietet.

- Monitoring und Reporting: Veeam bietet umfassende Monitoring- und Reporting-Tools, um Administratoren Einblicke in den Status der Backups, Wiederherstellungen und die allgemeine Gesundheit des Systems zu geben.

- Replikation für Hochverfügbarkeit: Veeam ermöglicht die Einrichtung von Replikationen für virtuelle Maschinen, um eine Hochverfügbarkeit von kritischen Systemen sicherzustellen und im Notfall schnell auf eine alternative

- SSH: Veeam bietet eine einfache Verbindungen mit SSH. Dies ist Perfekt für unseren Terminalserver geeignet.
  
*Systemvoraussetzungen Veeam:*
Min Windows Server 2016.
Und min 2Gb Speicher Frei für die Installation.

Auf den Folgenden angaben bauen wir unser Backupserver auf:

IP-Adresse:44.219.232.33
Subnetz:172.31.48.0/20
Instance-Typ: t2.large (8Gb Ram und 2 vCPUs)
OS: Windows 2022 Server
Port: SSH(22) RDP(3389)

Für den Backupserver haben wir eine Fixe öffentliche IP Adresse verwendent. Dies hat den Voteil das wir nicht immer eine neue IP-Adresse verwenden müssen.
Wie man sieht ist der Backupserver in einem anderem Subnetz wie der Terminalserver. Als Instance-Typ verwenden wir wieder das gleiche wie beim Terminalserver. Der Backupserver bräuchte eigentlich nicht so viel Ram, weil er nicht dauerhaft verwendet wird. Da aber das Paket nicht so viel Kostet verwenden wir wieder das selbe.
Für das nehmen wir einfach wieder Windows 2022. Wir Konfigurieren ausserdem wieder den RDP Port sodass wir extern auf den Server kommen.




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



## Datensicherungskonzept

Für eine opitmale Sicherheit des Kunden möchten wir ein möglich gutes Datensicherungskozept.











