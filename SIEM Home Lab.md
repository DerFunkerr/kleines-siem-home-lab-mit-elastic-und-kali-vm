# Mein SIEM Home Lab mit ELK Stack & Kali Linux

Ich habe ein eigenes SIEM-Home-Lab aufgebaut, um meine Fähigkeiten in der Sicherheitsanalyse zu verbessern. Dafür habe ich den ELK Stack (Elasticsearch, Logstash)
in Kombination mit Kali Linux genutzt.

### Ziel

Mein Ziel war es, ein funktionierendes Lab zur Analyse und Erkennung von Sicherheitsereignissen einzurichten – praxisnah und realitätsnah.

### Was ich gemacht habe

- Kali Linux heruntergeladen und in einer VM installiert
    
- Einen SIEM-Agenten in Kali integriert
    
- Mit Tools wie Nmap gezielt Sicherheitsereignisse erzeugt
    
- Die Logdaten im ELK Stack überwacht und analysiert
    
- Eigene Dashboards in Kibana erstellt
    
- Alarme konfiguriert, um potenzielle Bedrohungen zu erkennen
    

### Mein Mehrwert

Durch dieses Lab konnte ich praxisnah lernen, wie man mit SIEM-Systemen arbeitet, Sicherheitsereignisse analysiert und effektive Überwachungsmechanismen aufsetzt. Es war ein wertvoller Schritt zur Vorbereitung auf reale Aufgaben im Bereich Security Operations.


<br><br>

## Schritt 1: Umgebung einrichten

Ich habe meine Arbeitsumgebung für das SIEM-Lab vorbereitet. Dazu gehörten folgende Schritte:

- **Kali Linux** als virtuelle Maschine eingerichtet (VirtualBox)
    
- **Elastic Cloud Account** erstellt und mein erstes Deployment angelegt
    
- Einen passenden Cloud-Anbieter und eine Region gewählt


    

Damit war die Grundlage geschaffen, um den ELK Stack in der Cloud zu nutzen und Kali Linux als Datenquelle zu integrieren.


![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/1.PNG?raw=true)



## Schritt 2: Elastic Agent & Integration einrichten

Auf meiner bereits eingerichteten **Kali Linux VM** habe ich den **Elastic Agent** installiert, um sie in mein Elastic Cloud Deployment einzubinden.

- Über das **Elastic Cloud Dashboard** zur Sektion **"Add Integrations"** navigiert

![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/2.PNG?raw=true)
    
- Die **Elastic Defend-Integration** ausgewählt und eingebunden

![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/3.PNG?raw=true)
- Den bereitgestellten **Installationsbefehl** in die Kali-VM kopiert und ausgeführt  
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/4.PNG?raw=true)
    

Die Installation des Agents dauerte einige Minuten. Danach habe ich überprüft, ob der Dienst korrekt läuft – mit folgendem Befehl:

```bash
sudo systemctl status elastic-agent.service
```

![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/5.PNG?raw=true)
**Ergebnis:** Der Agent war erfolgreich installiert, aktiv und meine Kali-VM war nun an **Elastic Cloud angebunden** – bereit, Sicherheitsereignisse zu erfassen und weiterzuleiten.

## Schritt 3: Sicherheitsereignisse erzeugen und SIEM-Logs überwachen

Um Sicherheitsereignisse zu generieren, habe ich in meiner Kali-VM im Terminal den folgenden Befehl ausgeführt:

```bash
sudo nmap -A -sV localhost
```
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/6.PNG?raw=true)

Dieser Scan erzeugt diverse Netzwerkereignisse, die vom Elastic Agent erfasst und an den SIEM weitergeleitet werden.


Anschließend habe ich im **Elastic SIEM Dashboard** unter  **Analytics > Discover** geprüft, ob Alarme ausgelöst wurden.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/7.PNG?raw=true)


Dort konnte ich die vom Nmap-Scan generierten Ereignisse einsehen und analysieren.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/8.PNG?raw=true)


Anschließend bin ich zum zweiten Testschritt übergegangen, um zu prüfen, ob bei verdächtigem Verhalten ein Alarm ausgelöst wird. Dafür habe ich eine einfache Metasploit-Session gestartet:

```bash
msfconsole use exploit/unix/ftp/vsftpd_234_backdoor set RHOST <targetIP> exploit
```

Das Ergebnis war eindeutig: **Elastic Defend** erkannte die Aktivität sofort und meldete einen sicherheitsrelevanten Vorfall. Im SIEM erschien ein Alarm mit einem **Risiko-Score von 73** und einer Bewertung als **hoch**.

![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/9.PNG?raw=true)

Besonders hilfreich war, dass das Dashboard detaillierte Informationen zum Vorfall anzeigte – unter anderem den Pfad zur ausführenden Datei (`/usr/bin/msfconsole`), was auf den Einsatz von Metasploit hinwies.

Ich könnte über die Benutzeroberfläche direkt einen Sicherheitsfall eröffnen und den betroffenen Host isolieren. Im Falle eines echten Vorfalls wäre es so möglich, gezielt Gegenmaßnahmen einzuleiten und eine weitere Ausbreitung zu verhindern.


## Schritt 4: Dashboard zur Visualisierung erstellen

Als nächstes habe ich ein Dashboard erstellt, um die gesammelten Logs im SIEM übersichtlich darzustellen. Das ging relativ einfach:

- Im **Analytics-Tab** habe ich für die horizontale Achse die **Zeitstempel (timestamps)** ausgewählt
    
- Für die vertikale Achse habe ich die **Anzahl der Ereignisse (count)** genutzt
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/10.PNG?raw=true)
    
So konnte ich auf einen Blick die aggregierten Sicherheitsereignisse über die Zeit visualisieren.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/11.PNG?raw=true)


## Schritt 5: Eigene Regeln zur Erkennung von Ereignissen konfigurieren

Elastic SIEM bietet viele vorgefertigte Regeln, aber ich habe eine eigene Regel erstellt, um gezielt Nmap-Scans zu erkennen:

1. Im **Elastic SIEM Dashboard** bin ich unter dem Reiter **Security** zur Sektion **Rules** gegangen.
    
2. Dort habe ich auf **„Create new rule“** geklickt, um eine neue Regel anzulegen.
    
3. Die meisten Einstellungen habe ich auf Standard gelassen und nur bei **Custom Query** folgenden Ausdruck eingetragen:
    
	    process.args: "nmap"

![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/12.PNG?raw=true)

4. Ich habe der Regel einen Namen und eine Beschreibung gegeben und den Schweregrad passend gesetzt.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/13.PNG?raw=true)
    
5. Den Zeitplan habe ich vorerst auf den Standardwerten belassen.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/14.PNG?raw=true)
    
6. Für die Aktion habe ich **E-Mail-Benachrichtigungen** ausgewählt, damit ich im Falle eines Nmap-Scans sofort informiert werde.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/15.PNG?raw=true)
    

Dabei habe ich darauf geachtet, dass die E-Mail-Einstellungen korrekt konfiguriert sind, damit die Alerts zuverlässig ankommen.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/16.PNG?raw=true)


## Schritt 6: Alerts für eigene Regeln generieren

Um sicherzugehen, dass meine angelegte Regel funktioniert, habe ich folgende Tests durchgeführt:

1. **Nmap-Scan ausführen**  
    Im Terminal meiner Kali-VM habe ich erneut folgenden Befehl ausgeführt:
    
```bash
    sudo nmap -A -sV localhost
```
    
2. **Alerts im SIEM prüfen**  
    Im Elastic SIEM Dashboard bin ich in den Bereich **„Alerts“** gewechselt und habe kontrolliert, ob durch den Nmap-Scan neue Warnungen generiert wurden.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/17.PNG?raw=true)
    
3. **E-Mail-Benachrichtigungen überprüfen**  
    Zusätzlich habe ich meinen E-Mail-Posteingang geprüft, um sicherzustellen, dass ich eine Benachrichtigung mit relevanten Informationen zum Scan erhalten habe.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/18.PNG?raw=true)
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/19.PNG?raw=true)

Mit diesen Schritten konnte ich bestätigen, dass meine eigene Regel korrekt arbeitet und Alerts zuverlässig ausgelöst werden.


## Schritt 7: Vorgefertigte Regeln nutzen und Alerts generieren

Um die Wirksamkeit vordefinierter Regeln zu testen, habe ich die Regel **„Linux User Added to Privileged Group“** aktiviert. Diese erkennt, wenn ein Benutzer auf einem Linux-System in eine privilegierte Gruppe aufgenommen wird – ein wichtiger Indikator für mögliche Rechteausweitung.

1. **Regel aktivieren**  
    Im SIEM-Dashboard habe ich die Regel per **Toggle-Schalter** aktiviert.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/20.PNG?raw=true)
    
2. **Neuen Benutzer anlegen und Berechtigungen erhöhen**  
    In der Kali-VM habe ich im Terminal folgende Befehle ausgeführt:
    
```bash
    sudo adduser testuser sudo usermod -aG sudo testuser
```
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/21.PNG?raw=true)

3. **Alert prüfen**  
    Anschließend habe ich im SIEM-Dashboard unter dem Reiter **„Alerts“** kontrolliert, ob der Vorfall erkannt und entsprechend gemeldet wurde.
![Nmap Scan](https://github.com/DerFunkerr/kleines-siem-home-lab-mit-elastic-und-kali-vm/blob/main/images/22.PNG?raw=true)

So konnte ich verifizieren, dass auch vordefinierte Regeln zuverlässig funktionieren und privilegierte Aktionen sichtbar machen.


## Fazit

Durch den Aufbau und die Nutzung meines eigenen SIEM-Labors mit **Elastic Cloud**, **Kali Linux** und dem **ELK Stack** konnte ich praxisnah nachvollziehen, wie sicherheitsrelevante Ereignisse erfasst, analysiert und bewertet werden.

Ich habe gelernt, wie man:

- den Elastic Agent einrichtet und mit Elastic Cloud verbindet,
    
- gezielt Ereignisse wie Nmap-Scans und Exploits erzeugt,
    
- eigene sowie vordefinierte Regeln im SIEM konfiguriert,
    
- Alarme auslöst und auswertet,
    
- und Maßnahmen zur Incident Response vorbereitet.
    

Diese Hands-on-Erfahrung hat mir nicht nur ein besseres technisches Verständnis vermittelt, sondern auch gezeigt, wie wichtig strukturierte Überwachung und schnelle Reaktion in realen Sicherheitsumgebungen sind.
