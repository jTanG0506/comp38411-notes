---
title: UNIX Password-Based Authentication
markup: "mmark"
weight: 2
---

## UNIX Password-Based Authentication
The `UNIX` password file contains a one-way function of user passwords, rather than plaintext passwords, for reasons such as theft and accidental disclosure. Each user password serves as the key to encrypt a known plaintext (64 zero-bits), and this yields a one-way function of the key since only the user knows the password.

The one-way function, `crypt()` makes repeated use of DES, iterating the encipherment $$t = 25$$ times. A user's password is truncated to its first 8 ASCII characters, and each character provides 7 bits for a 56-bit DES key (padded with 0-bits if less than 8 characters). The key is used to encrypt the 64-bit constant 0 using DES, with the output fed back as input $$t$$ times iteratively. The 64-bit result is unpacked into a string of 11 printable characters.

`crypt()` uses a non-standard method of password salting, to preclude the use of off-the-shelf DES hardware for attacks:
1. **Password Salting**: `UNIX` passwords make use of a salt based on the system clock at the time of password created for each user-selected password. This salt is used to alter the standard expansion function of DES, providing one of 4096 variations. That is, a dictionary attack now requires 4096 variations of each trial password.
1. **Preventing use of off-the-shelf DES chips**: As the DES expansion permutation is dependent on the salt used, standard DES chips can no longer to be used to implement the `UNIX` password algorithm, thus deterring adversaries that wish to use hardware to speed up an attack.

When a user tries to log in, the program `/bin/login` takes the password input by the user and the salt from the password file, to generate a fresh encrypted password, and compares this password to the one stored in the `/etc/passwd` file. If the two encrypted results match, the user is authenticated.

-----

### UNIX Password Formats
Most modern implementations of `UNIX` support hashing algorithms in addition to the legacy DES-based `crypt()` function.

![UNIX Password Format](/docs/figures/unix-password-format.png)

 The modern password format takes the form `$id$salt$hashedpwd`. The ID in the modern encrypted password format indicates the algorithm used to produce the hash; `$1$` is MD5, `$2a$` and `$2y$` are Blowfish, `$5$` is SHA-256 and `$6$` is SHA-515.

-----

### Password File
`UNIX` uses a `/etc/passwd` file to keep track of every user on the system. An entry in this file contains the username, real name, and identification information for each user.

An example entry in the `/etc/passwd` looks like:

`jtang:$$1$$ad07f91a4a9f...:506:506:Jonathan Tang:/home/jtang:/bin/bash`

-----

### Shadow File
As the `/etc/passwd` file needs to be used by many processes, this file needs to be readable to anyone, which can pose a security risk. To counter this, a shadow password file `/etc/shadow` contains the encrypted passwords and is only accessible by the root account. The corresponding password field is replaced with a single `x` character to indicate that a shadow file is in use. This approach only reduces the security risk compared to storing it directly in the `/etc/passwd` file, but it is still vulnerable if an attack is based on processes with root privileges.

When we use shadow passwords, an entry in `/etc/passwd` looks like:

`jtang:x:506:506:Jonathan Tang:/home/jtang:/bin/bash`

and the corresponding entry in `/etc/shadow`:

`jtang:$$1$$ad07f91a4a9f...:17707:0:90:14:::`

-----

### Replay Attacks
Recall that a replay attack is an impersonation based on a single previous protocol execution, that is, an attacker can eavesdrop on a network to get your login credentials and later replay it to gain access to the network. In order to perform this attack, the adversary needs to modify the client so that instead of encrypting the encrypted password, but rather replay it directly, and eavesdrop on the network or gain access to the password file.

However, we usually assume that the LAN is secure, in the sense that eavesdropping can be noticed, and that users are not able to bring their own client software in. Therefore, we usually overlook the possibilities of replay attacks in LAN environments.
