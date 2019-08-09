---
layout: blank
title: Let's Encrypt Stats
slug: stats-dashboard
date: 2018-04-12
---
<!-- This is used as a full-screen display by various parties, including
     (minimally) Mozilla. Please check with the committers before removing. -->

<div class="dashboard">
  <div class="figure">
    <h1>Let's Encrypt Growth Timeline</h1>
    <div id="combinedTimeline" title="Issuance Timeline" class="statsgraph">
  </div>

  <p><a href="{{< ref "/stats.md" >}}">{{< ref "/stats.md" >}}</a></p>
</div>

<script src="/js/stats.js"></script>
<script src="/js/plotly-min.js"></script>