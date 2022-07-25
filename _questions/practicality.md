---
html-id: practicality
question: Are your attacks practical?
---

Yes, sufficiently motivated attackers could perform the [RSA key recovery attack](#rsa-key-recovery){: .page-scroll }.

The bottleneck of the attack is the required 512 user login attempts.
If users frequently log out of their account, the attack could recover their RSA private key within a few months.

However, users stay logged in by default on the MEGA clients that we examined.
Nevertheless, as the attacker in our threat model controls the core infrastructure of MEGA, they could make the attack much more practical with an inconspicuous code update.
Currently, MEGA clients cache the old session ID and user's secret key material by default.
Therefore, a new SID is only exchanged when users manually terminate their session and re-enter their password.
An adversary could push an update to MEGA's clients such that they transparently re-negotiate a new session ID (SID) on every new access.
Afterwards, the adversary can perform the 512 queries within hours or days without the user noticing.

The adversary can use a recovered RSA key to perform the [plaintext recovery](#pt-recovery){: .page-scroll}, [framing](#framing-attack){: .page-scroll}, and [integrity](#integrity-attack){: .page-scroll } attacks.

The [integrity attack](#integrity-attack){: .page-scroll } can additionally be performed without the RSA key if the attacker knows a single AES-ECB plaintext-ciphertext pair under the master key.
Such a pair can for example be obtained by MEGA from a link to a publicly shared user file or folder.

Finally, the [GaP-Bleichenbacher attack](#gap-bleichenbacher){: .page-scroll} is of a more theoretical nature since $$ 2^{16.9} $$ interactions with the client are very expensive.
However, attacks only improve over time, and this attack shows a fundamental flaw in the use of RSA encryption.

### Update [July 2022]

In ["Cryptanalyzing MEGA in Six Queries"](https://eprint.iacr.org/2022/914){: target="_blank" }, Keegan Ryan and Nadia Heninger from UC San Diego published an improved key recovery attack which reduces the number of required login attempts to six.
This improvement not only makes recovering the RSA key easy to perform for an adversary, but also makes our [plaintext recovery](#pt-recovery){: .page-scroll}, [framing](#framing-attack){: .page-scroll}, and [integrity](#integrity-attack){: .page-scroll } attacks feasible.
