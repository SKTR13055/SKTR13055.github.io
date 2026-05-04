---
layout: page
title: Certifications
icon: fas fa-certificate
order: 5 
---

Here are my professional certifications, training, and challenge completions:

<div class="cert-grid">
  {% for cert in site.data.certifications %}
    <div class="cert-card">
      
      <!-- Display the image if a URL is provided -->
      {% if cert.url and cert.url != "" %}
        <a href="{{ cert.url }}" target="_blank" rel="noopener noreferrer">
          <img src="{{ cert.url }}" alt="{{ cert.name }}" class="cert-img">
        </a>
      {% else %}
        <!-- Fallback if no image is uploaded -->
        <div class="cert-img-placeholder">
          <i class="fas fa-award fa-3x"></i>
        </div>
      {% endif %}

      <div class="cert-info">
        <h3>{{ cert.name }}</h3>
        <p class="cert-issuer"><strong>{{ cert.issuer }}</strong></p>
        <p class="cert-date">{{ cert.date }}</p>
      </div>

    </div>
  {% endfor %}
</div>

<style>
/* Grid Layout: Automatically fits as many 250px cards as possible per row */
.cert-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 25px;
  margin-top: 30px;
}

/* The Card Container */
.cert-card {
  border: 1px solid var(--card-border-color, #e1e4e8);
  border-radius: 8px;
  overflow: hidden;
  background-color: var(--card-bg-color, #ffffff);
  box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  display: flex;
  flex-direction: column;
}

/* Hover Effect: Lifts the card up slightly */
.cert-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 15px rgba(0,0,0,0.15);
}

/* The Image inside the card */
.cert-img {
  width: 100%;
  height: 180px;
  object-fit: cover; /* Ensures images fill the space without stretching */
  border-bottom: 1px solid var(--card-border-color, #e1e4e8);
  display: block;
}

/* The Placeholder (if no image exists) */
.cert-img-placeholder {
  width: 100%;
  height: 180px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--card-placeholder-bg, #f6f8fa);
  color: #a3aab1;
  border-bottom: 1px solid var(--card-border-color, #e1e4e8);
}

/* The Text Section */
.cert-info {
  padding: 15px;
  flex-grow: 1;
}

.cert-info h3 {
  margin: 0 0 10px 0;
  font-size: 1.15em;
  line-height: 1.3;
}

.cert-info p {
  margin: 0;
  font-size: 0.9em;
}

.cert-issuer {
  color: var(--text-color, #333);
  margin-bottom: 5px !important;
}

.cert-date {
  color: #6a737d;
}

/* Dark Mode Support for modern Jekyll Themes */
@media (prefers-color-scheme: dark) {
  .cert-card {
    --card-bg-color: #22272e;
    --card-border-color: #444c56;
    --card-placeholder-bg: #2d333b;
    --text-color: #c9d1d9;
  }
  .cert-info h3 { color: #c9d1d9; }
  .cert-date { color: #768390; }
  .cert-card { box-shadow: 0 2px 8px rgba(0,0,0,0.5); }
}
</style>
