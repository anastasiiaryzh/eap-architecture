# System Context Diagram

**C4 Level 1** — EAP as a black box, showing who uses it and which external systems it depends on.

```mermaid
flowchart TB
    subgraph Users["👥 Users"]
        R["👤 Requester\n──────────\nEmployee who submits\nrequests for hardware,\nsoftware, or services"]
        A["👤 Approver\n──────────\nManager / Team Lead\nwho reviews and decides\non requests"]
        AD["👤 Admin\n──────────\nSystem Administrator\nwho fulfills requests\nand manages the system"]
    end

    EAP["🖥️ EAP\n────────────────────────\nEnterprise Application Project\nWeb-based employee request\nmanagement system"]

    EMAIL["📧 Email Service\n──────────────\nSMTP — sends workflow\nnotifications to users"]

    R  -- "Submit & track\nown requests" --> EAP
    A  -- "Review, approve\nor reject requests" --> EAP
    AD -- "Fulfill requests,\nmanage users & config" --> EAP
    EAP -- "Notification emails\non every status change" --> EMAIL
```

## Users

| Actor | Role | Key Actions |
|---|---|---|
| Requester | Employee | Submit, edit, cancel, track own requests |
| Approver | Manager / Team Lead | Review requests, approve or reject with comments |
| Admin | System Administrator | Fulfill approved requests, manage users, configure request types, generate reports |

## External Systems

| System | Purpose |
|---|---|
| Email Service (SMTP) | Notifies users on every workflow event (submission, approval, rejection, completion, cancellation) |