---
html-id: change-pw
question: Does changing my password ensure I am safe?
---

Not entirely.
Changing the password will derive new authentication and encryption keys for your account.
However, your master key will not be updated, only re-encrypted with the new password.
Furthermore, existing asymmetric keys, including the RSA private key that could have been recovered using our [first attack](#rsa-key-recovery){: .page-scroll }, as well as the symmetric file and folder keys are not changed.
We are not aware of any functionality to re-new these keys using MEGA's standard clients.
