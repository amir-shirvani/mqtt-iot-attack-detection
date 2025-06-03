# MQTT IoT Attack Detection using pfSense and Suricata

🔐 This project demonstrates how to simulate, detect, and mitigate MQTT-based attacks in an IoT lab environment using open-source tools.

## 🧪 Technologies Used

- **pfSense** – firewall, network segmentation, IDS/IPS integration  
- **Suricata** – custom detection rules for MQTT flood attacks  
- **Kali Linux** – simulating attacker behavior (DoS flooding, spoofing)  
- **Ubuntu** – hosting the Mosquitto MQTT broker  
- **VirtualBox** – isolated virtual lab environment

## 🕸️ Network Topology

Three VMs connected via pfSense:

- LAN: Ubuntu MQTT Broker (192.168.1.0/24)  
- OPT1: Kali Attacker (192.168.30.0/24)  
- WAN: Simulated external access

All traffic is routed and inspected via pfSense.

## 💥 Simulated Attacks

- MQTT flood using `mosquitto_pub`  
- Unauthorized publishing and topic hijacking

## 📜 Custom Suricata Rule Example

```snort
alert tcp any any -> any 1883 (msg:"MQTT Flood Detected";
flow:to_server,established; detection_filter:track by_src, count 10,
seconds 1; classtype:attempted-dos; sid:900001; rev:1;)
