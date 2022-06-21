---
name: Framing Attack
html-id: framing-attack
short-description: MEGA can insert arbitrary files into the user's file storage which are indistinguishable from genuinely uploaded ones.
icon: fa-file-import
---

This attack allows MEGA to forge data in the name of the victim and place it in the target's cloud storage.
While the previous attacks already allow an adversary to modify existing files using the compromised keys, this attack allows the adversary to preserve existing files or add more documents than the user currently stores.
For instance, a conceivable attack might frame someone as a whistle-blower and place an extensive collection of internal documents in the victim's cloud storage.
Such an attack might gain credibility when it preserves the user's original cloud content.

To create a new file key, the attacker makes use of the particular format that MEGA uses for node keys.
Before they are encrypted, node keys are encoded into a so called <q>obfuscated key</q> object, which consists of a reversible combination of the node key and information used for decryption of the file or folder.
(Specifically, a nonce and a truncated MAC tag.)
Since none of our attacks allow an attacker to encrypt values of their choosing with AES-ECB under the master key, the attacker works in the reverse direction to create a new valid obfuscated key object.
That is, it first obtains an obfuscated key by decrypting a randomly sampled key ciphertext using the [plaintext recovery attack](#pt-recovery){: .page-scroll }.
Note that this ciphertext can really be randomly sampled from the set of all bit strings of the length of one encrypted obfuscated key object.
Decryption will always succeed, since the AES-ECB encryption mode used to encrypt key material does not provide any means of checking the integrity of a ciphertext.
The resulting string is not under the control of the adversary and will be random-looking, but can regardless be used as a valid obfuscated key object since both file keys and the integrity information (nonces and MAC tags) can be arbitrary strings.
Hence, the adversary parses the decrypted string into a file key, a nonce and MAC tag.
It then modifies the target file which is to be inserted into the victim's cloud such that it passes the integrity check when the file key and integrity information from the obfuscated key is used.
The attacker achieves this by inserting a single AES block in the file at a chosen location.
Finally, the adversary uses the known file key to encrypt the file and uploads the result to the victim's cloud.

![Forged image](img/forged_img.png){: .fluid-img style="width: 100%; max-width: 624px;" id="forged-img"}

Many standard file formats such as PNG and PDF tolerate 128 injected bits (for instance, in the file's metadata, as trailing data, or in unused structural components) without affecting the displayed content.
The image above shows the file our proof of concept inserts in the victim account.
Our attack added the highlighted bytes to satisfy the integrity check.
Due to the structure of PNG files, these bytes do not change the displayed pixels, i.e., it is visually identical to the unmodified image.


<br>

### Proof-of-Concept Attack

The PoC shows our framing attack in the MitM setting described in the [RSA private key recovery attack](#rsa-key-recovery){: .page-scroll }.

The video shows the following steps:
1. The victim logs into their account without any attack running. There are only three files in the cloud storage and none of them is called `hacker-cat.png`.
2. The victim logs out and the adversary runs the [plaintext recovery attack](#pt-recovery){: .page-scroll } once. This involves the following steps:
    - The adversary hijacks the victim's login attempt and replaces the encrypted SID with the encrypted key that it picked for the file forgery.
    - The victim's client responds with the decrypted SID, from which the adversary can recover the plaintext for the injected ciphertext blocks.
    - The log in attempt fails, which is only a limitation of the MitM setting. A malicious cloud provider can perform this attack without the user noticing.
3. The adversary uses the key recovered in the previous step to forge an encrypted file.
4. The victim logs in again.
5. The adversary injects the forged file into the loaded file tree.
6. The victim's cloud now displays four files, including a new file called `hacker-cat.png`.
7. When the user views this file in the browser, it correctly decrypts and shows the image.
{: .text-left }

<div class="embed-responsive embed-responsive-16by9">
    <video class="embed-responsive-item" controls muted>
        <source src="videos/framing_attack_poc.mp4" type="video/mp4">
        Unsupported video tag, please try another browser to view the PoC video.
    </video>
</div>
