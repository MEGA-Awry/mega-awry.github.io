---
html-id: aes-enough
question: MEGA uses AES for End-to-End Encryption (E2EE), why isn't that enough?
---

Unfortunately, the use of secure primitives alone does not guarantee security.
The overall system design is crucial.
For instance, this includes key derivation and management, the support of advanced features (like file-sharing or a chat), and how the primitives are used and combined.

Our attacks exploit unintended interactions between ciphertexts resulting from key reuse.
Furthermore, MEGA uses the ECB mode of AES for some encryptions, which does not protect the integrity of ciphertexts.
