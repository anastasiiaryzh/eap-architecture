# EAP API Specification

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 23-01-2026  
**Status:** Draft  
**Base URL:** `/api/v1`  
**Scope:** Sprints 2–3  

---

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Users](#users)
4. [Requests](#requests)
5. [Request Types](#request-types)
6. [Approvals](#approvals)
7. [Audit Logs](#audit-logs)
8. [Common Models](#common-models)
9. [Error Responses](#error-responses)

---

## Overview

### Conventions

- All endpoints prefixed with `/api/v1`
- Authentication via `Authorization: Bearer <access_token>` header
- Request/response bodies in JSON
- Dates in ISO 8601 format (UTC)
- Pagination via `page` and `limit` query params (default: page=1, limit=10)
- Paginated responses include `total`, `page`, `limit`, `pages` metadata

### HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | Success |
| 201 | Created |
| 204 | No Content (successful delete) |
| 400 | Validation error |
| 401 | Unauthorized (missing/invalid token) |
| 403 | Forbidden (insufficient permissions) |
| 404 | Not found |
| 409 | Conflict (duplicate resource) |
| 422 | Unprocessable entity |
| 500 | Internal server error |

---

## Authentication

### POST /auth/register

Register a new user account. Returns access token so user is logged in immediately.

**Auth:** None

**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass1!",
  "first_name": "John",
  "last_name": "Doe"
}
```

**Response:** `201 Created`
```json
{
  "access_token": "eyJhbG...",
  "token_type": "bearer",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "requester",
    "created_at": "2026-01-23T10:00:00Z"
  }
}
```

**Errors:**
- `400` — Invalid email format, weak password
- `409` — Email already registered

---

### POST /auth/login

Authenticate and receive access token.

**Auth:** None

**Request:**
```json
{
  "email": "user@example.com",
  "password": "SecurePass1!"
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJhbG...",
  "token_type": "bearer",
  "user": {
    "id": 1,
    "email": "user@example.com",
    "first_name": "John",
    "last_name": "Doe",
    "role": "requester"
  }
}
```

**Errors:**
- `401` — Invalid credentials

---

### POST /auth/change-password

Change the current user's password.

**Auth:** Bearer token

**Request:**
```json
{
  "current_password": "OldPass1!",
  "new_password": "NewPass1!"
}
```

**Response:** `200 OK`
```json
{
  "message": "Password changed successfully"
}
```

**Errors:**
- `400` — Weak new password
- `401` — Current password incorrect

---

## Users

### GET /users/me

Get current user's profile.

**Auth:** Bearer token
**Permission:** Any authenticated user

**Response:** `200 OK`
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "role": "requester",
  "is_active": true,
  "created_at": "2026-01-23T10:00:00Z",
  "updated_at": "2026-01-23T10:00:00Z"
}
```

---

### PUT /users/me

Update current user's profile.

**Auth:** Bearer token
**Permission:** Any authenticated user

**Request:**
```json
{
  "first_name": "Jonathan",
  "last_name": "Doe"
}
```

**Response:** `200 OK` — Updated user object

---

## Requests

### POST /requests/draft

Create a new request (saved as draft).

**Auth:** Bearer token
**Permission:** Requester

**Request:**
```json
{
  "title": "New laptop for development",
  "description": "Need a laptop with 32GB RAM for development work",
  "business_justification": "Current machine is 5 years old and cannot run required tools",
  "priority": "medium",
  "request_type_id": 1,
  "request_subtype_id": 2
}
```

**Response:** `201 Created`
```json
{
  "id": 42,
  "title": "New laptop for development",
  "description": "Need a laptop with 32GB RAM for development work",
  "business_justification": "Current machine is 5 years old and cannot run required tools",
  "priority": "medium",
  "status": "draft",
  "request_type": {
    "id": 1,
    "name": "Hardware Request"
  },
  "request_subtype": {
    "id": 2,
    "name": "Desktop"
  },
  "requester": {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe"
  },
  "approver": null,
  "created_at": "2026-01-23T10:00:00Z",
  "updated_at": "2026-01-23T10:00:00Z",
  "submitted_at": null
}
```

---

### POST /requests/submit

Create a new request (saved as submitted).

**Auth:** Bearer token
**Permission:** Requester

**Request:**
```json
{
  "title": "New laptop for development",
  "description": "Need a laptop with 32GB RAM for development work",
  "business_justification": "Current machine is 5 years old and cannot run required tools",
  "priority": "medium",
  "request_type_id": 1,
  "request_subtype_id": 2
}
```

**Response:** `201 Created`
```json
{
  "id": 42,
  "title": "New laptop for development",
  "description": "Need a laptop with 32GB RAM for development work",
  "business_justification": "Current machine is 5 years old and cannot run required tools",
  "priority": "medium",
  "status": "submitted",
  "request_type": {
    "id": 1,
    "name": "Hardware Request"
  },
  "request_subtype": {
    "id": 2,
    "name": "Desktop"
  },
  "requester": {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe"
  },
  "approver": {
    "id": 2,
    "first_name": "Saskia",
    "last_name": "Jansen"
  },
  "created_at": "2026-01-23T10:00:00Z",
  "updated_at": "2026-01-23T10:00:00Z",
  "submitted_at": "2026-01-23T10:00:00Z"
}
```

---

### GET /requests

List requests for the current user.

**Auth:** Bearer token
**Permission:**
- Requester/Approver/Admin: own created requests
- Admin: all requests

**Query params:**
```
?page=1
&limit=10
&status=draft,submitted    (comma-separated)
&priority=high,urgent
&request_type_id=1
&search=laptop                           (searches id, title, description)
&sort_by=updated_at                      (updated_at | priority | created_at)
&sort_order=desc                         (asc | desc)
```

**Response:** `200 OK`
```json
{
  "data": [
    {
      "id": 42,
      "title": "New laptop for development",
      "priority": "medium",
      "status": "draft",
      "request_type": {
        "id": 1,
        "name": "Hardware Request"
      },
      "request_subtype": {
        "id": 2,
        "name": "Desktop"
      },
      "requester": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe"
      },
      "approver": {
        "id": 2,
        "first_name": "Saskia",
        "last_name": "Jansen"
      },
      "updated_at": "2026-01-23T10:00:00Z",
      "created_at": "2026-01-23T10:00:00Z",
      "submitted_at": "2026-01-23T10:00:00Z"
    }
  ],
  "total": 15,
  "page": 1,
  "limit": 10,
  "pages": 2,
  "status_counts": {
    "draft": 3,
    "submitted": 5,
    "approved": 1,
    "in_progress": 1,
    "rejected": 2,
    "completed": 1,
    "cancelled": 0
  }
}
```

---

### GET /requests/{id}

Get full request details.

**Auth:** Bearer token
**Permission:**
- Requester: own requests only
- Approver: own + assigned requests
- Admin: all requests

**Response:** `200 OK`
```json
{
  "id": 42,
  "title": "New laptop for development",
  "description": "Need a laptop with 32GB RAM for development work",
  "business_justification": "Current machine is 5 years old and cannot run required tools",
  "priority": "medium",
  "status": "approved",
  "request_type": {
    "id": 1,
    "name": "Hardware Request"
  },
  "request_subtype": {
    "id": 2,
    "name": "Desktop"
  },
  "requester": {
    "id": 1,
    "first_name": "John",
    "last_name": "Doe",
    "email": "john@example.com"
  },
  "approver": {
    "id": 5,
    "first_name": "Alice",
    "last_name": "Manager"
  },
  "created_at": "2026-01-23T10:00:00Z",
  "updated_at": "2026-01-23T12:00:00Z",
  "submitted_at": "2026-01-23T11:00:00Z",
  "approved_at": null,
  "rejected_at": null,
  "completed_at": null,
  "cancelled_at": null,
  "status_history": [
    {
      "previous_status": null,
      "new_status": "draft",
      "changed_by": { "id": 1, "first_name": "John", "last_name": "Doe" },
      "comment": null,
      "created_at": "2026-01-23T10:00:00Z"
    },
    {
      "previous_status": "draft",
      "new_status": "submitted",
      "changed_by": { "id": 1, "first_name": "John", "last_name": "Doe" },
      "comment": null,
      "created_at": "2026-01-23T11:00:00Z"
    }
  ]
}
```

**Errors:**
- `403` — Not authorized to view this request
- `404` — Request not found

---

### PUT /requests/{id}

Edit a draft request.

**Auth:** Bearer token
**Permission:** Owner only, status must be `draft`

**Request:**
```json
{
  "title": "Updated title",
  "description": "Updated description",
  "business_justification": "Updated justification",
  "priority": "high",
  "request_type_id": 2,
  "request_subtype_id": 7
}
```

**Response:** `200 OK` — Full updated request object

**Errors:**
- `403` — Not the owner or request is not in draft status
- `404` — Request not found

---

### PATCH /requests/{id}/status

Change request status.

**Auth:** Bearer token
**Permission:** Depends on transition (see state machine below)

**Request:**
```json
{
  "status": "submitted"
}
```

For rejection (comment required, min 20 chars):
```json
{
  "status": "rejected",
  "comment": "Budget not available for this quarter. Please resubmit next quarter."
}
```

**Allowed transitions:**

| From | To | Who | Comment required |
|------|----|-----|-----------------|
| draft | submitted | Requester (owner) | No |
| submitted | cancelled | Requester (owner) | No |
| submitted | approved | Approver (assigned) | No |
| submitted | rejected | Approver (assigned) | Yes (min 20 chars) |
| approved | in_progress | Admin | No |
| approved | rejected | Admin | Yes (min 20 chars) |
| in_progress | completed | Admin | No |

**Response:** `200 OK` — Full updated request object

**Side effects:**
- `draft → submitted`: Auto-assigns approver based on request type, sends email notification to approver
- `* → cancelled`: Sends email notification to approver (if assigned)

**Errors:**
- `400` — Invalid status transition or missing required comment
- `403` — Not authorized for this transition
- `404` — Request not found

---

### DELETE /requests/{id}

Delete a draft request (hard delete).

**Auth:** Bearer token
**Permission:** Owner only, status must be `draft`

**Response:** `204 No Content`

**Errors:**
- `403` — Not the owner or request is not in draft status
- `404` — Request not found

---

## Request Types

### GET /request-types

List active request types with their subtypes. Used to populate the request creation form.

**Auth:** Bearer token
**Permission:** Any authenticated user

**Response:** `200 OK`
```json
{
  "data": [
    {
      "id": 1,
      "name": "Hardware Request",
      "description": "Requests for physical equipment and devices",
      "subtypes": [
        { "id": 1, "name": "Laptop" },
        { "id": 2, "name": "Desktop" },
        { "id": 3, "name": "Monitor" },
        { "id": 4, "name": "Peripherals" },
        { "id": 5, "name": "Mobile device" },
        { "id": 6, "name": "Other" }
      ]
    },
    {
      "id": 2,
      "name": "Software & Access",
      "description": "Software licenses and system access requests",
      "subtypes": [
        { "id": 7, "name": "Software license" },
        { "id": 8, "name": "System access" },
        { "id": 9, "name": "Application access" },
        { "id": 10, "name": "VPN access" },
        { "id": 11, "name": "Other" }
      ]
    },
    {
      "id": 3,
      "name": "Services & Facilities",
      "description": "Office services, training, and facilities requests",
      "subtypes": [
        { "id": 12, "name": "Parking spot" },
        { "id": 13, "name": "Office equipment" },
        { "id": 14, "name": "Training/course enrollment" },
        { "id": 15, "name": "Travel approval" },
        { "id": 16, "name": "Other" }
      ]
    }
  ]
}
```

---

## Approvals

### GET /approvals

List requests assigned to the current approver.

**Auth:** Bearer token
**Permission:** Approver only

**Query params:**
```
?page=1
&limit=10
&status=submitted     (comma-separated)
&priority=high,urgent
&search=laptop
&sort_by=updated_at                 (updated_at | priority)
&sort_order=asc                     (oldest first by default)
```

**Response:** `200 OK`
```json
{
  "data": [
    {
      "id": 42,
      "title": "New laptop for development",
      "priority": "medium",
      "status": "submitted",
      "request_type": {
        "id": 1,
        "name": "Hardware Request"
      },
      "request_subtype": {
        "id": 2,
        "name": "Desktop"
      },
      "requester": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe"
      },
      "updated_at": "2026-01-23T10:00:00Z",
      "created_at": "2026-01-23T10:00:00Z"
    }
  ],
  "total": 8,
  "page": 1,
  "limit": 10,
  "pages": 1,
  "status_counts": {
    "submitted": 5,
    "approved": 12,
    "rejected": 4
  }
}
```

---

### GET /approvals/history

List previously actioned requests by the current approver.

**Auth:** Bearer token
**Permission:** Approver only

**Query params:**
```
?page=1
&limit=10
&status=approved,rejected
&request_type_id=1
&date_from=2026-01-01
&date_to=2026-01-31
```

**Response:** `200 OK` — Same structure as GET /approvals

---

## Audit Logs

### GET /audit-logs

Search audit logs.

**Auth:** Bearer token
**Permission:** Admin only

**Query params:**
```
?page=1
&limit=10
&user_id=1
&action_type=REQUEST_CREATED
&entity_type=REQUEST
&entity_id=42
&date_from=2026-01-01T00:00:00Z
&date_to=2026-01-31T23:59:59Z
&sort_order=desc
```

**Response:** `200 OK`
```json
{
  "data": [
    {
      "id": 100,
      "user": {
        "id": 1,
        "first_name": "John",
        "last_name": "Doe",
        "email": "john@example.com"
      },
      "action_type": "REQUEST_CREATED",
      "entity_type": "REQUEST",
      "entity_id": 42,
      "new_value": { "title": "New laptop", "status": "draft" },
      "ip_address": "192.168.1.1",
      "created_at": "2026-01-23T10:00:00Z"
    }
  ],
  "total": 150,
  "page": 1,
  "limit": 10,
  "pages": 15
}
```

---

## Common Models

### User (Summary)
```json
{
  "id": 1,
  "first_name": "John",
  "last_name": "Doe"
}
```

### Pagination Metadata
```json
{
  "total": 100,
  "page": 1,
  "limit": 10,
  "pages": 10
}
```

### Request Status Values
`draft` | `submitted` | `approved` | `in_progress` | `rejected` | `completed` | `cancelled`

### Priority Values
`low` | `medium` | `high` | `urgent`

### User Roles
`requester` | `approver` | `admin`

---

## Error Responses

All errors follow this format:

```json
{
  "detail": "Human-readable error message",
  "errors": [
    {
      "field": "email",
      "message": "Email already registered"
    }
  ]
}
```

For validation errors (400/422), the `errors` array lists each field issue.
For auth errors (401/403), only `detail` is present.
