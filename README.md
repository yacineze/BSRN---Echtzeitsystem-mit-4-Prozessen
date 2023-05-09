# BSRN---Echtzeitsystem-mit-4-Prozessen
Das Ziel des Projekts ist die Implementierung eines Echtzeitsystems mit vier Prozessen, die 
miteinander kommunizieren. Die vier Prozesse heißen Conv, Log, Stat und Report und sollen 
die Messwerte von Analog-Digital-Wandlern simulieren und statistische Daten berechnen.
Lösungsansatz: 
Für die Interprozess-Kommunikation (IPC) werden verschiedene Techniken verwendet, 
darunter Pipes, Message Queues, Shared Memory mit Semaphore und Sockets.
1. Pipes: Conv schreibt Daten in eine Pipe und Log und Stat lesen die Daten aus der Pipe. 
Stat schreibt die statistischen Daten in eine andere Pipe, und Report liest die statistischen 
Daten aus der Pipe.
2. Message Queues: Conv sendet die Daten an eine Message Queue, und Log und Stat 
empfangen die Daten aus der Message Queue. Stat sendet die statistischen Daten an 
eine andere Message Queue, und Report empfängt die statistischen Daten aus der 
Message Queue.
3. Shared Memory mit Semaphore: Conv schreibt Daten in den gemeinsam genutzten 
Speicher, und Log und Stat lesen die Daten aus dem Speicher. Stat schreibt die 
statistischen Daten in einen anderen freigegebenen Speicherbereich, und Report liest 
die statistischen Daten aus dem freigegebenen Speicher. Semaphoren werden 
verwendet, um den Zugriff auf gemeinsam genutzte Speicherbereiche zu 
synchronisieren.
4. Sockets: Conv sendet die Daten an einen Socket, und Log und Stat empfangen die Daten 
vom Socket. Stat sendet die statistischen Daten an einen anderen Socket, und Report 
empfängt die statistischen Daten vom Socket.
Pipes
1. Conv:
- Erstellen von Pipe, um Daten an den nächsten Prozess (Log) zu senden.
- Generieren von Zufallszahlen, um die Messwerte von Analog-Digital-Wandlern zu 
simulieren.
- Daten in die Pipe schreiben, die vom nächsten Prozess gelesen werden sollen.
2. Log:
- Erstellen von Pipe, um Daten aus dem vorherigen Prozess (Conv) zu empfangen.
- Daten aus der Pipe lesen
- Daten in eine lokale Datei schreiben
3. Stat:
- Erstellen von Pipe, um Daten aus dem vorherigen Prozess (Conv) zu empfangen.
- Daten aus der Pipe lesen.
- Statistische Daten berechnen (Mittelwert und Summe)
- Daten in eine Pipe schreiben, damit sie vom nächsten Prozess (Report) gelesen 
werden 
4. Report:
- Erstellen von Pipe, um Daten aus dem vorherigen Prozess (Stat) zu empfangen.
- Daten aus der Pipe lesen
- Die statistischen Daten auf der Konsole ausgeben
Message Queues
- Erstellen einer Message Queue für jeden Kommunikationskanal zwischen Prozessen
- Verwendung von fork für den Conv-Prozess und Verwendung von Message Queue, 
um Daten an den Log-Prozess zu senden.
- Verwendung von fork für den Log-Prozess und Verwendung von Message Queue, 
um Daten an den Stat-Prozess zu senden.
- Verwendung von fork für den Stat-Prozess und Verwendung von Message Queue, 
um Daten an den Report-Prozess zu senden.
- Verwendung von fork für den Report-Prozess und Verwendung von Message Queue, 
um Daten vom Stat-Prozess zu empfangen.
- Jeder Prozess sollte aus seiner Eingangs-Message Queue lesen und in seine 
Ausgangs-Message Queue schreiben
Shared Memory mit Semaphore
1. Erstellen von Shared-Memory-Segment mit dem Systemaufruf shmget(), um einen 
eindeutigen Schlüssel für das Shared-Memory-Segment zu erhalten.
2. Anhängen des gemeinsam genutzte Speichersegments mit dem Systemaufruf shmat() an 
den Adressraum jedes Prozesses.
3. Verwendung von sem_init(). Es wird zwei Semaphore geben: eine, um den Zugriff auf das 
gemeinsam genutzte Speichersegment zu synchronisieren, und eine andere, um zu 
signalisieren, wann die statistischen Daten zum Lesen bereit sind.
4. Generieren von Zufallszahlen im Conv-Prozess und diese in das gemeinsame 
Speichersegment schreiben. Verwendung der ersten Semaphore, um den gegenseitigen 
Ausschluss beim Zugriff auf das gemeinsame Speichersegment sicherzustellen.
5. Im Log-Prozess die Daten aus dem Shared-Memory-Segment lesen und diese in eine lokale 
Datei schreiben.
6. Im Stat-Prozess die Daten aus dem gemeinsamen Speichersegment lesen und die 
statistischen Daten (Mittelwert und Summe) berechnen. Die statistischen Daten in das 
gemeinsame Speichersegment schreiben. Verwendung der zweiten Semaphore, um zu 
signalisieren, dass die statistischen Daten zum Lesen bereit sind.
7. Im Report-Prozess die statistischen Daten aus dem gemeinsam genutzten Speichersegment
lesen und diese an die Shell ausgeben. Verwendung der zweiten Semaphore, um zu warten, 
bis die statistischen Daten bereit sind.
Sockets
Um die Kommunikation zwischen den Prozessen mithilfe von Sockets zu implementieren, 
werden wir:
- die Funktion socket() verwenden, um einen neuen Socket zu erstellen, 
- bind(), um den Socket an eine bestimmte Adresse und einen bestimmten Port zu 
binden, 
- listen(), um den Socket zu einem Server zu machen und darauf zu warten,
- accept(), um eingehende Verbindungen zu akzeptieren, 
- connect(), um eine Verbindung zu einem Server herzustellen, 
- send() und recv(), um Daten über den Socket zu senden und zu empfangen
- und close(), um den Socket zu schließen, wenn er fertig ist.
