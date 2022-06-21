---
html-id: further-vulns
question: Could there be further vulnerabilities?
---

Yes, with a system as large and complex as MEGA's, there is always a possibility for vulnerabilities.
Furthermore, our analysis showed that MEGA uses non-standardized cryptographic primitives in unusual ways in several places in their system.
Some of these might be vulnerable to attacks, although we did not exploit them in the attacks presented here.

In the [mitigation section](#mitigation){: .page-scroll } we outline the changes to MEGA's cryptographic architecture that we consider advisable.
Our recommendations include a transition to well-analyzed and standardized algorithms and would ensure that MEGA follows current best practices.
Implementing these changes would give confidence that no further vulnerabilities are possible that, e.g., exploit interactions between primitives due to key reuse.

However, some of the proposed changes would be very expensive to perform in practice.
Please refer to the [section on MEGA's patch](#mega-mitigation){: .page-scroll } for information on the changes implemented by them.
