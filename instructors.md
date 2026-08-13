---
layout: page
title: Instructors
nav_order: 4
description: A listing of all the course instructors.
---

# Instructors

{% assign instructors = site.staffers | where: 'role', 'Instructor' %}
<div class="staffer-grid">
{% for staffer in instructors %}
{{ staffer }}
{% endfor %}
</div>
