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

# Tech Stack & Architecture Highlights

* **Frontend & HMI Dashboard:** Built using **React.js** and modern **CSS** to render real-time vehicle telemetry, status warnings, and safety-critical alerts in a low-latency cockpit view.
* **Messaging & Event Bus:** Utilizes **uProtocol** for decoupled, publish-subscribe event-driven communication between vehicle sensors (cabin temperature, child presence) and application services.
* **Diagnostics & Communication Adapters:** Integrates **OpenSOVD Classic Diagnostic Adapters** and **UDS over DoIP / Serial** to bridge high-level application logic with low-level controllers.
* **Embedded Real-Time OS:** Built with target board execution using **ThreadX RTOS** to manage safety-critical tasks, window actuation, ventilation, and alarm triggers.
* **System Integration:** Cleanly separates edge services, guardian risk-evaluation loops, and HMI presentation layers for robust fault tolerance and safety monitoring.

---

# Engineering & Development Process

1. **Safety-First System Modeling:** Designed the guardian loop architecture to continuously ingest sensor events via uProtocol, executing real-time risk evaluations for cabin environment safety.
2. **Hardware-Software Co-Design:** Bridged high-level web dashboards (React) with low-level hardware execution (ThreadX RTOS and virtualized actuators) to simulate real-world automotive environments.
3. **Modular Diagnostics Pipeline:** Implemented OpenSOVD and UDS standards to ensure standard diagnostic communication flows from edge applications down to physical/simulated actuators.
```



https://github.com/user-attachments/assets/841010f8-67a1-4103-8471-3d0cbcaba61d





