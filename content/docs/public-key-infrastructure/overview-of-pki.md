---
title: Overview of Public Key Infrastructure
markup: "mmark"
weight: 1
---

## Overview of Public Key Infrastructure
Public Key Infrastructure (PKI) is the combined set of protocols, hardware, software, user roles, policies and procedures used to manage asymmetric key information across a network, which enable practical deployment and wide-scale application of public key cryptography.

-----

### Components of a PKI
- **End entity**: the end-users, devices, or any other entity that can be identified in the subject field of a public-key certificate.
- **Certification Authority (CA)**: the issuer of certificates and certificate revocation lists (CRLs). It may also support a range of administrative functions, but these are usually delegated to a Registration Authority.
- **Registration Authority (RA)**: an optional component that manages a number of administrative functions from the CA.
- **CRL issuer**: an optional component that a CA can delegate to publish CRLs.
- **Repository**: a data store for certificates and CRLs so that end entities can retrieve them. 

-----

### Main functions of PKI
- **Registration**: the process whereby a subject makes itself known to a CA, either directly or through an RA, prior to that CA issuing a certificate for that subject. This process usually involves some offline or online procedure for mutual authentication, and the end entity is issued one or more shared secret keys used for subsequent authentication.
- **Initialisation**: the process in which a credential service provider, usually the CA, prepares the policies, procedures, and services ready. This includes setting up the key generation and update services, certificate issuance, distribution and revocation procedures, and potential interaction with other providers, namely a registration authority or other CAs.
- **Certification**: the process in which a CA issues a certificate for a subject's public key, returns that certificate to the subject's client system, and posts that certificate in a repository.
- **Key Pair Recovery**: the process of allowing a subject to restore their encryption / decryption key pair from an authorised key backup facility. Loss of access to the decryption key can result from forgotten passwords, corrupted storage mediums, damaged hardware tokens, and so on.
- **Key Pair Update**: the process which allows key pairs to be updated and new certificates to be issued. An update is required when the certificate lifetime expires and as a result of certificate revocation.
- **Revocation Request**: the process in which an authorised subject can advise a CA to revoke a certificate. For example, when the associated private key is believed to be compromised, or name change.
- **Cross Certification**: the process in which pairs of CA's exchange information to establish a trust relationship, by signing each other's public keys in a certificate known as a cross-certificate.
