---
title: Background
html-id: background
---

[MEGA](https://mega.io/) is a cloud storage and collaboration platform founded in 2013 offering <q>secure storage and communication</q> services.
With over 250 million registered users, 10 million daily active users and 1000 PB of stored data, MEGA is a significant player in the consumer domain.
What sets them apart from their competitors such as DropBox, Google Drive, iCloud and Microsoft OneDrive is the claimed security guarantees:
MEGA advertise themselves as <q>the privacy company</q> and promise **U**ser-**C**ontrolled end-to-end **E**ncryption (UCE).

We challenge these security claims and show that an adversarial service provider, or anyone controlling MEGA's core infrastructure, can break the confidentiality and integrity of user data.

### Key Hierarchy

At the root of a MEGA client's key hierarchy, illustrated in [the figure below](#key-hierarchy){: .page-scroll }, is the password chosen by the user.
From this password, the MEGA client derives an authentication key and an encryption key.
The authentication key is used to identify users to MEGA.
The encryption key encrypts a randomly generated master key, which in turn encrypts other key material of the user.  

For every account, this key material includes a set of asymmetric keys consisting of an RSA key pair (for sharing data with other users), a Curve25519 key pair (for exchanging chat keys for MEGA's chat functionality), and a Ed25519 key pair (for signing the other keys).
Furthermore, for every file or folder uploaded by the user, a new symmetric encryption key called a _node key_ is generated.
The private asymmetric keys and the node keys are encrypted by the client with the master key using AES-ECB and stored on MEGA's servers to support access from multiple devices.
A user on a new device can enter their password, authenticate to MEGA, fetch the encrypted key material, and decrypt it with the encryption key derived from the password.

<picture>
    <source media="(max-width: 1000px)" srcset="img/key_hierarchy_portrait.svg">
    <img id="key-hierarchy" alt="MEGA's key hierarchy" class="img-fluid" style="max-width: 100%" src="img/key_hierarchy.svg">
</picture>
