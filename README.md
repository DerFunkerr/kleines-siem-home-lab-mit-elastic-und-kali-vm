# 🔐 SIEM Home Lab mit ELK Stack & Elastic Cloud

Dieses Projekt dokumentiert den Aufbau eines eigenen SIEM-Labors mit Kali Linux, dem Elastic Stack (ELK) und Elastic Cloud. Ziel ist es, sicherheitsrelevante Ereignisse zu erzeugen, zu überwachen und darauf zu reagieren – praxisnah und realitätsnah.

## 📋 Inhalt

- Einrichtung von Kali Linux & Elastic Cloud
- Installation und Konfiguration des Elastic Agent
- Erzeugung und Erkennung von Bedrohungen (z. B. via Nmap & Metasploit)
- Erstellen von Dashboards in Kibana
- Konfiguration eigener und vorgefertigter Erkennungsregeln
- Reaktion auf Alarme & Incident Response

## 🚀 Schnellstart

1. **Kali Linux** als VM einrichten (z. B. via VirtualBox)
2. Elastic Cloud Deployment starten und Agent installieren
3. Test-Events erzeugen mit:
   ```bash
   sudo nmap -A -sV localhost
   ```
4. Individuelle Regeln konfigurieren (z. B. für `process.args: "nmap"`)
5. Alerts analysieren und ggf. Maßnahmen ableiten

## 📸 Screenshots

Bilder und visuelle Schritte findest du in der Datei [`docs/SIEM.md`](docs/SIEM.md). Screenshots sind im Ordner `images/` gespeichert.

> Hinweis: Die ursprüngliche Obsidian-Syntax `![[Bildname.png]]` wurde für GitHub durch reguläre Markdown-Bildsyntax ersetzt:
>
> ```markdown
> ![Beschreibung](../images/1.png)
> ```

## 🧠 Fazit

Dieses Lab war ein wertvoller Schritt, um tiefere Einblicke in SIEM-Prozesse und Sicherheitsüberwachung zu gewinnen. Es eignet sich hervorragend als Grundlage für praktisches Lernen im Bereich Security Operations.

## 📂 Verzeichnis

- `docs/SIEM.md` – Ausführliche Dokumentation aller Schritte
- `images/` – Screenshots (bitte hier hinzufügen)
