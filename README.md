# network-wide-adblocker-pi-zero

## Role: Network Administration (Self Hosted)
## Tech Stack: Raspberry Pi Zero 2W, microSD card 64Gb, Linux (Raspberry Pi OS Lite), Pi-hole, DNS, DHCP, SSH

- Standard ISP routers lack granular traffic control and expose networks to telemetry/ads. Implemented a network-wide DNS sinkhole to reduce bandwidth usage, block telemetry, and enforce network-level security policies without client-side software

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