---
name: Plaintext Recovery
html-id: pt-recovery
short-description: MEGA can decrypt other key material, such as node keys, and use them to decrypt all user communication and files.
icon: fa-database
---

As shown in the [key hierarchy](#key-hierarchy){: .page-scroll } MEGA clients encrypt the private keys for sharing, chat key transfer, and signing with the master key using AES-ECB.
Furthermore, file and folder keys also use the same encryption.
A plaintext recovery attack lets the adversary compute the plaintext from a given ciphertext.
In this specific attack, MEGA can decrypt AES-ECB ciphertexts created with a user's master key.
This gives the attacker access to the aforementioned and highly sensitive key material encrypted in this way.
With the sharing, chat, signing, and node keys of a user, the adversary can decrypt the victim's data or impersonate them.

This attack exploits the lack of key separation for the master key and knowledge of the recovered RSA private key (e.g., from the [RSA key recovery attack](#rsa-key-recovery){: .page-scroll }).
The decryption oracle again arises during authentication, when the encrypted RSA private key and the session ID (SID), encrypted with the RSA public key, is sent from MEGA's servers to the user.
MEGA can overwrite part of the RSA private key ciphertext in the SID exchange with two target ciphertext blocks.
It then uses the SID returned by the client to recover the plaintext for the two target blocks.
Since all key material is protected with AES-ECB under the master key, an attacker exploiting this vulnerability can decrypt node keys, the victim's Ed25519 signature key, its Curve25519 chat key, and, thus, also all exchanged chat keys.
Given that the private RSA key has been recovered, one client login suffices to recover 32 bytes of encrypted key material, corresponding, for instance, to one full node key.
