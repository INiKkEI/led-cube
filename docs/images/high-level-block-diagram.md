

```mermaid
flowchart LR
    PSU[USB-C 5 V Input]
    PROT[Protection / Filtering<br/>bulk capacitance, input protection, decoupling]
    ESP[ESP32<br/>refresh timing, animation engine,<br/>serial debug, button handling]
    BTN[User Button<br/>animation select]
    DRV[Serial Driver Chain<br/>data, clock, latch, OE<br/>64 column outputs + 8 layer-control bits]
    MOS[Layer MOSFETs<br/>low-side switching]
    CUBE[8×8×8 Monochrome LED Cube<br/>64 column lines<br/>8 common-cathode layers]

    PSU --> PROT
    PROT -->|5 V rail| ESP
    PROT -->|5 V rail| DRV
    PROT -->|5 V rail| CUBE

    BTN -->|GPIO input| ESP

    ESP -->|data / clock / latch / OE| DRV
    DRV -->|64 column signals| CUBE
    DRV -->|layer control signals| MOS
    MOS -->|active layer to GND| CUBE