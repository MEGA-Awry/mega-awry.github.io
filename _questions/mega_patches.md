---
html-id: mega-patches
question: Do MEGA's patches protect me?
---

The [mitigation section](#mitigation){: .page-scroll } discusses countermeasures in more detail.

MEGA decided to address our attacks with different measures than those that we proposed.
Their patch is less invasive than our recommendations, but in turn maintains the non-standard use of cryptographic primitives.
It suffices to protect against the [RSA key recovery](#rsa-key-recovery){: .page-scroll } and [plaintext recovery](#pt-recovery){: .page-scroll } attacks.
Hence, it also protects against the chain of attacks up to the [framing attack](#framing-attack){: .page-scroll} that we present here.
(That is, it removes the access to the AES-ECB decryption oracle needed for the framing attack, which we instantiated with the [plaintext recovery attack](#pt-recovery){: .page-scroll }.)

However, their patch does not directly protect against the framing attack, nor the integrity attack.
If the prerequisites can be fulfilled in a different way (see e.g. the discussion on publicly shared files and folders in the [integrity attack](#integrity-attack){: .page-scroll } section), these attacks could still be mounted.
Furthermore, the patch introduced by MEGA does not protect against the [GaP-Bleichenbacher attack](#gap-bleichenbacher){: .page-scroll }, although the latter is arguably too inefficient to be practically exploitable.

We did not analyze whether MEGA's patches can be bypassed or if they introduce new vulnerabilities.
