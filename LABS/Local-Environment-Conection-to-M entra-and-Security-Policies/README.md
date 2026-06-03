# Hybrid Cyber Threat Monitoring Lab: Active Directory & Microsoft Entra ID

## 📌 Project Overview
This project demonstrates the deployment, hardening, and integration of a hybrid identity infrastructure, bridging an on-premises enterprise network with cloud-based identity management. The environment simulates a realistic corporate structure designed to facilitate centralized access control, robust security policies, and future integration with SIEM monitoring tools.

## 🛠️ Technologies Used
* **Operating System:** Windows Server 2025 (Domain Controller)
* **Cloud Platform:** Microsoft Azure & Microsoft Entra ID
* **Hybrid Connectivity:** Microsoft Entra Connect (Password Hash Synchronization)
* **Automation & Management:** PowerShell, Group Policy Management (GPMC)

---

## 🚀 Deployment Steps

### Phase 1: On-Premises Infrastructure Setup (The Earth)
1. **Domain Controller Configuration:** Deployed a virtualized Windows Server 2025 instance, promoting it to a primary Domain Controller for the internal forest `cyberlab.local`.
2. **Organizational Structure:** Designed and populated the directory infrastructure using PowerShell scripts to create dedicated Organizational Units (OUs) reflecting corporate departments (**TI, Finanzas, Recursos Humanos, Ventas**).

<img width="1760" height="750" alt="Captura de pantalla 2026-06-03 103443" src="https://github.com/user-attachments/assets/6086e62c-f59b-408c-b8b8-1e68454236e6" />


### Phase 2: Security Hardening (Account Lockout Policies)
To mitigate credential-based threats such as brute-force and dictionary attacks, strict account lockout parameters were implemented via Group Policy Objects (GPOs):
* **Account Lockout Threshold:** 5 invalid logon attempts.
* **Account Lockout Duration:** 15 minutes.
* **Reset Account Lockout Counter After:** 15 minutes.

<img width="1712" height="680" alt="Captura de pantalla 2026-06-03 104419" src="https://github.com/user-attachments/assets/1a9d543a-9f9b-4d4c-ae13-61785cd7aeb6" />


### Phase 3: Hybrid Directory Synchronization (The Bridge)
1. **Microsoft Entra Connect:** Configured Password Hash Synchronization (PHS) to map identity management from the local directory directly to Microsoft Entra ID in the cloud.
2. **Identity Verification:** Successfully synchronized on-premises user objects (e.g., `ana.gomez`) to the cloud ecosystem, confirming the directory authority remains on-premises.

<img width="478" height="42" alt="image" src="https://github.com/user-attachments/assets/78546710-95dc-44c4-ba7b-1484c84a6a2f" />

<img width="1818" height="894" alt="Captura de pantalla 2026-06-03 180121" src="https://github.com/user-attachments/assets/1a7853b8-54bb-4d2d-9504-934a803ca2a4" />

<img width="1854" height="860" alt="Captura de pantalla 2026-06-03 180151" src="https://github.com/user-attachments/assets/e0579036-e7a3-4e16-b3c9-6917efd9b590" />

---

## 🛠️ Technical Challenges & Troubleshooting
During the initial hybrid synchronization cycle, the PowerShell command `Start-ADSyncSyncCycle -PolicyType Delta` failed with an exception indicating that the connector was blocked: `System.InvalidOperationException: Connector: [...] - AAD is busy`.

### Root Cause Analysis
The synchronization engine pipeline was locked or unresponsive from a previous overlapping sync task execution, preventing manual triggers from registering.

### Resolution
The synchronization service (`ADSync`) was forcefully restarted via an elevated PowerShell console to clear the processing queue and reset the operational state:

<img width="1701" height="899" alt="Captura de pantalla 2026-06-03 180251" src="https://github.com/user-attachments/assets/3a048069-4e0e-4b3c-b600-f3fe6aecbd1f" />
