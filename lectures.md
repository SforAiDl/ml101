---
layout: page
title: Lectures
nav_order: 2
description: Listing of course modules and topics.
---

# Lectures

{% for module in site.modules %}
{{ module }}
{% endfor %}
