---
name: RSA Key Recovery Attack
html-id: rsa-key-recovery
short-description: MEGA can recover a user's RSA private key by maliciously tampering with 512 login attempts.
icon: fa-key
---
MEGA uses RSA encryption for sharing node keys between users, to exchange a session ID with the user at login and in a legacy key transfer for the MEGA chat.
Each user has a public RSA key $$ pk_{share} $$ used by other users or MEGA to encrypt data for the owner, and a private key $$ sk_{share} $$ used by the user themselves to decrypt data shared with them.
The private RSA key is stored for the user in encrypted form on MEGA's servers.
Key recovery means that an attacker successfully gets possession of the private key of a user, allowing them to decrypt ciphertexts intended for the user.

We discovered a practical attack to recover a user's RSA private key by exploiting the lack of integrity protection of the encrypted keys stored for users on MEGA's servers.
An entity controlling MEGA's core infrastructure can tamper with the encrypted RSA private key and deceive the client into leaking information about one of the prime factors of the RSA modulus during the session ID exchange.
More specifically, the session ID that the client decrypts with the mauled private key and sends to the server will reveal whether the prime is smaller or greater than an adversarially chosen value.
This information enables a binary search for the prime factor, with one comparison per client login attempt, allowing the adversary to recover the private RSA key after 1023 client logins.
Using lattice cryptanalysis, the number of login attempts required for the attack can be reduced to 512.

**Update:**
In July 2022, Keegan Ryan and Nadia Heninger from UC San Diego drastically improved on the key recovery attack described above.
Their attack requires only six login attempts.
From the abstract of their paper ["Cryptanalyzing MEGA in Six Queries"](https://eprint.iacr.org/2022/914){: target="_blank" }:
> Our optimized attack combines several techniques, including a modification of the extended hidden number problem and the structure of RSA keys, to exploit additional information revealed by MEGA’s protocol vulnerabilities.
MEGA has emphasized that users who had logged in more than 512 times could have been exposed; these improved attacks show that this bound was conservative, and that unpatched clients should be considered vulnerable under a much more realistic attack scenario.

### Proof of Concept Attack

Since the server code is not published, we cannot implement a Proof-of-Concept (PoC) in which the adversary actually controls MEGA.
Instead, we implemented a MitM attack by installing a bogus TLS root certificate on the victim.
This setup allows the attacker to impersonate MEGA towards the user while using the real servers to execute the server code (which is unknown to us).
Server responses can be patched on the fly since they do not rely on secrets stored by the server, allowing the attack to be performed in real time as the victim interacts with MEGA.

The following video shows how our attack recovers the first byte of the RSA private key.
Afterward, we abort the attack to avoid any adverse impact on MEGA's production servers.
For each recovered bit, the attack consists of the following steps:
1. The victim logs in.
2. The adversary hijacks the login and replaces the correct session ID with their probe for the next secret bit.
3. Based on the client's response, the adversary updates its interval for the binary search of the RSA prime factor.
4. The login fails for the victim. This is only a limitation of the MitM setup, since the correct SID is lost. An adversarial cloud storage provider can simply accept the returned SID.
{: .text-left }

Note that after aborting the attack, the search interval is `[0xe03ff...f, 0xe07ff...f]`.
The secret prime factor $$ q $$ of the RSA modulus starts with `0xe06...`.
This value is within the search interval and the adversary already recovered the first byte `0xe0`.
Using all of $$ q $$, the adversary can recover the RSA private key $$ (d, N) $$ using the public key $$ (e, N) $$ as $$ p = N / q $$ and $$ d = e^{-1} \mod (p-1)(q-1) $$.

<div class="embed-responsive embed-responsive-16by9">
    <video class="embed-responsive-item" controls muted>
        <source src="videos/rsa_key_recovery_poc.mp4" type="video/mp4">
        Unsupported video tag, please try another browser to view the PoC video.
    </video>
</div>
