---
title: Security Attacks
markup: "mmark"
weight: 3
---

## Security attacks
A **threat** is a potential for violation of security, which exists when there is a circumstance, capability, action, or event that could breach security and cause harm. That is, a threat is a possible danger that might exploit a vulnerability. An **attack** is an assault on the system security that derives from an intelligent threat; that is, a creative act that is a deliberate attempt to evade security services and violate the security policy of a system. 

------

### Passive attacks
Passive attacks are like eavesdropping on, or monitoring of, transmissions. The goal of the opponent is to obtain information in transmission. Passive attacks are complicated to detect as they do not involve any alteration of data and neither the sender nor receiver is aware that a third party has read the messages or observed the traffic pattern. Therefore, the emphasis in dealing with passive attacks is on prevention rather than detection.

#### Release of message contents
The **release of message contents** is easily understood. For example, an email or telephone conversation may contain sensitive or confidential information, and we would like to prevent an opponent from learning the contents of these transmissions.

#### Traffic analysis
**Traffic analysis** is a more subtle type of passive attack. Suppose that we have masked the contents (typically by encryption) of messages so that even if the opponent was able to capture the message, they could not extract the information from the message. Even with encryption in place, the opponent may still be able to observe the patterns of these messages and determine the location and identity of communicating hosts. It could also be possible to observe the frequency and length of messages being exchanges - this can be useful in guessing the nature of the communication taking place. 

------

### Active attacks
Active attacks involve some modification of the data stream of the creation of a false stream. Active attacks are quite challenging to entirely prevent as there is a wide range of potential physical, software and network vulnerabilities. The goal is to detect active attacks and to recover from any disruption or delays caused by them. If the detection has a deterrent effect, it may also contribute to prevention.

#### Masquerade
A **masquerade** takes place when one entity pretends to be a different entity. A masquerade attack usually includes one of the other forms of attack. For example, authentication sequences can be captured and replaced after a valid authentication sequence has taken place, thus enabling an authorised entity with few privileges to obtain extra privileges by impersonating an entity that has those privileges. 

#### Replay
**Replay** involves the passive capture of a data unit and its subsequent retransmission to produce an unauthorised effect. Since the original messages are intercepted and re-transmitted verbatim, attackers employing replay attacks do not necessarily need to decrypt them. 

#### Modification of messages
**Modification of messages** means that there has been an alteration to some portion of a legitimate message, or that messages are delayed or reordered, to produce an unauthorised effect. 

#### Denial of service
The **denial of service** prevents or inhibits the normal use or management of communications facilities. This attack may have a specific target, for example, an attack may suppress all messages directed to a particular destination (e.g. the security audit service). Another form of service denial is the disruption of an entire network, either by disabling the network or by overloading it with messages to degrade performance. 