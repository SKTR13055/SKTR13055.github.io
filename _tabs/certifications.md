---
layout: page
title: Certifications
icon: fas fa-certificate
order: 5 
---

Here is a comprehensive list of my professional certifications, training, and challenge completions:

<ul>
  {% for cert in site.data.certifications %}
    <li style="margin-bottom: 10px;">
      <strong>
        {% if cert.url and cert.url != "" %}
          <a href="{{ cert.url }}" target="_blank" rel="noopener noreferrer">{{ cert.name }}</a>
        {% else %}
          {{ cert.name }}
        {% endif %}
      </strong> 
      — {{ cert.issuer }} <span style="color: #6a737d;">({{ cert.date }})</span>
    </li>
  {% endfor %}
</ul>
