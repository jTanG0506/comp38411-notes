---
title: Needham Schroeder Protocol
markup: "mmark"
weight: 4
---

## Needham-Schroeder Protocol
Suppose a party $$A$$ wishes to establish a secure communication link another party $$B$$, and that both parties $$A$$ and $$B$$ possess master keys $$K_A$$ and $$K_B$$, respectively, for communicating with a shared key distribution centre.  $$A$$ engages in the Needham-Schroeder Protocol as follows:

1. $$A$$ sends a request to the KDC for a session key intended for communicating with $$B$$. The message sent by $$A$$ to the KDC is encrypted using $$K_A$$ and includes $$A$$'s network address $$(ID_A)$$, $$B$$'s network address $$(ID_B)$$ and a unique session identifier, known as a nonce, denoted by $$N_A$$. This message can be expressed as $$E(K_A, ID_A \; || \; ID_B \; || \; N_A)$$.
1. The KDC responds to $$A$$ with a message encrypted using $$K_A$$, which contains
  - the session key $$K_{AB}$$ that $$A$$ can use for communicating to $$B$$
	- the original message received from $$A$$, so that $$A$$ can match the response received from the KDC with the request sent, as $$A$$ may be trying to establish multiple simultaneous sessions with $$B$$
	- a ticket which is meant for $$A$$ to be sent to $$B$$, which contains the session key $$K_{AB}$$ and $$A$$'s identifier $$K_A$$. This ticket is encrypted using $$B$$'s master key $$K_B$$, so $$A$$ cannot look inside the contents of this ticket.
This message can be expressed as $$E(K_A, K_{AB} \; || \; ID_A \; || \; ID_B \; || \; N_A \; || \; E(K_B, K_{AB} \; || \; ID_A))$$.
1. $$A$$ decrypts the message received from the KDC, using the master $$K_A$$. $$A$$ knows that the message received is indeed from the KDC as only $$A$$ and the KDC have access to the master key $$K_A$$. $$A$$ keeps the session key $$K_{AB}$$ and sends the ticket intended for $$B$$ to $$B$$. This message is sent unencrypted by $$A$$, but since it was previously encrypted by the KDC using $$B$$'s master key, $$K_B$$, the first contact from $$A$$ to $$B$$ is protected from eavesdropping. That is, $$A$$ sends $$E(K_B, K_{AB} \; || \; ID_A)$$ to $$B$$.
1. $$B$$ decrypts the message received from $$A$$ using the master key $$K_B$$, and compares the $$ID_A$$ in the decrypted message with the sender identifier associated with the message received from $$A$$, so $$B$$ can be certain that nobody is masquerading as $$A$$. $$B$$ now also have the session key $$K_{AB}$$ for communicating securely with $$A$$. $$B$$ sends $$A$$ another nonce, $$N_B$$, encrypted using the session key $$K_S$$. That is, $$B$$ sends $$E(K_{AB}, N_B)$$ to $$A$$.
1. Upon receipt, $$A$$ decrypts the message and sends back $$N_B - 1$$, encrypted using the same session key. This acts as a confirmation that the session key $$K_S$$ works for the ongoing session, and ensures that $$B$$ knows that it did not receive a first contact from $$A$$ that $$A$$ is no longer interested in. That is, $$A$$ sends $$E(K_{AB}, N_B - 1)$$ to $$B$$.

![Needham Schroeder](/docs/figures/needham-schroeder.png)
