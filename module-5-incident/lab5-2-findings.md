# Lab 5.2 — Change Management: TLS Upgrade

## Change Summary

**Change ID:** CHANGE-001  
**Change:** Update Nginx TLS configuration to prefer TLS 1.3  
**System:** Lab Nginx Gateway  
**Status:** Completed

## Change Execution

1. Backed up the existing Nginx site configuration.
2. Added `ssl_protocols TLSv1.3;` to the HTTPS server block.
3. Validated the configuration using `nginx -t`.
4. Reloaded Nginx successfully.
5. Verified the TLS 1.3 handshake using OpenSSL.
6. Confirmed Nginx remained active after the change.

## Verification

Nginx configuration test:

```text
syntax is ok
test is successful
```

TLS verification:

```text
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Protocol : TLSv1.3
```

Nginx service status:

```text
Active: active (running)
```

## Rollback

A backup of the original Nginx configuration was created before the change.

**Rollback trigger:** TLS verification failure or site becoming unreachable.

**Rollback steps:**

1. Restore the backed-up Nginx configuration.
2. Run `nginx -t` to validate the restored configuration.
3. Reload Nginx using `sudo systemctl reload nginx`.
4. Verify that the site is reachable and TLS is functioning.

## Evidence

- `screenshots/lab5-2-tls13-verification.png`
- `changes/CHANGE-001-TLS-Upgrade.md`
