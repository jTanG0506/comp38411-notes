---
title: X.509 Certificates
markup: "mmark"
weight: 3
---

## X.509 Certificates
X.509 Certificates are used to verify web servers, authenticate to a network service, or sign executable programs. In this *chain of trust* model, the CAs operate through a strictly hierarchical organisation in which the trust can only flow downwards. The CAs at the top of the hierarchy is known as **Root CAs**, and the CAs below the root is commonly referred to as **Intermediate-Level CAs**.

![Chain Of Trust](/docs/figures/chain-of-trust.png)

The root certificate is issued by a CA, which might be a public organisation, such as Verisign, Comodo, and so on, or a private entity that issues certificates for use on internal networks. By definition, the root certificate cannot be signed by another certificate, so instead, the private key associated with the certificate's public key is used to sign itself, and this is known as a **self-signed certificate**.

There are three kinds of certificates, depending on the level of identity assurance and authentication that was carried out with regard to the applicant organisation.

- **Extended Validation (EV)**: the highest level certificates that are issued after a rigorous identity verification processes for establishing the legitimacy of the applicant organisation. These certificates can take days for a CA to issue and are the most expensive.
- **Organisation Validation (OV)**: the next lower level certificates, which usually verify the existence of the organisation, the domain name, and optionally a follow-up phone call from the CA.
- **Domain Validation (DV)**: the lowest level of identity and domain validation, where the only check that is made before such a certificate is issued is that the applicant has the right to use a specific domain name. These certificates are usually issued in a matter of minutes and are the least expensive.

-----

### X.509 Certificate Format

![X509 Certificate](/docs/figures/x509-certificate.png)

The different fields of the X.509 certificate are as follows:
- **Version Number**: the version of X.509 standard to which the certificate corresponds.
- **Serial Number**: a unique serial number assigned to the certificate by the CA.
- **Signature Algorithm ID**: the name of the digital signature algorithm used to sign the certificate. For example, RSA or DSA.
- **Issuer**: name of the Certificate Authority that issued this certificate.
- **Validity Period**: the period during which the certificate is valid, defined by two date times, not-before and not-after.
- **Subject**: the identifier for the individual or organisation to which the certificate was issued.
- **Subject Public Key**: the public key that is meant to be authenticated by this certificate. This field also names the algorithm used for public-key generation.
- **Issuer Unique Identifier**: allows multiple CA's to operate as logically a single CA.
- **Subject Unique Identifier**: allows multiple certificate holders to act as a single logical entity.
- **Extensions**: allows a CA to associate additional private information to a certificate, such as ways to manage certification hierarchy and certification revocation lists.
- **Signature**: the digital signature by the issuing CA for the certificate. This signature is obtained by computing a message digest of the above fields with a hashing algorithm and then encrypting it with the CA's private key.

The authenticity of the contents of the certificate can be verified by using the CA's public key to retrieve the message digest and then by comparing this digest with the one computed from the rest of the fields.

#### Certificate Notation
The X.509 standard uses the following notation to define a certificate

$$CA \; \langle \langle A \rangle \rangle = CA \; \{V, SN, AI, CA, UCA, A, UA, Ap, T^A\}$$

where

$$
\begin{aligned}
CA \; \langle \langle A \rangle \rangle & = \; \text{the certificate of user A issued by certification authority CA}\\
CA \; \{I\} & = \; \text{the signing of I by CA, consisting of I with the encrypted hash code appended}\\
V & = \; \text{version of the certificate}\\
SN & = \; \text{serial number of the certificate}\\
AI & = \; \text{identifier of the algorithm used to sign the certificate}\\
CA & = \; \text{name of the certificate authority}\\
UCA & = \; \text{optional unique identifier of the CA}\\
A & = \; \text{name of user A}\\
UA & = \; \text{optional unique identifier of the user A}\\
Ap & = \; \text{public key of user A}\\
T^A & = \; \text{period of validity of the certificate}
\end{aligned}
$$

-----

### Certificate Revocation Lists
Suppose we wish to revoke the validity of a certificate before it is expired, just like when you lose your bank card and wish to block the card from further use. Certificates that we wish to revoke before their expiration date are added to a Certificate Revocation List, and when a user receives a certificate in a message, the user must check if the certificate has been revoked.

Some of the reasons we may wish to revoke a certificate before it expires include:

1. The user's private key has been compromised
1. The user is no longer certified by this CA
1. The CA's certificate has been compromised

![Certificate Revocation List](/docs/figures/certificate-revocation-list.png)

When a user receives a certificate in a message, the user must ensure that it has a valid CA signature, has not expired, and is not listed in the CA's most recent CRL. To avoid the delays and costs of checking the CRL directory each time a certificate is received, it is common to maintain a local cache of certificates and revoked certificates lists.

-----

### Verifying a Certificate Chain
To verify a certificate, we follow the issuance chain in a bottom-up manner, back up to the root certificate, ensuring at each step that every certificate has a valid signature, is not expired, and has not been revoked. This approach for establishing trust assumes that the root level CA must be trusted implicitly. The status of root CAs is verified annually by designed agencies; for example, Comodo's annual status as a root CA is verified annually by KPMG.

In the example below, user A can acquire the following certificates from the directory to establish a certification path to C:

$$B \langle \langle F \rangle \rangle \; F \langle \langle X \rangle \rangle \; X \langle \langle Z \rangle \rangle$$

When A has obtained these certificates, it can unwrap the certification path to recover a trusted copy of C's public key. Using this public key, A can send encrypted messages to C. If A wishes to receive encrypted messages back from C, then C will require A's public key, which can be obtained from the following certification path:

$$C \langle \langle G \rangle \rangle \; G \langle \langle Y \rangle \rangle \; Y \langle \langle Z \rangle \rangle$$

![Certificate Verification](/docs/figures/certificate-verification.png)

-----

## Cross Certification
Suppose in the same example as above, but now X and Y are cross certified, that is, F and G have established a trust relationship, by signing each other's public keys. Now A's certification paths with cross certification are:

$$
\begin{aligned}
& B \langle \langle F \rangle \rangle \; F \langle \langle X \rangle \rangle \; X \langle \langle Z \rangle \rangle\\
& B \langle \langle F \rangle \rangle \; F \langle \langle G \rangle \rangle \; G \langle \langle Y \rangle \rangle \;  Y \langle \langle Z \rangle \rangle
\end{aligned}
$$

which means that C only has to go up A's second certification path to find G.
