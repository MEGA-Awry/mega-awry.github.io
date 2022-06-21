---
html-id: apply-to-others
question: Can your attacks be applied to other services?
---

The attacks rely on details of the architecture and implementation of MEGA.
Therefore, they cannot be directly applied to other services.

Nevertheless, some of our observations are generalizable.
For instance, the [RSA key recovery attack](#rsa-key-recovery){: .page-scroll } shows that the missing integrity protection of ciphertexts does not only fail to provide IND-CCA security but can also lead to powerful attacks in practice.
Other services with malleable key ciphertexts might be vulnerable to similar attacks.

Moreover, the [GaP-Bleichenbacher attack](#gap-bleichenbacher){: .page-scroll } is a new variant of Bleichenbacher's attack on PKCS#1 v1.5 that makes the attack applicable to other custom implementations of the RSA padding.
