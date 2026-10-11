# Software-Defined Vehicle (SDV) Telemetry & Safety HMI

An industry-grade, real-time Web Human-Machine Interface (HMI) and telemetry dashboard developed as a part of the **Eclipse SDV 2026 Hackathon**. This project bridges low-level embedded hardware execution with a modern web dashboard to deliver real-time vehicle telemetry, safety-critical state alerts, and child presence detection capabilities.

---

# System Architecture & Pipeline

The system is engineered around a low-latency pipeline connecting physical edge compute nodes to a browser-based cockpit dashboard:

```text
[ Onboard Sensors & Child Presence Node ]
                   │
                   ▼
[ ThreadX Standalone Microcontroller Board ]  (Azure RTOS / ThreadX Execution Environment)
                   │  (UART / Serial Stream / Custom Protocol)
                   ▼
[ Middleware Bridge / Node.js Process ]
                   │  (WebSockets / JSON Payload)
                   ▼
[ React.js HMI Web Dashboard ]  (Live Cockpit, Telemetry & Safety Alerts)
