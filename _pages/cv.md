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
  <!-- <a href="/files/RESUME_Raza_Hashmi.pdf" class="cv-download-btn" download>⬇️ Download CV (PDF)</a> -->
  <button id="download-pdf-btn" class="cv-download-btn" type="button" style="margin-left:1em;">🖨️ Download as PDF</button>
  <h1>Curriculum Vitae</h1>
  <hr>
  <h2>Education</h2>
  <ul>
    <li><strong>BSc in Economics</strong>, Lahore University of Management Sciences, 2016</li>
  </ul>

  <h2>Work Experience</h2>
  <ul>
    <li><strong>Technical and Data Lead</strong> <br>
      <em>Ark Re ltd.</em> <br>
      <span class="cv-date">March 2016 - Present</span><br>
      <span class="cv-desc">Leading Development team, Solutions Engineering, ML.</span>
    </li>
    <li><strong>Research Assistant</strong> <br>
      <em>Lahore University of Management Sciences</em> <br>
      <span class="cv-date">April 2019 - Feb 2020</span><br>
      <span class="cv-desc">Working on different Research Ideas. Supervisor: Professor Asim Karim</span>
    </li>
    <li><strong>Research Assistant</strong> <br>
      <em>Lahore University of Management Sciences</em> <br>
      <span class="cv-date">2016-2018</span><br>
      <span class="cv-desc">Writing code and conducting experiments to quantify risk attitudes in Islamic vs traditional equities. Supervisor: Professor Imtiaz-ul-Haq</span>
    </li>
    <li><strong>Teaching Assistant</strong> <br>
      <em>MECO 121 Principles of Macroeconomics, LUMS</em> <br>
      <span class="cv-date">2016</span>
    </li>
    <li><strong>Teaching Assistant</strong> <br>
      <em>ECON 221 Intermediate Macroeconomics, LUMS</em> <br>
      <span class="cv-date">2015</span>
    </li>
  </ul>

  <h2>Honors & Awards</h2>
  <ul>
    <li>Graduation with High Distinction, 2016</li>
    <li>Deans Honours List – Lahore University of Management Sciences, 2012-2016</li>
  </ul>

  <h2>Skills</h2>
  <ul>
    <li><strong>Python</strong>: Pytorch, Jax, Tensorflow, Keras</li>
    <li><strong>SQL</strong></li>
    <li><strong>Java, Kotlin</strong>: App Development</li>
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

<!-- jsPDF and html2canvas CDN -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<script>
document.getElementById('download-pdf-btn').addEventListener('click', function() {
  const cvSection = document.querySelector('.cv-section');
  html2canvas(cvSection, { scale: 2 }).then(canvas => {
    const imgData = canvas.toDataURL('image/png');
    const pdf = new window.jspdf.jsPDF('p', 'pt', 'a4');
    const pageWidth = pdf.internal.pageSize.getWidth();
    const pageHeight = pdf.internal.pageSize.getHeight();
    // Calculate image dimensions to fit A4
    const imgWidth = pageWidth - 40;
    const imgHeight = canvas.height * imgWidth / canvas.width;
    let position = 20;
    pdf.addImage(imgData, 'PNG', 20, position, imgWidth, imgHeight);
    // If content overflows, add new pages
    let remainingHeight = imgHeight;
    while (remainingHeight > pageHeight - 40) {
      pdf.addPage();
      position = 20;
      remainingHeight -= (pageHeight - 40);
      pdf.addImage(imgData, 'PNG', 20, -remainingHeight + 20, imgWidth, imgHeight);
    }
    pdf.save('CV_Raza_Hashmi.pdf');
  });
});
</script>

<style>
.cv-section {
  max-width: 800px;
  margin: 0 auto;
  background: #fff;
  padding: 2rem 2.5rem;
  border-radius: 12px;
  box-shadow: 0 2px 16px rgba(0,0,0,0.07);
}
.cv-section h1 {
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 0.5rem;
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
  display: inline-block;
  margin: 0 auto 1.5rem auto;
  padding: 0.7em 1.5em;
  background: #2a7ae2;
  color: #fff;
  border-radius: 6px;
  font-size: 1.1em;
  font-weight: 600;
  text-decoration: none;
  box-shadow: 0 2px 8px rgba(42,122,226,0.08);
  transition: background 0.2s;
}
.cv-download-btn:hover {
  background: #185a9d;
  color: #fff;
}
</style>


