---
layout: about
title: about
permalink: /
description: ML | Graduate Student | Software Engineer 

profile:
  image: isaacb_headshot.jpg
news: false  # includes a list of news items
social: true  # includes social icons at the bottom of the page
---

Hello and welcome to my personal website !
I am currently conducting research at Applied Research Labratories, where I am utilizing
unsupervised-deep learning for classification of sonar imagery.
I am also pursuing an M.S. in Computer Science from UT Austin, where I am focusing in 
Artificial Intelligence and Machine Learning.

<p class="cta_buttons">
    <a class="btn btn-primary" href="#social"> Contact Me </a>
    <a class="btn btn-danger" href="./assets/pdf/utcs_isaacb_resume.pdf"> My Resume </a>
</p>

<div id="projects" class="projects">
    <header>
        <h1 class="landing_header">
            Projects
        </h1>
    </header>
        
    <div class="grid container">
        
      {% assign sorted_projects = site.projects | sort: "importance" %}
      {% for project in sorted_projects %}
      <div class="grid-item">
        {% if project.redirect %}
        <a href="{{ project.redirect }}" target="_blank">
        {% else %}
        <a href="{{ project.url | relative_url }}">
        {% endif %}
          <div class="card hoverable">
            {% if project.img %}
            <img class="project_thumbnail" src="{{ project.img | relative_url }}" alt="project thumbnail">
            {% endif %}
            <div class="card-body">
              <h2 class="card-title ">{{ project.title }}</h2>
              <p class="card-text">{{ project.description }}</p>
              <div class="row ml-1 mr-1 p-0">
                {% if project.github %}
                <div class="github-icon">
                  <div class="icon" data-toggle="tooltip" title="Code Repository">
                    <a href="{{ project.github }}" target="_blank"><i class="fab fa-github gh-icon"></i></a>
                  </div>
                  {% if project.github_stars %}
                  <span class="stars" data-toggle="tooltip" title="GitHub Stars">
                    <i class="fas fa-star"></i>
                    <span id="{{ project.github_stars }}-stars"></span>
                  </span>
                  {% endif %}
                </div>
                {% endif %}
              </div>
            </div>
          </div>
        </a>
      </div>
    {% endfor %}
    
    </div>
</div>