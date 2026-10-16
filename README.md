# Software-Defined Vehicle (SDV) Telemetry & Safety HMI

An industry-grade, real-time Web Human-Machine Interface (HMI) and telemetry dashboard developed as a part of the **Eclipse SDV 2026 Hackathon**. This project bridges low-level embedded hardware execution with a modern web dashboard to deliver real-time vehicle telemetry, safety-critical state alerts, and child presence detection capabilities.

---

# System Architecture & Pipeline

The system architecture is structured with clear isolation between sensor services, central application logic, controller layers, and the web-based HMI dashboard:

```mermaid
flowchart TD
    subgraph Inputs["Sensor Services — Simulated or Physical"]
        Temperature["Cabin Temperature Sensor"]
        Presence["Child Presence Sensor"]
    end

    subgraph Central["Application Services — Laptop / Edge Target"]
        Guardian["Guardian Loop — Risk Evaluation"]
        Warning["Warning and Status Output"]
        Adapter["Actuation Adapter"]
    end

    subgraph Control["Controller and Actuator Layer & Target Board"]
        ThreadX["ThreadX RTOS Embedded Target"]
        CDA["OpenSOVD Classic Diagnostic Adapter"]
        Controller["Window / Fan / Alarm Controller"]
        Simulated["Simulated Actuator"]
        Physical["Physical Actuator"]
    end

    subgraph HMI["Web HMI Dashboard"]
        React["React.js Cockpit Dashboard<br/>(Real-time State & Alerts)"]
    end

    Temperature -->|"uProtocol: temperature events"| Guardian
    Presence -->|"uProtocol: presence events"| Guardian
    Guardian -->|"Current state and warnings"| Warning
    Guardian -->|"uProtocol RPC: action request"| Adapter
    Guardian -->|"Telemetry stream"| React
    Adapter -->|"SOVD REST request"| CDA
    CDA -->|"UDS over DoIP / Serial"| Controller
    Controller -->|"Virtual setup"| Simulated
    Controller -->|"Hardware setup"| Physical
    Adapter -.->|"Action result or failure"| Guardian
```



https://github.com/user-attachments/assets/841010f8-67a1-4103-8471-3d0cbcaba61d





