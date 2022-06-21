---
title: Mitigation 
html-id: mitigation
---

### Responsible Disclosure
{: #disclosure }

We contacted MEGA to inform them of the vulnerabilities in their system and to suggest three different levels of mitigation (immediate, minimal, and recommended) on March 24, 2022.
MEGA acknowledged the attacks, confirmed that the system is vulnerable and needs patching, and awarded us a bug bounty.
We agreed to a 90-day disclosure window.

### Our Proposed Countermeasures
{: #our-mitigation }

The scale of MEGA poses many practical challenges for both MEGA and customers, such as the load and traffic implications of re-encrypting over 1000 petabytes of user data.
Furthermore, backwards compatibility is a significant issue as disruptive changes render some customer data inaccessible, while legacy code is a security risk.

Due to these significant practical challenges, we organize our proposed mitigations in three levels:
1. [Immediate countermeasures](#imm-countermeasures){: .page-scroll }: Backwards compatible and non-invasive mitigations which complicate the attacks sufficiently to temporarily protect users from the most severe security breaches.
2. [Minimal countermeasures](#min-countermeasures){: .page-scroll }: More substantial changes that better address the attacks while avoiding the most expensive modifications (such as file re-encryption).
3. [General recommendations](#general-recs){: .page-scroll }: Long-term goals for redesigning the cryptographic architecture to address the root causes of the attacks and adhere to best practices.
{: .text-left }

A visual overview of how the proposed mitigation steps change the key hierarchy is [here](img/mitigation_key_hierarchy.png){: target="_blank" }.

#### Immediate Countermeasures
{: #imm-countermeasures }

- _Key separation for the master key:_ following the key separation principle, we propose to introduce a derivation key and derive different keys to protect the RSA Sharing, Chat, Sign, and Node Keys, instead of using a single master key to encrypt them all.
- _Ad hoc integrity protection:_ we propose to use HMAC on top of the existing ciphertexts to protect the integrity of key ciphertexts.
- _Fixing the RSA padding prefix:_ we propose to enforce stricter client-side checks of MEGA's RSA padding scheme to increase the number of queries needed for the [GaP-Bleichenbacher attack](#gap-bleichenbacher){: .page-scroll} and further reduce the practicality of the attack.
{: .text-left}

These measures protect against our first four attacks.
However, they should only be considered as temporary measures that can be deployed quickly.
They do not address the root flaws in the cryptographic design.

#### Minimal Countermeasures
{: #min-countermeasures }

- _AES-GCM for key ciphertexts:_ instead of the ad hoc integrity protection proposed as part of the immediate countermeasures, we suggest replacing AES-ECB with an authenticated encryption scheme, namely AES-GCM.
- _RSA-OAEP for sharing:_ we recommend replacing MEGA's padding scheme by a standardized padding mechanism for RSA encryption. Additionally, we suggest using different RSA key pairs for sharing node keys and for the legacy chat key transfer to improve key separation.
{: .text-left }

These measures address our attacks more adequately at the cost of losing backwards compatibility.

#### General Recommendations
{: #general-recs }

- _File encryption:_ we propose a thorough redesign of MEGA's system. This includes protecting node keys with AES-GCM and using separate keys for file encryption and for protecting their attributes. Furthermore, we propose to replace MEGA's custom variant of AES-CCM for file encryption with the well-analyzed AES-GCM.
- _Augmented PAKE for authentication:_ we recommend the usage of [OPAQUE](https://eprint.iacr.org/2018/163.pdf) to avoid targeted dictionary attacks on user passwords by MEGA.
{: .text-left }

The proposed measures in this section require users to re-encrypt their files because the key material is derived differently, and new primitives are used.
According to our estimates, this process would take more than half a year, even in the ideal setting where all customers are reachable and have the computational resources to re-encrypt up to hundreds of gigabytes of data.
While this needs to be planned carefully, the long-term goal should, in our opinion, be to replace insecure legacy code and temporary patches.
The immediate and minimal countermeasures are only temporary suggestions and are not unlikely vulnerable to more attacks themselves due to the extensive key reuse and non-standard combination of primitives.


### MEGA's Patch
{: #mega-mitigation }

MEGA decided to introduce additional client-side checks on the format of RSA private keys to protect against our first attack.
They are explained in more detail in [MEGA's blog post](https://blog.mega.io/mega-security-update/){: target="_blank" }.
While these checks directly prevent the RSA key recovery attack, and hence by extension the attacks that depend on it, this fix significantly differs from our proposed countermeasures.

