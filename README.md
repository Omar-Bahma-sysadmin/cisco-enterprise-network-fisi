# 🌐 Unternehmensnetzwerk-Architektur (FISI Portfolio)

Dieses Projekt demonstriert die Planung, Segmentierung und Konfiguration einer strukturierten Netzwerk-Infrastruktur für ein kleines bis mittleres Unternehmen (KMU) mittels **Cisco Packet Tracer**.

---

## 📐 Netzwerk-Topologie & Segmentierung (VLANs)

Das Netzwerk ist logisch in verschiedene VLANs unterteilt, um Sicherheit, Broadcast-Kontrolle und Performance zu optimieren.

| VLAN ID | Name | Subnetz | Gateway | Beschreibung |
|---|---|---|---|---|
| **VLAN 10** | Verwaltung | `192.168.10.0/24` | `192.168.10.1` | Arbeitsplätze für Verwaltung & Buchhaltung |
| **VLAN 20** | IT-Abteilung | `192.168.20.0/24` | `192.168.20.1` | IT-Administration & Entwicklung |
| **VLAN 30** | Produktion | `192.168.30.0/24` | `192.168.30.1` | Produktion & Logistik |
| **VLAN 99** | DMZ / Server | `192.168.99.0/24` | `192.168.99.1` | Zentrale Dienste (Web/DNS) |

---

## 🛠️ Implementierte Technologien & Funktionen

- **IEEE 802.1Q VLAN Trunking:** Logische Trennung der Abteilungen über einen zentralen Switch (`Switch-Main`).
- **Inter-VLAN Routing (Router-on-a-Stick):** Konfiguration von Sub-Interfaces auf dem Cisco-Router (`Router-Core`).
- **Zentrale Dienste (DNS & Webserver):** Interner Webserver erreichbar unter `www.firma-netzwerk.de` (`192.168.99.10`).
- **DHCP-Konfiguration:** Automatische IP-Vergabe für die Client-Netzwerke.

---

## ⚙️ Konfigurationsauszug (Cisco CLI)

### Switch Configuration (`Switch-Main`):
```cisconetwork
enable
configure terminal
hostname Switch-Main

! VLANs erstellen
vlan 10
 name Verwaltung
vlan 20
 name IT-Abteilung
vlan 30
 name Produktion
vlan 99
 name Servers
exit

! Access Ports
interface FastEthernet0/10
 switchport mode access
 switchport access vlan 10
 exit

interface FastEthernet0/4
 switchport mode access
 switchport access vlan 99
 exit

! Trunk Port zum Router
interface GigabitEthernet0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30,99
 exit
```

### Router Configuration (`Router-Core`):
```cisconetwork
enable
configure terminal
hostname Router-Core

interface GigabitEthernet0/0
 no shutdown
 exit

! Inter-VLAN Sub-Interfaces
interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
 exit

interface GigabitEthernet0/0.99
 encapsulation dot1Q 99
 ip address 192.168.99.1 255.255.255.0
 exit

! DHCP Pool
ip dhcp pool POOL_VLAN10
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.99.10
 exit
```

---

## 📁 Dateistruktur

- `enterprise_network.pkt` - Cisco Packet Tracer Projektdatei.
- `topology.png` - Netzwerkdiagramm / Screenshot.
- `README.md` - Projektdokumentation.

---
**Ziel:** Portfolio für Bewerbungen zur Ausbildung als *Fachinformatiker für Systemintegration (FISI)*.
