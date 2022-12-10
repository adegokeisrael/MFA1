# Multi-factor Authenticator


```
### A simple server implementation for a Universal second Factor Authentication Private Key infrastrution Digital certificate, and a location based authentication allow. The script created to ease the process of creating self signed certificates. It will create both CA and Server/Client certificates.
```
# U2F authentication library

This is a simple library to register and check signatures provided by U2F clients/devices.
It's intended to be used in Relying Parties - websites that want to add U2F 2-factor authentication
for their users.

To use U2F, it is recommended to familiarize yourself with [FIDO Alliance Specifications](https://fidoalliance.org/download/),
although basic usage is shown below.

## U2F Overview/properties

 * U2F provides hardware-based 2-nd factor authentication system. Public/private key infrastructure is used
   to ensure good security.
 * Provides proof of posession of hardware key, plus user presence flag.
 * Public/private key pairs are specific to website origin and 'application id'. Keys are useless if used from
   other origins.
 * Needs to be stored on server for each user: Key handle and public key (both strings).
 * Cannot be used as main authentication system because server needs to provide
   unique key handle to the user to get the signature.
## What will change of prodution

```
1. the certification will be done by a trusted certificate authority,Google.
```