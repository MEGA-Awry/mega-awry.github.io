---
---

{% assign today = site.time | date: '%s' %}
{% assign go_live_date = '2022-06-21T23:00:00Z' | date: '%s' %}
{% assign secs = go_live_date | minus: today %}
{% assign hrs = secs | divided_by: 3600 %}
{% assign mins = secs | divided_by: 60 | modulo: 60 %}

<style>

.middle {
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;

    font-size: 3em;

    width: 100%;
    height: 100%;
}

</style>

<body>
    <div class="middle">
        <p>
            Will be published in {{ hrs }} hours {{ mins }} min.
        </p>
    </div>
</body>

