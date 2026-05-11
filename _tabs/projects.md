---
layout: page
title: Projects
icon: fas fa-code
order: 4
---

Here are the detailed write-ups and explanations for my hands-on security tools and network designs:

<div class="projects-list">
  {% for project in site.projects %}
    <div class="project-item" style="margin-bottom: 30px; padding-bottom: 20px; border-bottom: 1px solid #e1e4e8;">
      
      <h3 style="margin-bottom: 5px;">
        <a href="{{ project.url }}">{{ project.title }}</a>
      </h3>
      
      {% if project.tech %}
        <p style="font-size: 0.9em; color: #6a737d; margin-top: 0; margin-bottom: 10px;">
          <strong>Tech Stack:</strong> {{ project.tech | join: ', ' }}
        </p>
      {% endif %}
      
      <p style="margin-bottom: 10px; line-height: 1.5;">
        <!-- This pulls the first paragraph of your write-up to act as a preview summary -->
        {{ project.excerpt | strip_html | truncatewords: 30 }}
      </p>

      <a href="{{ project.url }}">Read full write-up &rarr;</a>

    </div>
  {% endfor %}
</div>
