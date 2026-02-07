# 🛡️ Network-Wide Adblocker (Pi-hole)

### **Role:** Network Administration (Self-Hosted)

---

## 🚀 Overview
Standard ISP routers lack granular traffic control and expose networks to telemetry and intrusive advertising. This project implements a **network-wide DNS sinkhole** to reduce bandwidth usage, block telemetry, and enforce network-level security policies without requiring client-side software.

## 🛠️ Tech Stack

| Component | Technology |
| :--- | :--- |
| **Hardware** | ![Raspberry Pi](https://img.shields.io/badge/Raspberry%20Pi%20Zero%202W-C51A4A?style=flat-square&logo=Raspberry-Pi&logoColor=white) |
| **OS** | ![Linux](https://img.shields.io/badge/Linux%20(Pi%20OS%20Lite)-FCC624?style=flat-square&logo=linux&logoColor=black) |
| **Service** | ![Pi-hole](https://img.shields.io/badge/Pi--hole-96060C?style=flat-square&logo=pi-hole&logoColor=white) |
| **Storage** | 64GB MicroSD |
| **Protocols** | `DNS` • `DHCP` • `SSH` • `IPv4/IPv6` |

## ⚙️ How It Works
The Pi-hole acts as a gatekeeper for DNS requests. Instead of devices connecting directly to the open internet for ads, the traffic follows this logic:

1.  **Request:** Client device asks for `ads.google.com`.
2.  **Forward:** Router sends the query to the Raspberry Pi.
3.  **Sinkhole:** Pi-hole compares the request against a massive "Gravity" database.
4.  **Result:** * **Blocked:** The Pi returns `0.0.0.0` (the ad never loads).
    * **Allowed:** The Pi returns the real IP and the content loads.

## ✨ Key Features & Benefits
* **Privacy by Default:** Blocks tracking and telemetry at the source.
* **Network Performance:** Saves bandwidth by preventing ad-heavy assets from downloading.
* **Device Agnostic:** Protects everything from Smart TVs and IoT devices to mobile phones without installing individual apps.

```mermaid
graph TD
    A[Client Device] -->|1. Request: 'ads.google.com'| B(Router)
    B -->|2. Forward DNS Query| C{Pi-hole on Pi Zero 2 W}
    
    C -- Check Blocklist --> D{Is it blocked?}
    
    D -- YES --> E[Return 0.0.0.0]
    E -->|3. Blocked Response| B
    B -->|4. Ad fails to load| A
    
    D -- NO --> F[Forward to Upstream DNS e.g. 8.8.8.8]
    F -->|3. Get Real IP| G[Internet]
    G -->|4. Return Real IP| F
    F -->|5. Return Real IP| C
    C -->|6. Return Real IP| B
    B -->|7. Content Loads| A
```