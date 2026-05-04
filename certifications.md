---
layout: page
title: Certifications
permalink: /certifications/
---

Here are my professional certifications and training:

<ul>
  {% for cert in site.data.certifications %}
    <li>
      <strong><a href="{{ cert.url }}" target="_blank" rel="noopener noreferrer">{{ cert.name }}</a></strong> 
      — {{ cert.issuer }} ({{ cert.date }})
    </li>
  {% endfor %}
</ul>
