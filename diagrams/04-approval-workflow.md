# Approval Workflow — Sequence Diagram

End-to-end flow of a request from submission to completion, including the rejection and cancellation paths.

```mermaid
sequenceDiagram
    actor Requester
    participant System as EAP System
    actor Approver
    actor Admin
    participant Email as Email Service

    %% ── Submission ──────────────────────────────────────────
    Requester ->> System: Submit request
    System -->> Email: Send confirmation
    Email -->> Requester: ✉ Submission confirmed (request ID + summary)

    System ->> System: Auto-route to designated\nApprover for request type
    System -->> Email: Notify Approver
    Email -->> Approver: ✉ New request requires your review

    %% ── Happy path: Approve → Fulfil ────────────────────────
    alt Approved by Approver
        Approver ->> System: Approve (optional comment)
        System -->> Email: Notify Requester
        Email -->> Requester: ✉ Request approved

        System -->> Email: Notify Admin
        Email -->> Admin: ✉ Request ready to fulfil

        Admin ->> System: Take request → In Progress
        System -->> Email: Notify Requester
        Email -->> Requester: ✉ Request is in progress

        Admin ->> System: Mark Completed
        System -->> Email: Notify Requester
        Email -->> Requester: ✉ Request completed

    %% ── Rejected by Approver ─────────────────────────────────
    else Rejected by Approver
        Approver ->> System: Reject (mandatory reason)
        System -->> Email: Notify Requester
        Email -->> Requester: ✉ Request rejected + reason

    %% ── Rejected by Admin ────────────────────────────────────
    else Rejected by Admin
        Admin ->> System: Reject approved request
        System -->> Email: Notify Requester
        Email -->> Requester: ✉ Request rejected by Admin

    %% ── Cancelled by Requester ───────────────────────────────
    else Cancelled by Requester (before Approved)
        Requester ->> System: Cancel request
        System -->> Email: Notify Approver
        Email -->> Approver: ✉ Request was cancelled
    end
```

## Notification Matrix

| Event | Recipient | Email Content |
|---|---|---|
| Request submitted | Requester | Confirmation with request ID and summary |
| Request auto-assigned | Approver | New request requiring review |
| Request approved | Requester | Approval confirmation |
| Request ready to fulfil | Admin | Approved request in backlog |
| Request in progress | Requester | Status update |
| Request completed | Requester | Fulfilment confirmation |
| Request rejected (Approver or Admin) | Requester | Rejection notification with reason |
| Request cancelled | Approver | Cancellation notification |
| Comment added | All relevant parties | New comment notification |

> All emails are sent within 1 minute of the triggering event and include the request ID and a direct link.