---
layout: page
title: Projects
icon: fas fa-code
order: 4
---

Here are the detailed write-ups and explanations for my hands-on security tools and network designs:

<div class="projects-list" style="margin-top: 20px;">
  {% for post in site.categories.Projects %}
    <div class="project-card" style="border: 1px solid #444; border-radius: 10px; padding: 20px; margin-bottom: 25px; background: rgba(0,0,0,0.02); box-shadow: 0 4px 6px rgba(0,0,0,0.05);">
      
      <h3 style="margin-top: 0; margin-bottom: 8px;">
        <a href="{{ post.url }}" style="text-decoration: none;">{{ post.title }}</a>
      </h3>
      
      {% if post.tech %}
        <p style="font-size: 0.9em; color: #888; margin-top: 0; margin-bottom: 12px;">
          <strong>Tech Stack:</strong> {{ post.tech | join: ', ' }}
        </p>
      {% endif %}
      
      <p style="margin-bottom: 15px; line-height: 1.6;">
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>

      <a href="{{ post.url }}" style="font-weight: bold;">Read full write-up &rarr;</a>

    </div>
  {% endfor %}
</div>
