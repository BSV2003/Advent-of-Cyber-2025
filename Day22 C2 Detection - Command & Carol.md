# C2 Detection - Command & Carol
Explore how to analyze a large PCAP and extract valuable information.

## Learning Objectives
- Convert a PCAP to Zeek logs
- Use RITA (Real Intelligence Threat Analytics) to analyze Zeek logs
- Analyze the output of RITA

**PCAP:** Packet capture (PCAP) is a networking practice involving the interception of data packets travelling over a network. Once the packets are captured, they can be stored by IT teams for further analysis. The inspection of these packets allows IT teams to identify issues and solve network problems affecting daily operations.

**Zeek:** Zeek (formerly Bro) is the world's leading platform for network security monitoring. Flexible, open source, and powered by defenders.

---

## 🔹 **Zeek**

- Open-source **Network Security Monitoring (NSM)** tool
- Converts raw network traffic into structured logs
- Passive analysis (no blocking, no signatures)

## 🔹 **RITA (Real Intelligence Threat Analytics)**

- Open-source framework by **Active Countermeasures**
- Designed to detect **C2 communication patterns**
- Works only with **Zeek logs**

**NSM:** Network Security Monitoring is based upon the collection of data to perform detection and analysis. With the collection of a large amount of data, it makes sense that a SOC should have the ability to generate statistical data from existing data, and that these statistics can be used for detection and analysis.

### What RITA Detects

- C2 beaconing (periodic connections)
- DNS tunneling
- Long-lived connections
- Data exfiltration
- Rare TLS/HTTP signatures
- New or low-prevalence external hosts
- Known malicious IPs/domains (Threat Intel)

### 🔬 Detection Logic Used by RITA

RITA correlates:
- IP addresses
- Ports
- Timestamps
- Connection duration
- Data volume

It then analyzes:
- Periodic connection intervals
- Excessive DNS queries
- Long or random FQDNs
- Unusual TLS certificates
- Non-standard ports
- Outbound data volume

#### 🔁 Workflow

> PCAP → Zeek Logs → RITA Import → RITA Analysis → Threat Hunting

### 📂 Step 1: Convert PCAP to Zeek Logs

**Directory Structure**
``` bash
ls
pcaps/
zeek_logs/
```

**Convert PCAP**
```bash
zeek readpcap pcaps/AsyncRAT.pcap zeek_logs/asyncrat
```

**Example Zeek Logs Generated**
- conn.log
- dns.log
- http.log
- ssl.log
- files.log
- x509.log

### 📥 Step 2: Import Zeek Logs into RITA

```bash
rita import --logs ~/zeek_logs/asyncrat/ --database asyncrat
```

RITA:
- Parses logs
- Normalizes data
- Correlates connections
- Enriches with threat intelligence

### 📊 Step 3: View RITA Results

```bash
rita view asyncrat
```

**RITA Interface Sections
- Search Bar
- Results Pane
- Details Pane**

## ⚠️ Important Threat Modifiers

| Modifier            | Description                                     |
| ------------------- | ----------------------------------------------- |
| MIME mismatch       | Content type doesn’t match URI                  |
| Rare signature      | Uncommon TLS/User-Agent patterns                |
| Prevalence          | Number of internal hosts talking to destination |
| First Seen          | Newly observed external host                    |
| Missing host header | Suspicious HTTP traffic                         |
| Long connection     | Possible beaconing                              |
| Large outbound data | Possible exfiltration                           |

#### 🚩 Indicators Observed

- Long FQDNs (e.g. trycloudflare.com)
- Rare TLS fingerprints
- Non-standard ports
- Long-lived connections
- IPs/domains flagged by VirusTotal

#### 🧠 Why Metadata Matters

- C2 traffic is often **encrypted**
- Payload inspection is ineffective
- **Behavioral patterns** expose malicious activity
