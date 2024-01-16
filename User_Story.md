#HNO Arzt Dr. Med. Müller

Dr. med. Müller, geboren am 15. März 1975, ist  Hals-Nasen-Ohren-Arzt, der mit großer Hingabe und Fachkenntnis seinen Patienten in der Stadt Zürich dient. Mit über 20 Jahren Erfahrung in der HNO-Heilkunde hat sich Dr. Müller einen Ruf für exzellente Patientenversorgung und medizinische Kompetenz aufgebaut. Nach 10 Jahren Studieren entschied sich Herr Müller eine eigene Praxis aufzubauen. Mitlerweile arbeitet er seit 12 Jahren in seiner eigenen Praxis. Mit seinen 4 MPAs macht er täglich Kunden gesund.
<br>

Seit der Coronazeit läuft es in der Praxis Rund. Die MPAs werden täglich mit so vielen sachen überrumpelt das diese sogar am Wochenende zuhause Arbeiten müssen. Bis jetzt haben die MPAs immer Lokal auf dem PC oder Laptop zuhause gearbeitet. Problem darin ist, dass sie die ganzen Patientendaten Lokal auf den Computer abgespeichert werden und sie daher keine schlaue Backup lösung haben. Ausserdem haben die MPAs auf die Daten keinen Zugriff wenn sie von Zuhause aus arbeiten. Dies fällt natürlich auch Herr Müller auf. Ihm sind seine Kundendaten das A und O. Schon seit langem wollte er dagegen etwas unternehmen. Nun ist es aber Zeit. Er gewinnt immer mehr Kunden und immer mehr Daten sind ungesichert auf dem Lokalen PC. Deswegen entscheided sich der Arzt eine Lösung bei seinem IT Partner zu finden.<br>

Die IT schlägt folgendes vor:

-Backupserver getrent von anderen Server(Sicherheit)

-Terminalserver für einfachen Zugriff auf Daten(auch von Zuhause aus) Da die Praxis mit Windows arbeitet machen wir ein Windows Terminal.

-Backupserver ist nicht im selben Netz wie der Terminalserver(Für sichere Backups)

-Client bleibt so wie er ist(Es wird einfach ein RDP Zugriff eingerichtet)

-Als Backup Software nehmen wir Veeam 12.0 für Backups über SSH.

-checkmk für Monitoring

-Raid 05 für den Terminalserver
