---
layout: page
title: Projects
icon: fas fa-code
order: 4
---

Here are the detailed write-ups and explanations for my hands-on security tools and network designs:

<div class="projects-list">
  {% for post in site.categories.Projects %}
    <div class="project-item" style="margin-bottom: 30px; padding-bottom: 20px; border-bottom: 1px solid #e1e4e8;">
      
      <h3 style="margin-bottom: 5px;">
        <a href="{{ post.url }}">{{ post.title }}</a>
      </h3>
      
      {% if post.tech %}
        <p style="font-size: 0.9em; color: #6a737d; margin-top: 0; margin-bottom: 10px;">
          <strong>Tech Stack:</strong> {{ post.tech | join: ', ' }}
        </p>
      {% endif %}
      
      <p style="margin-bottom: 10px; line-height: 1.5;">
        <!-- This pulls the first paragraph of your write-up to act as a preview summary -->
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>

      <a href="{{ post.url }}">Read full write-up &rarr;</a>

    </div>
  {% endfor %}
</div>