# Post-Incident Report (PIR)

**Incident ID:** INC-2026-0408-001  
**Date:** 2026-04-08  
**Severity:** P2  
**Status:** Resolved

## Incident Summary

On 2026-04-08, 25 users in the finance department of a mid-size financial institution were unable to receive SMS OTPs for MFA. Email OTP continued to work. The issue was detected at 09:15 IST and investigated through the Kaleyra dashboard, where REJECTED status codes were observed. The incident was escalated to L2, the Kaleyra sender ID issue was identified, and a backup sender ID was activated. Customer testing confirmed that OTP delivery was working again at 11:45 IST.

## Timeline

| Time (IST) | Event |
|---|---|
| 09:15 | Customer reported MFA failure for 25 users |
| 09:18 | Ticket #1001 created – priority P2 |
| 09:20 | First response sent to customer |
| 09:35 | Kaleyra checked – REJECTED codes confirmed |
| 10:30 | Escalated to L2 |
| 11:00 | L2 identified Kaleyra sender ID flagged |
| 11:40 | Backup sender ID activated |
| 11:45 | Customer confirmed OTP working |

## Root Cause

The Kaleyra sender ID was flagged for a DND-related violation by TRAI due to another sender sharing the route. This caused SMS OTP requests for the affected users to receive REJECTED status codes.

## Impact Assessment

- **Users affected:** 25
- **Department:** Finance
- **Severity:** P2
- **Duration:** 2 hours 30 minutes
- **Business impact:** Affected users could not complete MFA using SMS OTP. Email OTP remained available.

## Resolution Steps

1. Investigated the Kaleyra dashboard and confirmed REJECTED status codes.
2. Escalated the issue to L2 for sender ID investigation.
3. Identified the flagged sender ID.
4. Switched to the backup sender ID.
5. Verified OTP delivery using test numbers.
6. Customer confirmed successful OTP delivery.

## Prevention Actions

- [ ] Add sender ID monitoring alerts in the Kaleyra dashboard.
- [ ] Maintain and periodically validate a backup sender ID.
- [ ] Add carrier/sender ID status checks to the MFA operational checklist.

## Open Items

- [ ] Review Kaleyra sender ID status and carrier whitelist.
- [ ] Document sender ID monitoring procedure in the internal KB.
- [ ] Verify backup sender ID readiness periodically.
