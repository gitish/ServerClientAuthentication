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



## Certificates
Although Alice could have sent a private message to the bank, signed it and ensured the integrity of the message, she still needs to be sure that she is really communicating with the bank. This means that she needs to be sure that the public key she is using is part of the bank's key-pair, and not an intruder's. Similarly, the bank needs to verify that the message signature really was signed by the private key that belongs to Alice.

If each party has a certificate which validates the other's identity, confirms the public key, and is signed by a trusted agency, then both can be assured that they are communicating with whom they think they are. Such a trusted agency is called a Certificate Authority and certificates are used for authentication.

## Certificate Contents
A certificate associates a public key with the real identity of an individual, server, or other entity, known as the subject. As shown in Table 1, information about the subject includes identifying information (the distinguished name) and the public key. It also includes the identification and signature of the Certificate Authority that issued the certificate and the period of time during which the certificate is valid. It may have additional information (or extensions) as well as administrative information for the Certificate Authority's use, such as a serial number.

### Table 1: Certificate Information
Subject	Distinguished Name, Public Key
Issuer	Distinguished Name, Signature
Period of Validity	Not Before Date, Not After Date
Administrative Information	Version, Serial Number
Extended Information	Basic Constraints, Netscape Flags, etc.
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

Example of a PEM-encoded certificate (snakeoil.crt)
```
-----BEGIN CERTIFICATE-----
MIIC7jCCAlegAwIBAgIBATANBgkqhkiG9w0BAQQFADCBqTELMAkGA1UEBhMCWFkx
FTATBgNVBAgTDFNuYWtlIERlc2VydDETMBEGA1UEBxMKU25ha2UgVG93bjEXMBUG
A1UEChMOU25ha2UgT2lsLCBMdGQxHjAcBgNVBAsTFUNlcnRpZmljYXRlIEF1dGhv
cml0eTEVMBMGA1UEAxMMU25ha2UgT2lsIENBMR4wHAYJKoZIhvcNAQkBFg9jYUBz
bmFrZW9pbC5kb20wHhcNOTgxMDIxMDg1ODM2WhcNOTkxMDIxMDg1ODM2WjCBpzEL
MAkGA1UEBhMCWFkxFTATBgNVBAgTDFNuYWtlIERlc2VydDETMBEGA1UEBxMKU25h
a2UgVG93bjEXMBUGA1UEChMOU25ha2UgT2lsLCBMdGQxFzAVBgNVBAsTDldlYnNl
cnZlciBUZWFtMRkwFwYDVQQDExB3d3cuc25ha2VvaWwuZG9tMR8wHQYJKoZIhvcN
AQkBFhB3d3dAc25ha2VvaWwuZG9tMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKB
gQDH9Ge/s2zcH+da+rPTx/DPRp3xGjHZ4GG6pCmvADIEtBtKBFAcZ64n+Dy7Np8b
vKR+yy5DGQiijsH1D/j8HlGE+q4TZ8OFk7BNBFazHxFbYI4OKMiCxdKzdif1yfaa
lWoANFlAzlSdbxeGVHoT0K+gT5w3UxwZKv2DLbCTzLZyPwIDAQABoyYwJDAPBgNV
HRMECDAGAQH/AgEAMBEGCWCGSAGG+EIBAQQEAwIAQDANBgkqhkiG9w0BAQQFAAOB
gQAZUIHAL4D09oE6Lv2k56Gp38OBDuILvwLg1v1KL8mQR+KFjghCrtpqaztZqcDt
2q2QoyulCgSzHbEGmi0EsdkPfg6mp0penssIFePYNI+/8u9HT4LuKMJX15hxBam7
dUHzICxBVC1lnHyYGjDuAMhe396lYAn8bCld1/L4NMGBCQ==
-----END CERTIFICATE-----
```
Certificate Authorities
By verifying the information in a certificate request before granting the certificate, the Certificate Authority assures itself of the identity of the private key owner of a key-pair. For instance, if Alice requests a personal certificate, the Certificate Authority must first make sure that Alice really is the person the certificate request claims she is.

Certificate Chains
A Certificate Authority may also issue a certificate for another Certificate Authority. When examining a certificate, Alice may need to examine the certificate of the issuer, for each parent Certificate Authority, until reaching one which she has confidence in. She may decide to trust only certificates with a limited chain of issuers, to reduce her risk of a "bad" certificate in the chain.

Creating a Root-Level CA
As noted earlier, each certificate requires an issuer to assert the validity of the identity of the certificate subject, up to the top-level Certificate Authority (CA). This presents a problem: who can vouch for the certificate of the top-level authority, which has no issuer? In this unique case, the certificate is "self-signed", so the issuer of the certificate is the same as the subject. Browsers are preconfigured to trust well-known certificate authorities, but it is important to exercise extra care in trusting a self-signed certificate. The wide publication of a public key by the root authority reduces the risk in trusting this key -- it would be obvious if someone else publicized a key claiming to be the authority.

A number of companies, such as Thawte and VeriSign have established themselves as Certificate Authorities. These companies provide the following services:

Verifying certificate requests
Processing certificate requests
Issuing and managing certificates
It is also possible to create your own Certificate Authority. Although risky in the Internet environment, it may be useful within an Intranet where the organization can easily verify the identities of individuals and servers.

| Keystore | Truststore |
|------------|---------|
|Keystore is used to store private key and identity certificates that a specific program should present to both parties (server or client) for verification.| Truststore is used to store certificates from Certified Authorities (CA) that verify the certificate presented by the server in SSL connection.|
|Keystore stores your credential|Truststore stores others credentials|
|Keystore is needed when you are setting up the server side on SSL|Truststore setup is required for the successful connection at the client side|
|Client will store its private key and identify certificate on Keystore|Server will authenticate the client against the certificate stored on the server’s Truststore|
|javax.net.ssl.keyStore is used to specify Keystore | javax.net.ssl.trustStore is used to specify Truststore |
|Keystore passwords are stored in plaintext that is only readable by the specific group|Truststore passwords are stored in plaintext that can be read by everyone|
|Keystore contains private and sensitive information|Truststore doesn’t contain private and sensitive information









