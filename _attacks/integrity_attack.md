---
name: Integrity Attack
html-id: integrity-attack
short-description: The impact of this attack is the same as that of the framing attack, trading off less stealthiness for easier pre-requisites.
icon: fa-file-circle-exclamation
---

This attack exploits the peculiar structure of MEGA's obfuscated key objects to manipulate an encrypted node key such that the decrypted key consists of all zero bytes.
Since the attacker now knows the key, this key manipulation can be used to forge a file in a manner similar to the [framing attack](#framing-attack){: .page-scroll}.
Unlike for the framing attack (which requires the ability to decrypt arbitrary AES-ECB ciphertexts), for this attack the adversary only needs access to a single plaintext block and the corresponding ciphertext encrypted with AES-ECB under the master key.
For instance, the attacker can use MEGA's protocol for public file sharing to obtain the pair: when a user shares a file or folder publicly, they create a link containing the obfuscated key in plaintext.
Hence, a malicious cloud provider who obtains such a link knows both the plaintext and the corresponding ciphertext for the node key, since the latter is uploaded to MEGA when the file was created (before being shared).
This can then be used as the starting point for the key manipulation and allows a forged file ciphertext to be uploaded into the victim's cloud, just as for the framing attack.
However, this attack is less surreptitious than the framing attack because of the low probability of the all-zero key appearing in an honest execution, meaning that an observant user who inspects the file keys stored in their account could notice that the attack has been performed.
