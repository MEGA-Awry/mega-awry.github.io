---
name: GaP-Bleichenbacher Attack
html-id: gap-bleichenbacher
short-description: MEGA can decrypt RSA ciphertexts using an expensive padding oracle attack.
icon: fa-eye
---

This attack is a novel variant of [Bleichenbacher's attack on PKCS#1 v1.5 padding](https://doi.org/10.1007/BFb0055716).
We call it a _Guess-and-Purge_ (GaP) Bleichenbacher attack.
MEGA can use this attack to decrypt RSA ciphertexts by exploiting a padding oracle in the fallback chat key exchange of MEGA's chat functionality.
The vulnerable RSA encryption is used for sharing node keys between users, to exchange a session ID with the user at login and in a legacy key transfer for the MEGA chat.
As shown in the [key hierarchy](#key-hierarchy){: .page-scroll }, each user has a public RSA key used by other users or MEGA to encrypt data for the owner, and a private key used by the user themselves to decrypt data shared with them.
With this attack, MEGA can decrypt these RSA ciphertexts, albeit requiring an impractical number of login attempts.

**Attack idea:**  
The original Bleichenbacher attack maintains an interval for the possible plaintext value of a target ciphertext.
It exploits the malleability of RSA to test the decryption of multiples of the unknown target message.
Successful unpadding leaks the prefix of the decrypted message due to the structure of the PKCS#1 v1.5 padding.
This prefix allows an adversary to reduce intervals efficiently and recover the plaintext.

MEGA uses a custom padding scheme which is less rigid than PKCS#1 v1.5.
This makes it challenging to apply Bleichenbacher's attack, because a successful unpadding no longer corresponds to a single solution interval.
Instead many disjoint intervals are possible, depending on the value of an unknown prefix in MEGA's padding scheme.
Our attack devices a new Guess-and-Purge strategy that _guesses_ the unknown prefix and quickly _purges_ wrong guesses.
This enables us to perform a search for the decryption of a ciphertext in $$ 2^{16.9} $$ client login attempts on average.

Although this attack is weaker than the [RSA key recovery attack](#rsa-key-recovery){: .page-scroll } (in the sense that a key recovery implies plaintext recovery), it is complementary in the vulnerabilities that it exploits and requires different attacker capabilities.
It attacks the legacy chat key decryption of RSA encryption instead of the session ID exchange and can be performed by a slightly weaker adversary since no key-overwriting is necessary.

The details of the GaP-Bleichenbacher attack are intricate.
For a full description, see Appendix B of [the paper](#paper){: .page-scroll }.
