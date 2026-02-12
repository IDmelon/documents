---
title: "Overview"
description: ""
lead: ""
date: 2026-01-30 11:43:39 -08:00
lastmod: 2026-01-30 11:43:39 -08:00
draft: false
images: []
menu:
  docs:
    parent: "deployment"
weight: 1000
type: docs
toc: true
---

<section class="sub-menu section-sm pt-0 ps-3 pb-3">
  <div class="row">
    <div class="home-page-card card-transition home-card-body text-dark" >
      <div class="card-body py-3 d-flex flex-column align-items-center justify-content-center">
        <h2 class="card-title">IDmelon as SaaS</h2>
        <p class="card-text">Create and import users, create shared accounts, check user activities.</p>
        <a href="/docs/{{ if .Site.Params.options.docsVersioning }}{{ .Site.Params.docsVersion }}/{{ end }}deployment/saas/overview" class="stretched-link"></a>
      </div>
    </div>
    <div class="home-page-card card-transition home-card-body text-dark" >
      <div class="card-body py-3 d-flex flex-column align-items-center justify-content-center">
        <h2 class="card-title">On-Prem</h2>
        <p class="card-text">Assign, disable, import, manage security keys, and configure pins.</p>
        <a href="/docs/{{ if .Site.Params.options.docsVersioning }}{{ .Site.Params.docsVersion }}/{{ end }}deployment/on_prem/overview" class="stretched-link"></a>
      </div>
    </div>
    <div class="home-page-card card-transition home-card-body text-dark" >
      <div class="card-body py-3 d-flex flex-column align-items-center justify-content-center">
        <h2 class="card-title">IDmelon.exe Apps</h2>
        <p class="card-text">Automatically deploy IDmelon applications using scripts, PDQ, or Intune.</p>
        <a href="/docs/{{ if .Site.Params.options.docsVersioning }}{{ .Site.Params.docsVersion }}/{{ end }}deployment/exe_apps/overview" class="stretched-link"></a>
      </div>
    </div>
  </div>
</section>