---
title: Security Models
markup: "mmark"
weight: 6
---

## Security models

### Distributed system security model
![Distributed System Security Model](/docs/figures/distributed-system-security-model.png)

In a distributed system security model, there is a wide range of security threats, some of which include:
- How does the server know who is making the request? Clients should be assigned a unique identifier so that they can be identified, which is useful for logging requests
- Can the server be certain about the identity of the principal behind the invocation? How can we assure that the client is whom they claim to be?
- Is the client allowed to perform the requested actions on the server? Access to objects on the server should be controlled using *access rights*
- Are the messages secured during transit? If not, a perpetrator could read, copy, alter or inject messages as they travel across the network.
- Is the result received the client indeed from the server process? Could it be that the response was modified before it reached the client?

------

### eCommerce security model
![eCommerce Security Model](/docs/figures/ecommerce-security-model.png)

In this e-commerce security model, the opponent is a misbehaving insider. The customer could be making a valid dispute, for example, claiming that their untracked order was never received and it could be the case the order was lost in transit or never dispatched in the first place. Conversely, the customer could be making a fraudulent claim by taking advantage of the fact that the parcel was untracked and claiming the order never arrived, even though it did.

The role of the trusted third party (such as an arbitrator) is to resolve the dispute using evidence generated by non-repudiation services, for example, checking with the courier that they did indeed receive the parcel from the merchant and its destination was the customer.

------

### Communication security model
![Communication Security Model](/docs/figures/communication-security-model.png)

In this communication security model, the emphasis is on protecting the data while in transit between the two parties (*principles*). We wish to protect the information transmission from an opponent who may present a threat to confidentiality, authenticity, and so on.

The security-related transformation in this example would be the encryption of the message so that it is unreadable by the opponent, in addition to a code generated based on the contents of a message, which can be used to verify the identity of the sender. The two principals share some secret information, which is hopefully unknown to the opponent. In this example, the secret would be the keys used to encrypt before transmission and decrypt the message on reception.

Sometimes we may require a trusted third party to achieve secure transmission. For example, the third-party may be responsible for distributing the secret information to the two principals while keeping it away from any opponent. The third party may also be needed to arbitrate disputes between the two principals concerning the authenticity of a message transmission.

------

### Network security model
![Network Security Model](/docs/figures/network-security-model.png)

In this model, the focus is on protecting data and services on a network against external attacks or unauthorised usage. On the network perimeter, we could use firewalls to specify what traffic is allowed to pass through it. 

A network could employ an intrusion detection system (IDS), which is essentially an alarm system that is used to detect and alert on suspicious activity. This system can be built from a single device or a collection of sensors placed at strategic points in a network. Further, a network could also employ an intrusion prevention system (IPS) which attempts to defend the target without the administrator's direct intervention automatically.

The host could make use of an authorisation server, whose role is to issue access tokens to the client after successfully authenticating the resource owner and obtaining authorisation. The host should also contain screening logic that is designed to detect and reject worms, viruses, and other similar attacks.