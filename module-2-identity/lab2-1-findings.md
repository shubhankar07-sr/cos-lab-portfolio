# Lab 2.1 — OpenLDAP Directory Findings

## Objective

Deploy and verify an OpenLDAP directory to simulate a basic Active Directory-style identity service.

## 1. OpenLDAP Installation

Installed the required OpenLDAP packages:

```bash
sudo apt install -y slapd ldap-utils

## 2. LDAP Directory Configuration

LDAP directory was configured with the following base structure:

- Base DN: `dc=lab,dc=instasafe,dc=local`
- People OU: `ou=People,dc=lab,dc=instasafe,dc=local`
- Groups OU: `ou=Groups,dc=lab,dc=instasafe,dc=local`

The directory was verified using `ldapsearch`.

## 3. Users Created

The LDAP directory contains the following users:

- Alice Smith
  - UID: `alice`
  - Email: `alice@lab.instasafe.local`
- Bob Jones
  - UID: `bob`
  - Email: `bob@lab.instasafe.local`

Both users were successfully returned by the LDAP search.

## 4. LDAP Authentication Validation

Alice's LDAP credentials were tested using `ldapwhoami`.

Successful authentication returned:

`dn:uid=alice,ou=People,dc=lab,dc=instasafe,dc=local`

A wrong-password test was also performed. LDAP returned:

`ldap_bind: Invalid credentials (49)`

This confirms that invalid LDAP credentials are rejected correctly.

## 5. InstaSafe AD Sync Mapping

The OpenLDAP setup simulates an Active Directory-style identity directory.

- **Base DN:** Identifies the root of the LDAP directory.
- **Bind DN:** `cn=admin,dc=lab,dc=instasafe,dc=local`
- **User identity:** LDAP `uid` identifies individual users.
- **User attributes:** `cn`, `sn`, and `mail` provide user information.
- **Authentication:** LDAP bind validates the user's credentials.

These concepts are relevant to directory-based identity synchronization and authentication in InstaSafe.

## 6. Evidence

- `lab2-1-ldapsearch-users.png` — LDAP directory structure and users.
- `lab2-1-ldapwhoami-success.png` — Successful Alice LDAP authentication.
- `lab2-1-ldapwhoami-error49.png` — Invalid credentials error (49).
