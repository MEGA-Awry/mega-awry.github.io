---
---

{% assign go-live-date = '2022-06-21T23:00:00Z' | date '%s' %}
{% assign diff = go-live-date | minus: today %}

Will be published in {% diff | divided_by: 3600 %}:{% diff | divided_by: 60 %}.

