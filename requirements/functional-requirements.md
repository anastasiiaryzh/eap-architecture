# Functional Requirements (FR)

**Project:** Enterprise Application Project (EAP)  
**Version:** 1.0  
**Date:** 09-01-2026  
**Status:** Draft  
**Source:** EAP Product Vision v1.0  

---

## Table of Contents

1. [Introduction](#introduction)
2. [User Management](#user-management)
3. [Request Management](#request-management)
4. [Approval Workflow](#approval-workflow)
5. [Notifications](#notifications)
6. [Dashboard and Reporting](#dashboard-and-reporting)
7. [Audit Trail](#audit-trail)
8. [System Configuration](#system-configuration)

---

## Introduction

This document specifies the functional requirements for the Enterprise Application Project (EAP), a web-based request management system. Requirements are organized by functional area and prioritized for MVP delivery.

### Requirement Priority Levels
- **P0 (Critical):** Must-have for MVP
- **P1 (High):** Important for MVP
- **P2 (Medium):** Nice-to-have for MVP, likely post-MVP
- **P3 (Low):** Future enhancement

### Requirement Status
- **Draft:** Under review
- **Approved:** Ready for implementation
- **Implemented:** Completed
- **Deferred:** Postponed to future release

---

## User Management

### FR-UM-001: User Authentication
**Priority:** P0  
**Status:** Draft

**Description:** The system shall provide secure user authentication.

**Acceptance Criteria:**
- Users can register with email and password
- Users can log in with valid credentials
- Users can log out from any page
- Failed login attempts are logged
- Passwords must meet security requirements (min 8 chars, 1 uppercase, 1 number, 1 special char)
- JWT tokens are used for session management

---

### FR-UM-002: Role-Based Access Control
**Priority:** P0  
**Status:** Draft

**Description:** The system shall support three user roles with different permissions.

**User Roles:**
1. **Requester (Employee):**
   - Submit new requests
   - View own request history
   - Track status of own requests
   - Cancel own pending requests (before approval)
   - Add comments to own requests (P2)

2. **Approver (Manager/Team Lead):**
   - View requests requiring their approval
   - Take requests under review auto-assigned to Approver
   - Approve or reject requests
   - Add approval/reject comments
   - View request history

3. **Admin (System Administrator):**
   - View all requests in the system
   - Manage user accounts and roles
   - Configure request types
   - Mark requests as in progress/completed/rejected
   - Generate reports and analytics

**Acceptance Criteria:**
- Users can only access features permitted by their role
- Role changes take effect immediately
- Unauthorized access attempts are blocked and logged

---

### FR-UM-003: User Profile Management
**Priority:** P1  
**Status:** Draft

**Description:** Users shall be able to view and update their profile information.

**Acceptance Criteria:**
- Users can view their profile (name, email, role)
- Users can update their profile information (except role)
- Email changes require verification
- Profile changes are logged in audit trail
- Password changes require verification

---

### FR-UM-004: User Account Management (Admin)
**Priority:** P0  
**Status:** Draft

**Description:** Admins shall be able to manage user accounts.

**Acceptance Criteria:**
- Admins can create new user accounts
- Admins can assign/modify user roles
- Admins can deactivate users (soft delete)
- Admins can reactivate deactivated users
- Deactivated users cannot log in
- User management actions are logged

---

## Request Management

### FR-RM-001: Request Submission
**Priority:** P0  
**Status:** Draft

**Description:** Requesters shall be able to submit new requests through a web form.

**Request Fields:**
- Request Type (dropdown, required)
- Request Subtype (dropdown, required)
- Title (text, required, max 200 chars)
- Description (textarea, required, max 2000 chars)
- Business Justification (textarea, required, max 1000 chars)
- Priority (dropdown: Low, Medium, High, Urgent)
- Attachments (optional, max 5 files, max 10MB each)

**Acceptance Criteria:**
- All required fields must be filled before submission
- Request is assigned unique ID upon submission
- Request status is set to "Draft" on creation with explicit saving
- Request status changes to "Submitted" after submission
- Requester receives confirmation email
- Validation errors are displayed clearly

---

### FR-RM-002: Request Types
**Priority:** P0  
**Status:** Draft

**Description:** The system shall support multiple configurable request types.

**Initial Request Types (MVP):**
1. **Hardware Request**
   - Laptop
   - Desktop
   - Monitor
   - Peripherals
   - Mobile device
   - Other

2. **Software & Access**
   - Software license
   - System access
   - Application access
   - VPN access
   - Other

3. **Services & Facilities**
   - Parking spot
   - Office equipment
   - Training/course enrollment
   - Travel approval
   - Other

**Acceptance Criteria:**
- Each request type has specific subtype
- Request types can be added/modified by Admin (P2)
- Request types can be activated/deactivated by Admin

---

### FR-RM-003: Request Status Tracking
**Priority:** P0  
**Status:** Draft

**Description:** All requests shall have a clearly defined status throughout their lifecycle.

**Request States:**
| State | Description | Who Can Act |
|-------|-------------|-------------|
| Draft | Request being created (not sent to Approver) | Requester |
| Submitted | Awaiting review | Requester (change status to "Submitted") → System (auto-assign to one default approver attached to one specific type of request) |
| Under Review | Being reviewed (Approver starts review manually triggering this even) | Approver |
| Approved | Approved, awaiting fulfillment | Approver |
| In progress | Admin assigned request to themselves from general Admin dashboard backlog | Admin |
| Rejected | Denied with a reason | Approver, Admin |
| Completed | Fulfilled and closed | Admin |
| Cancelled | Requester cancelled (can be done by Requester before "Approved" status) | Requester |

**State Transitions:**
- Draft → Submitted (by Requester)
- Submitted → Under Review (by Approver)
- Submitted/Under Review → Cancelled (by Requester before approval)
- Under Review → Approved (by Approver)
- Under Review → Rejected (by Approver)
- Approved → In progress (by Admin)
- Approved → Rejected (by Admin)
- In progress → Completed (by Admin)

**Acceptance Criteria:**
- Each state change is timestamped
- Each state change records who made the change
- Status history is maintained
- Real-time status updates are visible to requester

---

### FR-RM-004: View Request Details
**Priority:** P0  
**Status:** Draft

**Description:** Users shall be able to view detailed information about requests.

**Request Details Include:**
- Request ID (integer)
- Request title
- Request type
- Request subtype
- Description
- Business justification
- Requester information
- Current status
- Status history timeline
- Approver information (if assigned)
- Approval/rejection comments
- Attachments
- Created date and last updated date
- All comments and activity

**Acceptance Criteria:**
- Requesters can view their own requests
- Approvers can view requests assigned to them
- Admins can view all requests
- Users cannot view requests outside their permission scope

---

### FR-RM-005: Request Dashboard (Requester)
**Priority:** P0  
**Status:** Draft

**Description:** Requesters shall have a dashboard showing all their requests.

**Dashboard Features:**
- List view of all own requests with quick information (id, title, type, status, last update date, priority, assignee)
- Status summary (count by status Submitted, Under review, Approved, Rejected, Completed, All)
- Filter by status (Draft, Submitted, Under review, Approved, Rejected, Completed, All)
- Filter by request type
- Sort by last update date, status, type
- Search by id, title or description
- Quick status indicators (color-coded badges)
- List of request is paginated

**Acceptance Criteria:**
- Dashboard loads within 2 seconds
- Default view shows newest last updated date requests
- Clicking request navigates to detail view
- Dashboard updates when status changes

---

### FR-RM-006: Request Cancellation
**Priority:** P1  
**Status:** Draft

**Description:** Requesters shall be able to cancel their own pending requests.

**Acceptance Criteria:**
- Requests can only be cancelled before approval
- Cancellation requires confirmation dialog
- Cancelled requests move to "Cancelled" status
- Approver is notified of cancellation
- Cancelled requests remain visible in history
- Can be cancelled in detail view

---

### FR-RM-007: Request Comments
**Priority:** P2   
**Status:** Draft

**Description:** Users shall be able to add comments to requests.

**Acceptance Criteria:**
- Requesters can add comments to their own requests
- Approvers can add comments when reviewing
- Admins can add comments to any request
- Comments are timestamped and attributed to user
- Comments are displayed in chronological order
- All parties receive email notification of new comments

---

### FR-RM-008: Request Attachments
**Priority:** P1  
**Status:** Draft

**Description:** Users shall be able to attach supporting documents to requests.

**Acceptance Criteria:**
- Up to 5 files per request
- Maximum 10MB per file
- Supported formats: PDF, DOC, DOCX, XLS, XLSX, PNG, JPG
- Attachments can be downloaded by authorized users
- Attachments are included in request details view

---

## Approval Workflow

### FR-AW-001: Approval Dashboard
**Priority:** P0  
**Status:** Draft

**Description:** Approvers shall have a dedicated dashboard showing requests requiring their approval.

**Features:**
- List of pending approval requests
- Count of pending approvals (badge)
- Request preview on Dashboard (title, subtype, status, date, priority, requester)
- Quick "under review" action
- Sort by date, priority
- Filter by request status (submitted, under review, approved, rejected)

**Acceptance Criteria:**
- Dashboard shows only requests assigned to logged-in approver
- Default sort is by last updated date (oldest first)
- Quick actions are accessible without opening detail view

---

### FR-AW-002: Request Approval
**Priority:** P0  
**Status:** Draft

**Description:** Approvers shall be able to approve requests.

**Acceptance Criteria:**
- Approver can approve from detail view
- Approval comments are optional
- Approval is logged with timestamp and approver ID
- Request status changes to "Approved"
- Requester receives email notification
- Admin receives notification for fulfillment

---

### FR-AW-003: Request Rejection
**Priority:** P0  
**Status:** Draft

**Description:** Approvers shall be able to reject requests with a mandatory reason.

**Acceptance Criteria:**
- Rejection reason is required (min 20 chars)
- Request status changes to "Rejected"
- Requester receives email notification with reason
- Rejected requests move out of approval queue
- Rejected requests cannot be resubmitted (requester must create new request)

---

### FR-AW-004: Approval Routing
**Priority:** P0  
**Status:** Draft

**Description:** Submitted requests shall be automatically routed to appropriate approvers.

**MVP Routing Logic:**
- All requests of a given type route to designated approver for that type
- Request from "Submitted" state auto assigned to designated approver
- Each request type has one default approvers

**Acceptance Criteria:**
- Request status changes from "Submitted" to "Under Review" by Approver
- Approver receives email notification
- Routing happens immediately upon submission

**Note:** Multi-level approval chains are out of scope for MVP.

---

### FR-AW-005: Approval History
**Priority:** P1  
**Status:** Draft

**Description:** Approvers shall be able to view their approval history.

**Acceptance Criteria:**
- Approvers can see all requests they've approved/rejected
- Filter by approval decision (Approved/Rejected)
- Filter by date range
- Filter by request type
- View statistics (approval rate, average time)
- Can be exported for reporting in formatted file

---

## Notifications

### FR-NT-001: Email Notifications
**Priority:** P0  
**Status:** Draft

**Description:** The system shall send email notifications for key workflow events.

**Notification Triggers:**
| Event | Recipient | Email Content |
|-------|-----------|---------------|
| Request submitted | Requester | Confirmation with request ID and summary |
| Request routed | Approver | New request requiring approval |
| Request under reviewing | Requester | Under review status |
| Request approved | Requester | Approval confirmation |
| Request rejected | Requester | Rejection notification with reason |
| Request completed | Requester | Fulfillment confirmation |
| Request cancelled | Approver | Cancellation notification |
| Comment added | Relevant parties | New comment notification |

**Acceptance Criteria:**
- Emails are sent within 1 minute of event
- Emails include request ID and direct link
- Emails are formatted professionally
- Failed email delivery is logged
- Users can opt-out of non-critical notifications (P2)

---

## Dashboard and Reporting

### FR-DR-001: Admin Dashboard
**Priority:** P0  
**Status:** Draft

**Description:** Admins shall have a system-wide dashboard with key metrics.

**Dashboard Widgets:**
1. System-wide statistics
   - Total requests (all time)
   - Active requests (not completed/rejected/cancelled)
   - Requests by status (chart)

2. Pending Approvals
   - Count of requests awaiting approval
   - List of oldest pending requests
   - Approval bottlenecks (approvers with most pending)

3. Request Volume Trends
   - Requests submitted per week/month (chart)
   - Requests by type (pie chart)

4. Recent Activity
   - Latest 10 request submissions
   - Latest 10 approvals/rejections

**Acceptance Criteria:**
- Dashboard loads within 2 seconds
- All data is current (real-time or max 5 min delay)
- Widgets can be refreshed individually

---

### FR-DR-002: Reporting and Analytics
**Priority:** P1  
**Status:** Draft

**Description:** Admins shall be able to generate various reports.

**Available Reports:**
1. **Request Summary Report**
   - Requests by type, status, time period
   - Customizable date range
   - Grouping options

2. **Approval Time Report**
   - Average/median approval time
   - By approver, request type, time period
   - Identify bottlenecks

3. **User Activity Report**
   - Requests per user
   - Approval activity per approver
   - Active users

4. **Audit Report**
   - Complete request history
   - All actions with timestamps and users
   - Filterable by request, user, action type

**Acceptance Criteria:**
- Reports can be previewed on screen
- Reports can be exported to formatted file
- Report generation completes within 10 seconds for standard date ranges

---

### FR-DR-003: Request Search
**Priority:** P1  
**Status:** Draft

**Description:** Users shall be able to search for requests.

**Search Capabilities:**
- Full-text search across title, description
- Filter by request type
- Filter by status
- Filter by requester (Admins only)
- Filter by approver (Admins only)
- Filter by date range
- Combined filters (AND logic)

**Acceptance Criteria:**
- Search results respect user permissions
- Search returns results within 2 seconds
- Results are paginated (10 per page)

---

## Audit Trail

### FR-AT-001: Complete Audit Logging
**Priority:** P0  
**Status:** Draft

**Description:** The system shall maintain a complete, immutable audit trail of all actions.

**Logged Actions:**
- User login/logout
- Request creation, submission, update
- Status changes
- Approvals and rejections
- Comments added
- Attachments uploaded/downloaded
- User account changes
- Role assignments
- System configuration changes

**Audit Record Fields:**
- Timestamp (UTC)
- User ID and username
- Action type
- Target entity (request ID, user ID, etc.)
- Before/after values (for updates)

**Acceptance Criteria:**
- All actions are logged automatically
- Audit records cannot be modified or deleted
- Audit logs are searchable by Admins
- Audit logs can be filtered by date, user, action type
- Audit logs can be exported for compliance

---

### FR-AT-002: Request History Timeline
**Priority:** P1  
**Status:** Draft

**Description:** Each request shall display a visual timeline of its history.

**Timeline Elements:**
- Created (with requester)
- Submitted (with timestamp)
- Routed to approver (with approver name)
- Comments added
- Status changes (with who and when)
- Approved/Rejected (with decision and comments)
- Completed (with timestamp)

**Acceptance Criteria:**
- Timeline is displayed on request detail page
- Timeline is in chronological order (newest first)
- Each event shows timestamp, actor, and action
- Timeline is visually clear and easy to follow

---

## System Configuration

### FR-SC-001: Request Type Configuration
**Priority:** P1  
**Status:** Draft

**Description:** Admins shall be able to configure request types.

**Configuration Options:**
- Add new request type
- Edit request type name and description
- Define custom fields for request type
- Set default approver(s) for request type
- Activate/deactivate request type

**Acceptance Criteria:**
- Changes take effect immediately
- Deactivated types are hidden from requesters but existing requests remain visible
- Configuration changes are logged in audit trail

---

## Out of Scope for MVP

The following features are explicitly **NOT** included in MVP and should be deferred:

- ❌ Multi-level approval chains
- ❌ Budget tracking and cost management
- ❌ Asset inventory management
- ❌ SLA-based auto-escalation
- ❌ Conditional workflow routing
- ❌ Native mobile apps (web responsive only)
- ❌ Third-party integrations (Slack, JIRA, etc.)
- ❌ Real-time chat/messaging
- ❌ Approval delegation
- ❌ Recurring request templates
- ❌ Advanced analytics and AI insights
- ❌ Custom workflow builder
- ❌ Multi-language support
