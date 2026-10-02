# Lab 4.1 – GitHub as a Support Desk

## Objective

Simulate a complete support ticket lifecycle using GitHub Issues and Projects, including ticket creation, triage, escalation, resolution, and closure.

## Repository

Support simulation repository:

`cogs-support-lab-tickets`

## Labels Created

The following support labels were created:

- P1-Critical
- P2-High
- P3-Medium
- P4-Low
- status:in-progress
- status:waiting-on-customer
- status:resolved
- product:ztna
- product:mfa
- product:sso

## Milestone

`Sprint 1 – Lab Tickets`

## Project Board

The project board contains the following lifecycle columns:

- Backlog
- In Progress
- Waiting on Customer
- Resolved
- Closed

## Mock Tickets

### Ticket 1 – P2 MFA Failure

**Title:**  
[P2] MFA SMS OTP not delivered – finance department, 25 users affected

**Impact:**  
25 finance users were unable to receive SMS OTP. Email OTP continued to work.

**Lifecycle:**  
Creation → Investigation → Escalation → Resolution → Closure

### Ticket 2 – P3 Account Lockout

**Title:**  
[P3] User account locked out after password change – AD sync delay suspected

**Impact:**  
One user was unable to access the application after changing their password.

### Ticket 3 – P4 Gateway Guidance

**Title:**  
[P4] How to add a second gateway – customer requesting guidance

**Purpose:**  
Customer requested guidance on the steps and prerequisites for adding a second gateway.

## Ticket 1 Lifecycle

### Investigation

An internal investigation note was added after checking the Kaleyra dashboard. Bulk REJECTED status codes were observed for all 25 affected numbers.

### Escalation

The P2 ticket was escalated to L2 for investigation of the Kaleyra sender ID and carrier whitelist.

### Resolution

The sender ID issue was resolved by switching to a backup sender ID. Delivery was verified using test numbers and the customer confirmed successful OTP delivery for all affected users.

### Closure

The ticket was moved through the project workflow to Resolved and then closed after customer confirmation.

## Evidence

- `lab4-1-labels.png` – GitHub labels
- `lab4-1-project-board.png` – Project board
- `lab4-1-ticket1-internal-escalation.png` – Ticket 1 investigation and escalation
- `lab4-1-ticket1-resolution-closure.png` – Ticket 1 resolution and closure

## Conclusion

This lab demonstrated the complete support ticket lifecycle using GitHub Issues and Projects, including priority classification, product categorization, investigation notes, escalation, resolution, customer confirmation, and closure.
