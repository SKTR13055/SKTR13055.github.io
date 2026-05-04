---
layout: page
title: Certifications
icon: fas fa-certificate # Optional: adds a nice FontAwesome icon if your theme supports it
order: 5 # Optional: change this number to move it up or down in the sidebar
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
