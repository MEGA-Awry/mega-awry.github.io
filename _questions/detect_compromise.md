---
html-id: detect-compromise
question: Can I detect whether any of your attacks was exploited on my account?
---

No, almost all of our attacks do not leave persistent traces.
Only the integrity attack may result in files with an unusual structure.
However, this is not a guaranteed indicator of compromise and could be further hidden.

After researchers from UCSD published an [improvement](https://eprint.iacr.org/2022/914){: target="_blank" } over our key recovery attack which requires only six client login attempts, this attack could have been performed by anyone controlling MEGA's infrastructure without the users noticing.
This makes the [plaintext recovery](#pt-recovery){: .page-scroll}, [framing](#framing-attack){: .page-scroll}, and [integrity](#integrity-attack){: .page-scroll } attacks feasible since their pre-requisite is only a compromised RSA private key.
