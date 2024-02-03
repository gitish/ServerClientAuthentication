# ServerClientAuthentication

## 1. What is SSL/TLS Strong Encryption
The Secure Sockets Layer protocol is a protocol layer that may be placed between a reliable connection-oriented network layer protocol (e.g. TCP/IP) and the application protocol layer (e.g. HTTP). SSL provides for secure communication between client and server by allowing mutual authentication, the use of digital signatures for integrity, and encryption for privacy.
For more information check [SSL/TLS Intro](https://httpd.apache.org/docs/current/ssl/ssl_intro.html#ssl) official page.

## What’s the difference between TLS and SSL?
Both TLS and SSL are protocols that help you securely authenticate and transport data on the Internet. TLS (Transport Layer Security), and SSL (Secure Socket Layers), are both cryptographic protocols that encrypt data and authenticate a connection when moving data on the Internet.

For example, if you’re processing credit card payments on your website, TLS and SSL can help you securely process that data so that malicious actors can’t get their hands on it.


✨ Well, TLS is just a more recent version of SSL. It fixes some security vulnerabilities in the earlier SSL protocols. Before you learn more about the specifics, it’s important to understand the basic history of SSL and TLS.

|  Version  |                Description                                              |
|-----------|-------------------------------------------------------------------------|
|SSL 1.0    | Never publicly released due to security issues.                         |
|SSL 2.0    | Released in 1995. Deprecated in 2011. Has known security issues.        |
|SSL 3.0    | Released in 1996. Deprecated in 2015. Has known security issues.        |
|TLS 1.0    | Released in 1999 as an upgrade to SSL 3.0. Planned deprecation in 2020. |
|TLS 1.1    | Released in 2006. Planned deprecation in 2020.                          |
|TLS 1.2    | Released in 2008.                                                       |
|TLS 1.3    | Released in 2018.                                                       |

SSL 2.0 was first released in February 1995 (SSL 1.0 was never publicly released because of security flaws). Although SSL 2.0 was publicly released, it also contained security flaws and was quickly replaced by SSL 3.0 in 1996. Then, in 1999, the first version of TLS (1.0) was released as an upgrade to SSL 3.0. Since then, there have been three more TLS releases, with the most recent release being TLS 1.3 in August 2018. At this point, both public SSL releases have been deprecated and have known security vulnerabilities (more on this later).

# What’s the Difference Between Client Certificates and Server Certificates?
**Client Certificates** are digital certificates for users and individuals to prove their identity to a server. Client certificates tend to be used within private organizations to authenticate requests to remote servers. 
**Server certificates** are more commonly known as TLS/SSL certificates and are used to protect servers and web domains. Server Certificates perform a very similar role to Client Certificates, except the latter is used to identify the client/individual and the former authenticates the owner of the site.

For more details check [source](https://www.digicert.com/faq/public-trust-and-certificates/whats-the-difference-between-client-certificates-vs-server-certificates)

## SSL Handshake
| Connection | Example |
|------------|---------|
|![image](https://github.com/gitish/ServerClientAuthentication/assets/16752447/73a0a49b-381a-4651-9816-c5813887450c) | <img width="419" alt="image" src="https://github.com/gitish/ServerClientAuthentication/assets/16752447/7af8f2c8-6210-4818-8bd8-c7c542b9da09"> |

| Keystore | Truststore |
|------------|---------|
|Keystore is used to store private key and identity certificates that a specific program should present to both parties (server or client) for verification.| Truststore is used to store certificates from Certified Authorities (CA) that verify the certificate presented by the server in SSL connection.|
|Keystore stores your credential|Truststore stores others credentials|
|Keystore is needed when you are setting up the server side on SSL|Truststore setup is required for the successful connection at the client side|
|Client will store its private key and identify certificate on Keystore|Server will authenticate the client against the certificate stored on the server’s Truststore|
|javax.net.ssl.keyStore is used to specify Keystore | javax.net.ssl.trustStore is used to specify Truststore |
|Keystore passwords are stored in plaintext that is only readable by the specific group|Truststore passwords are stored in plaintext that can be read by everyone|
|Keystore contains private and sensitive information|Truststore doesn’t contain private and sensitive information|

## Certificates
Suppose Sid wants to send a message to his bank to transfer some money. Sid would like the message to be private, since it will include information such as his account number and transfer amount. One solution is to use a cryptographic algorithm, a technique that would transform her message into an encrypted form, unreadable until it is decrypted. Once in this form, the message can only be decrypted by using a secret key. Without the key the message is useless: good cryptographic algorithms make it so difficult for intruders to decode the original text that it isn't worth their effort.
Although Sid could have sent a private message to the bank, signed it and ensured the integrity of the message, He still needs to be sure that He is really communicating with the bank. This means that he needs to be sure that the public key he is using is part of the bank's key-pair, and not an intruder's. Similarly, the bank needs to verify that the message signature really was signed by the private key that belongs to Sid.

If each party has a certificate which validates the other's identity, confirms the public key, and is signed by a trusted agency, then both can be assured that they are communicating with whom they think they are. Such a trusted agency is called a Certificate Authority and certificates are used for authentication.

## Certificate Contents
A certificate associates a public key with the real identity of an individual, server, or other entity, known as the subject. As shown in Table 1, information about the subject includes identifying information (the distinguished name) and the public key. It also includes the identification and signature of the Certificate Authority that issued the certificate and the period of time during which the certificate is valid. It may have additional information (or extensions) as well as administrative information for the Certificate Authority's use, such as a serial number.

### Table 1: Certificate Information
**Subject**: Distinguished Name, Public Key
**Issuer**: Distinguished Name, Signature
**Period of Validity**: Not Before Date, Not After Date
**Administrative Information**: Version, Serial Number
**Extended Information**: Basic Constraints, Netscape Flags, etc.

A distinguished name is used to provide an identity in a specific context -- for instance, an individual might have a personal certificate as well as one for their identity as an employee. Distinguished names are defined by the X.509 standard [X509], which defines the fields, field names and abbreviations used to refer to the fields (see Table 2).

### Table 2: Distinguished Name Information
| DN Field              |	Abbrev.	|                          Description	                                |       Example         |
|-----------------------|---------|-----------------------------------------------------------------------|-----------------------|
|Common Name            |	CN	    | Name being certified	                                                | CN=Joe Average        |
|Organization or Company|	O	      | Name is associated with this organization	                            | O=Snake Oil, Ltd.     |
|Organizational Unit    |	OU	    | Name is associated with this organization unit, such as a department	| OU=Research Institute |
|City/Locality	        | L	      | Name is located in this City	                                        | L=Snake City          |
|State/Province         | ST	    | Name is located in this State/Province	                              | ST=Desert             |
|Country                |	C	      | Name is located in this Country (ISO code)                            |	C=XZ                  |

A Certificate Authority may define a policy specifying which distinguished field names are optional and which are required. It may also place requirements upon the field contents, as may users of certificates. For example, a Netscape browser requires that the Common Name for a certificate representing a server matches a wildcard pattern for the domain name of that server, such as *.snakeoil.com.

The binary format of a certificate is defined using the ASN.1 notation [ASN1] [PKCS]. This notation defines how to specify the contents and encoding rules define how this information is translated into binary form. The binary encoding of the certificate is defined using Distinguished Encoding Rules (DER), which are based on the more general Basic Encoding Rules (BER). For those transmissions which cannot handle binary, the binary form may be translated into an ASCII form by using Base64 encoding [MIME]. When placed between begin and end delimiter lines (as below), this encoded version is called a PEM ("Privacy Enhanced Mail") encoded certificate.

Example of a PEM-encoded certificate (github.com.cer)
```
-----BEGIN CERTIFICATE-----
MIID5jCCAs6gAwIBAgIRAIqsX/INb037gLKu1Y9eXDEwDQYJKoZIhvcNAQELBQAw
gcIxCzAJBgNVBAYTAkdCMQswCQYDVQQIEwJHQjEPMA0GA1UEBxMGbG9uZG9uMRgw
FgYDVQQKEw9QdWJsaWNpcyBHcm91cGUxKTAnBgNVBAsTIGNkODUwYTQ2MWQxNzRi
MWU3YTBkYjk3MDQ4YWQ0YTI1MSkwJwYDVQQDEyBjYS5wdWJsaWNpc2dyb3VwZS5k
ZS5nb3Nrb3BlLmNvbTElMCMGCSqGSIb3DQEJARYWY2VydGFkbWluQG5ldHNrb3Bl
LmNvbTAeFw0yMzEyMjcxNjE1MzNaFw0yNTAxMjUxNjE1MzNaMBcxFTATBgNVBAMM
DCouZ2l0aHViLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALqO
uAftyP6tOBcLP1Z2aLI2LplFoY9+RefFRUsP61+QbpEVO8yjpSMfH9L+NeF6Da0b
Scc+KUjowo87srYilQawKi3w1Zj7UEYDTCrdq6osxjGyajLmuFcj5wa9li7nGk1i
E0yuFzw/pzGt9xt9a/Xf34aCmb/5jspWXomTN5SG17UZ/DdQGs7ommzKH0Cqu+vH
Sm0MerW8aZy4d2XqyWBR25arMJH09W8+acRWM1IVuvyF/jbKviD6MFdvxG8L2Ykp
NaWAc7T35zcrRamCWGHoMAwqSIk1xRICAZSxRr6jHCzACVJIeOZ2UExQmljwl0Ds
rmiRA3Brul4BeJFW11cCAwEAAaOBgDB+MAkGA1UdEwQCMAAwHQYDVR0OBBYEFDvv
VHYiz+OtVqQWOkhrEXZIAcUOMCMGA1UdEQQcMBqCDCouZ2l0aHViLmNvbYIKZ2l0
aHViLmNvbTAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsG
AQUFBwMCMA0GCSqGSIb3DQEBCwUAA4IBAQBKefzMmyOsWBR3Z1lP9CBy7A97zFG1
xC1p3qbIV0mjjBxsDDaRJTnj6k5LoLOKQ/OwHJRwq5xK/vp9l7FoJs2DKh5TL0Qk
Nwp3mPAZJvFM34n/ti7XT65beRMiBFLwoWREwwEm0gl82QBSf0VlZa4Q+91zbeZx
9gWedh3578HTh2+owCIZZ07E+sXb1TQGKcFqnzi0ggykTHtxib+MY3q0XavEpkQb
md7oGMuw0AGbRfffAnO7gBpNFX9iDuH3iDoJYKCvLWgUcXeu4dhcHUeKpW2Yxa9j
qZA77XtSUsR03u+Jyw8xtFyQqFv/00eOBOhSK25D3jAweT1WFOWexg7H
-----END CERTIFICATE-----
```
### Certificate Authorities
By verifying the information in a certificate request before granting the certificate, the Certificate Authority assures itself of the identity of the private key owner of a key-pair. For instance, if Alice requests a personal certificate, the Certificate Authority must first make sure that Alice really is the person the certificate request claims she is.

### Certificate Chains
A Certificate Authority may also issue a certificate for another Certificate Authority. When examining a certificate, Alice may need to examine the certificate of the issuer, for each parent Certificate Authority, until reaching one which she has confidence in. She may decide to trust only certificates with a limited chain of issuers, to reduce her risk of a "bad" certificate in the chain.

### Creating a Root-Level CA
As noted earlier, each certificate requires an issuer to assert the validity of the identity of the certificate subject, up to the top-level Certificate Authority (CA). This presents a problem: who can vouch for the certificate of the top-level authority, which has no issuer? In this unique case, the certificate is "self-signed", so the issuer of the certificate is the same as the subject. Browsers are preconfigured to trust well-known certificate authorities, but it is important to exercise extra care in trusting a self-signed certificate. The wide publication of a public key by the root authority reduces the risk in trusting this key -- it would be obvious if someone else publicized a key claiming to be the authority.
