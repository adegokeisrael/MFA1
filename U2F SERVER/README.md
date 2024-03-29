u2f-authenticator implementation
==================

This project implements A node.js served [Fido U2F authentication](https://fidoalliance.org/about/overview/) phishing-resistant MFA. A Multi-factor Authentication (MFA) is an authentication method that requires the user of a service to provide more than a username and password for service access. MFA requires one or more additional verification factors, with aim to decrease the likelihood of a successful cyber attack. This implementation provides a  second factor authentication to the regular email and password scheme.
The FIDO U2F protocol enables relying parties to offer a strong cryptographic 2nd factor option for end user security. The relying party's dependence on passwords is reduced. The password can even be simplified to a 4 digit PIN. End users carry a single U2F device which works with any relying party supporting the protocol. The user gets the convenience of a single 'keychain' device and convenient security.

### why this strategy?

This implementation solves major and common problems associated with the vulnerabilities of common MFAs with rise in the unrelibility of these MFAs (Authy and Google Authenticator). These MFA are prone to attacks such as:

* Evilginx : a form of MITM (Man in the Middle attack)
* pass the cookie attack 
* SMS MITM/ SIM swap attack (in the case of SMS based OTP.)
* Attacks on Hard and Soft tokens

### architerture
![u2f-protocol](https://user-images.githubusercontent.com/47308654/206934475-24d1abf8-bcf9-4c49-aaba-bc7c13abcb1b.png)


Installation
------------

Make sure you have node.js install on your machine.  Clone the source code from github and install the dependencies

```
npm install
```

Note that these implementation uses https connections. A sample self-signed certificate is included.  You can generate your own self-signed certificate by
```
openssl ecparam -genkey -out sslcert/private-key.pem -name prime256v1
openssl req -x509 -new -key sslcert/private-key.pem -out sslcert/server.pem
```


Usage
-----

Start the server by running
```
npm start
```

The server will start listening on localhost and port 4430 by default.  Point your Chrome / Chromium browser to
```
https://localhost:4430/demo
```

You can then simulate:
- Register a new token with the server.
- Authenticate with the server using the token.

You will be able to see the U2F messages passed between the server and token.

![Capture](https://user-images.githubusercontent.com/47308654/212582262-f37a2fca-0488-4de5-82b4-f31383d2fd27.PNG)


### Registration

To simulate a registration, click on the button to request a challenge from server.  Then, depending on the type of the token you have, your will need to
- un-plug and then plug in your token; or
- press the button on your token

The token will generate a key pair to sign the challenge and generate a response.  The response will then be sent to the server for validation and persisted for future authentication.

For demo purpose only, you can register the same token multiple times with the demo server.  After each successful registration, a new keyHandle will be shown on the browser page.  Note that in real life the server (Relying Party) will not send back the registration data to the browser (Client).  See comments in source code for details.

For this demo, the registration data is stored in session only.  In real life application, the data should be persisted in DB.

### Authentication

To simulate an authentication event, click on the button next to a particular keyHandle.  The server will generate a challenge in which the browser will pass to the token.  Again, plug in the token or click on the button on the token to sign the challenge. The response will be sent to server to validate and complete the authentication.

In production, the token is used as a two-factor authentication device.  So normally the server will trigger such authentication after the initial user/password authentication.



### !!! Caution !!!

Note that each registration requires a new key pair from the token.  Some tokens can generate infinite number of key pairs by using key-wrapping or deterministic algorithm (e.g. [Yubico's U2F tokens](https://www.yubico.com/blog/yubicos-u2f-key-wrapping/)).

But some tokens (e.g. the one I have from [HyperFIDO](https://www.hypersecu.com/hyperfido)) only support limited number of key pairs (64 pairs for HyperFIDO). Once over the limit, you won't be able to register the token with any new sites.  Some vendors do provide a reset program for you to wipe the token though.

### What will change in production?
  * signing of certificate will be done by a trusted certificate authority.
  * client application could be written in any framework from reactjs to vuejs and react naitive for mobile app. 
