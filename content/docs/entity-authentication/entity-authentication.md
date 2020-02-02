---
title: Entity Authentication
markup: "mmark"
weight: 1
---

## Entity Authentication
**Entity authentication** is the process whereby one party is assured of the identity of a second party, through corroborative evidence provided by the second party. Suppose $$A$$ wishes to verify the identity of $$B$$; then we will refer to $$A$$ as the *verifier* and B as the *prover* or *claimant*.

One of the primary purposes of entity authentication is to facilitate access control to a resource when an access privilege is linked to a particular identity; this is known as **authorisation**. The process of verifying the identity of a claimant is known as **authentication**, and this identity can be used for **accounting** by logging security-related events in an audit trail. These three services are commonly referred to as the *AAA services*.

-----

### Basis of identification
There are several techniques we can use for identifying a user, such as:
1. **Something known**: such as standard passwords, Personal Identification Numbers (PINs), and demonstrating knowledge of a private key in a challenge-response protocol.
1. **Something possessed**: a physical accessory, such as a magnetic-striped card, chip card, and hand-held password generators.
1. **Something inherent**: properties which make use of human physical characteristics, such as handwritten signatures, fingerprints, voice and retinal patterns.

Some of the properties that may help us choose which form of entity authentication is most suitable include:
1. **Computation Efficiency**: how many operations are required to execute the protocol?
1. **Communication Efficiency**: how message exchanges do we need? What is the total number of bits required to be transmitted?
1. **Involvement of a third party**: do we require a trusted third party to use this protocol?
1. **Storage of secrets**: what methods does this protocol require for storing keying material?
