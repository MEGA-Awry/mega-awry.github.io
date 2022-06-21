---
html-id: int-framing-difference
question: Both the framing and integrity attack forge files &mdash; what's the difference between them?
---

They differ in the assumed attacker capabilities and the stealthiness of the created files.

Framing attack:
- _Capabilities:_ the adversary needs access to an AES decryption oracle, i.e., it needs to be able to decrypt arbitrary ciphertexts encrypted with AES-ECB under the master key. (The [plaintext recovery attack](#pt-recovery){: .page-scroll }, provides exactly this.)
- _Stealthiness:_ the resulting file encryption uses a uniformly random key just like genuinely uploaded files.

Integrity attack:
- _Capabilities:_ the adversary only needs to know a single ciphertext-plaintext pair for AES-ECB under the master key. This is easier to achieve than a decryption oracle.
- _Stealthiness:_ the encrypted file uses a key of all zero bytes, which is very unlikely to happen for genuinely uploaded files. However, the user needs some technical expertise to extract the decrypted file key from their MEGA client in order to detect the attack.
