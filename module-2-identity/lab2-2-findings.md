# Lab 2.2 – SAML SSO with Keycloak

## Objective

Configure Keycloak as a SAML Identity Provider (IdP) and simulate a Service Provider (SP) using Python Flask.

## Environment

- Keycloak Realm: `instasafe-lab`
- Test User: `testuser`
- Test Group: `support-team`
- SAML Client ID: `https://sp.instasafe.local/saml`
- Flask SP Port: `9090`

## 1. Keycloak Realm and User Configuration

Created the Keycloak realm:

`instasafe-lab`

Created test user:

- Username: `testuser`
- Email: `user@test.instasafe.local`

A password was configured for the test user.

Created group:

`support-team`

Added `testuser` to the `support-team` group.

## 2. SAML Client Configuration

Created a SAML client with:

`https://sp.instasafe.local/saml`

Client name:

`InstaSafe SP Simulation`

Configured the SAML client for the Python Flask Service Provider.

## 3. SAML Client Scope and Attribute Mappers

Created client scope:

`saml-attributes`

The scope was assigned to the SAML client as a **Default** client scope.

### Email Mapper

- Mapper Type: `User Property`
- Name: `email`
- Property: `email`
- Friendly Name: `email`
- SAML Attribute Name: `email`

### Groups Mapper

- Mapper Type: `Group List`
- Name: `groups`
- Group Attribute Name: `member`
- Friendly Name: `groups`
- SAML Attribute Name Format: `Basic`
- Single Group Attribute: `On`
- Full Group Path: `On`

## 4. Keycloak IdP Metadata

Downloaded the Keycloak SAML IdP metadata from:

`http://52.66.109.44:8080/realms/instasafe-lab/protocol/saml/descriptor`

Saved the metadata file as:

`keycloak-idp-metadata.xml`

## 5. Python Flask SAML Service Provider

Installed the required Python packages:

- `python3-pip`
- `python3-saml`
- `flask`

Created the Flask SAML application:

`saml_sp_test.py`

The application was configured with:

- IdP URL: `http://52.66.109.44:8080/realms/instasafe-lab/protocol/saml`
- SP Entity ID: `https://sp.instasafe.local/saml`
- ACS URL: `http://52.66.109.44:9090/saml/callback`

Flask application endpoint:

`http://52.66.109.44:9090/saml/login`

Callback endpoint:

`http://52.66.109.44:9090/saml/callback`

## 6. SAML Authentication Test

Accessed:

`http://52.66.109.44:9090/saml/login`

The Flask application generated a SAML authentication request and redirected the browser to Keycloak.

Authenticated using the `testuser` account.

After authentication, Keycloak returned the SAML response to:

`http://52.66.109.44:9090/saml/callback`

The callback page displayed:

`SAML Response`

## 7. Verification

Flask logs confirmed successful SAML requests:

`GET /saml/login` → `302`

`POST /saml/callback` → `200`

This confirmed that the SAML authentication flow from the Flask SP to Keycloak and back to the callback endpoint was working successfully.

## 8. Troubleshooting

### HTTPS Required

Initially, Keycloak displayed:

`HTTPS required`

The `instasafe-lab` realm was configured with:

`sslRequired=NONE`

for the lab environment.

### Invalid SAML Request

The initial dummy SAML request was rejected by Keycloak.

The Flask application was then updated to generate a proper SAML `AuthnRequest` using HTTP-Redirect encoding.

### Client Signature Required

Keycloak initially returned a `SigAlg was null` validation error because **Client signature required** was enabled for the SAML client.

The setting was changed to:

`Client signature required = Off`

After saving the configuration, the SAML authentication flow completed successfully.

## Result

The SAML SSO flow was successfully tested using Keycloak as the Identity Provider and Python Flask as the Service Provider.

The test user successfully authenticated through Keycloak and the SAML response was received by the Flask callback endpoint.
