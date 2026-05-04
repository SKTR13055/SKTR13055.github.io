---
layout: page
title: Certifications
icon: fas fa-certificate
order: 5 
---

Here are my professional certifications and training:

<ul>
  {% for cert in site.data.certifications %}
    <li>
      <strong>
        {% if cert.url and cert.url != "" %}
          <a href="{{ cert.url }}" target="_blank" rel="noopener noreferrer">{{ cert.name }}</a>
        {% else %}
          {{ cert.name }}
        {% endif %}
      </strong> 
      — {{ cert.issuer }} ({{ cert.date }})
    </li>
  {% endfor %}
</ul>
