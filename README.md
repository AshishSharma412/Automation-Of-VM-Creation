# Automation of VM Creation

End-to-end automated VM provisioning platform for **Hyper-V**. Users submit a VM request through a ticketing workflow, which triggers a **Jenkins** pipeline that scans for an available IP, routes the request through an approval step, provisions the VM with **Ansible** playbooks, and emails the requester with build status and VM details.

---

## What it does

- Accepts VM requests (name, OS, cores, RAM, region, purpose)
- Scans private subnets to find an available IP (`ScanIP/`)
- Pauses for approval (with optional manual IP override)
- Provisions Windows or Ubuntu VMs on Hyper-V via Ansible
- Assigns IP and sends email notification on success or failure

---

## Tech Stack

| Tool | Role |
|------|------|
| Jenkins | Pipeline orchestration, input parameters, approval gate |
| Ansible | VM creation, IP assignment, configuration playbooks |
| Hyper-V | Virtualization platform |
| Python (Flask) | Local approval API (`HelperFiles/approve.py`) |
| HashiCorp Vault | Secure Ansible vault password retrieval |

---

## Project Structure

```
├── Jenkinsfile                  # Main CI/CD pipeline
├── CreateVM.yml                 # Hyper-V VM creation playbook
├── AssignIP_ubuntu.yml          # Ubuntu IP configuration
├── AssignIP_windows.yml         # Windows IP configuration
├── FetchIP.yml                  # Dynamic IP fetch for new VMs
├── vm_create_config.yml         # VM creation config
├── AnsibleRoles/                # CPU/RAM usage checks
├── HelperFiles/                 # Templates, approval API, scripts
├── InventoryFiles/              # Ansible inventory (incl. vault-encrypted config)
├── ScanIP/                      # IP range scanning scripts
└── Project_Readme.docx          # Full documentation (see below)
```

---

## Supported OS

- Windows
- Ubuntu

Supports multiple regions with separate subnets (e.g. INDIA, US).

---

## Full Documentation

For the complete workflow, setup assumptions, Jenkins stage breakdown, and implementation notes, see:

**[Project_Readme.docx](Project_Readme.docx)**

The docx covers:
- Prerequisites (Hyper-V host, Jenkins, Ansible)
- Jenkins pipeline stages (input → IP scan → approval → VM create → notify)
- Ansible vault and inventory setup
- Known improvements and errors encountered during development

---

## Pipeline Stages

1. **Input Parameters** — VM name, region, OS, cores, RAM, email
2. **Scan for IP** — Find first available IP in region subnet
3. **Request Approval** — Approve/reject via Jenkins (email + one-click API)
4. **Create VM** — Ansible playbooks provision VM on Hyper-V
5. **Post-Build** — Email user with VM details or error log

---

## Author

**Ashish Sharma**
