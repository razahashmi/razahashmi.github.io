---
layout: archive
permalink: /cv/
# title: "Curriculum Vitae"
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

<section class="cv-section">
  <div class="cv-header">
    <h1>Curriculum Vitae</h1>
    <a href="/files/RESUME_Raza_Hashmi.pdf" class="cv-download-btn" download title="Download CV (PDF)">⬇️</a>
  </div>
  <hr>
  <h2>Education</h2>
  <ul>
    <li><strong>BSc in Economics</strong>, Lahore University of Management Sciences, 2016</li>
  </ul>

  <h2>Work Experience</h2>
  <ul>
    <li><strong>Head of Product</strong> <br>
      <em>SHAPE Global Ltd</em> <br>
      <span class="cv-date">Jul 2023 - Aug 2025</span><br>
      <span class="cv-desc">
        - Leading SHAPE’s vision to become a science-backed intelligence platform for organizational health.<br>
        - Driving the product and development roadmap for core features, including survey intelligence, multi-level role-specific reporting, and AI integrations.<br>
        - Launching partner portals, strategic integrations, and admin tools to advance SHAPE as a self-sustaining HR intelligence solution.
      </span>
    </li>
    <li><strong>Product Manager</strong> <br>
      <em>World Flourishing Organization</em> <br>
      <span class="cv-date">Nov 2024 - Jul 2025</span><br>
      <span class="cv-desc">
        - Prototyped an AI-driven compliance framework that processes client-uploaded evidence and assigns Flourishing level.<br>
        - Developed a global flourishing standard combining internal metrics (Surveys, Policies) with external factors (ESG, sentiment).<br>
        - Laid the foundation for a new global benchmark in evaluating workplace wellbeing and ethical performance.
      </span>
    </li>
    <li><strong>Technical Product Lead</strong> <br>
      <em>SHAPE Global Ltd</em> <br>
      <span class="cv-date">May 2021 - Jun 2023</span><br>
      <span class="cv-desc">
        - Architected advanced privacy-preserving algorithms for survey anonymity.<br>
        - Led development of data caching pipelines, analytics dashboards, and role-based permissions for enterprise customers.<br>
        - Scaled the platform to support complex, multi-layered reporting for global organizations with dynamic hierarchies.
      </span>
    </li>
    <li><strong>Product Innovation Manager</strong> <br>
      <em>SHAPE Global Ltd</em> <br>
      <span class="cv-date">Sep 2019 - May 2021</span><br>
      <span class="cv-desc">
        - Pioneered a proprietary Scoring methodology for holistic employee feedback.<br>
        - Developed tools for enhanced user onboarding, org-mapping features, and survey configuration to drive platform adoption.
      </span>
    </li>
    <li><strong>Technical & Data Analyst</strong> <br>
      <em>SHAPE Global Ltd</em> <br>
      <span class="cv-date">Oct 2017 - Sep 2019</span><br>
      <span class="cv-desc">
        - Engineered SHAPE’s initial product infrastructure—covering survey logic, reporting, and admin workflows.<br>
        - Oversaw early client deployments, gathering user feedback to drive the initial product development lifecycle.
      </span>
    </li>
    <li><strong>Machine Learning Researcher</strong> <br>
      <em>Lahore University of Management Sciences</em> <br>
      <span class="cv-date">Sep 2018 - Sep 2019</span><br>
      <span class="cv-desc">
        - Investigated novel neural network architectures and training methodologies (Dr. Asim Karim).<br>
        - Focus on model interpretability and performance optimization.<br>
        - Co-authored "Explainable AI: Object Recognition With Help From Background" (ICLR 2022 CSS Workshop).
      </span>
    </li>
    <li><strong>Product & Data Analyst</strong> <br>
      <em>Ofono App</em> <br>
      <span class="cv-date">Mar 2017 - Nov 2017</span><br>
      <span class="cv-desc">
        - Worked on core user features for capturing emotional feedback (sliders, emojis, contextual text).<br>
        - Developed the initial data structure for emotional tagging and sentiment analysis.
      </span>
    </li>
    <li><strong>Research Assistant</strong> <br>
      <em>Lahore University of Management Sciences</em> <br>
      <span class="cv-date">May 2016 - Aug 2017</span><br>
      <span class="cv-desc">
        - Supported research projects on financial inclusion and economic development (Dr. Imtiaz ul Haq).<br>
        - Conducted data analysis, policy evaluation, and literature reviews.
      </span>
    </li>
    <li><strong>Intern</strong> <br>
      <em>Ministry of Planning, Development and Reforms</em> <br>
      <span class="cv-date">Jun 2015 - Sep 2015</span><br>
      <span class="cv-desc">
        - Assisted in data collection, policy research, and evaluation of national economic initiatives.
      </span>
    </li>
  </ul>

  <h2>Honors & Awards</h2>
  <ul>
    <li>Graduation with High Distinction, 2016</li>
    <li>Deans Honours List – Lahore University of Management Sciences, 2012-2016</li>
  </ul>

  <h2>Skills</h2>
  <ul>
    <li><strong>Technical</strong>: Python (Pytorch, Jax, Tensorflow, Keras), SQL, Java, Kotlin</li>
    <li><strong>Product & Research</strong>: Product Strategy, Privacy Algorithms, Uncertainty Estimation, Interpretability</li>
    <li><strong>Languages</strong>:
      <ul>
        <li>Urdu (Native)</li>
        <li>Persian (Native)</li>
        <li>Punjabi (Native)</li>
        <li>English (Expert)</li>
        <li>Hindi (Expert)</li>
        <li>French (Beginner)</li>
      </ul>
    </li>
  </ul>

  <h2>Publications</h2>
  <ul>
    {% for post in site.publications %}
      {% include archive-single-cv.html %}
    {% endfor %}
  </ul>

  <!-- Teaching
  <h2>Teaching</h2>
  <ul>
    {% for post in site.teaching %}
      {% include archive-single-cv.html %}
    {% endfor %}
  </ul>
  -->

  <h2>Experiments & Insights</h2>
  <p class="cv-note">A collection of projects and ideas that provided valuable lessons, even if they didn't succeed as planned.</p>
  <ul>
    {% for post in site.talks %}
      {% include archive-single-talk-cv.html %}
    {% endfor %}
  </ul>
</section>

<style>
.cv-section {
  max-width: 800px;
  margin: 0 auto;
  background: #fff;
  padding: 2rem 2.5rem;
  border-radius: 12px;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
}
.cv-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 0.5rem;
}
.cv-header h1 {
  margin: 0;
  font-size: 2.5rem;
}
.cv-section h2 {
  color: #2a7ae2;
  margin-top: 2rem;
  margin-bottom: 0.5rem;
  border-bottom: 1px solid #eaeaea;
  padding-bottom: 0.2rem;
}
.cv-section ul {
  list-style: none;
  padding-left: 0;
}
.cv-section li {
  margin-bottom: 1.1rem;
  line-height: 1.6;
}
.cv-date {
  color: #888;
  font-size: 0.95em;
}
.cv-desc {
  display: block;
  color: #444;
  margin-top: 0.2em;
}
.cv-note {
  font-style: italic;
  color: #666;
  margin-bottom: 1em;
}
.cv-download-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-left: 1.5rem;
  padding: 0.5em 0.7em;
  background: none;
  color: #2a7ae2;
  border: none;
  border-radius: 50%;
  font-size: 1.7em;
  font-weight: 600;
  text-decoration: none;
  box-shadow: none;
  transition: background 0.2s, color 0.2s;
  white-space: nowrap;
}
.cv-download-btn:hover {
  background: #f0f4fa;
  color: #185a9d;
}
</style>


