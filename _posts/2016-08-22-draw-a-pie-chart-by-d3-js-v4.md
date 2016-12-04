---
ID: 52
post_title: Draw a pie chart by d3.js v4
author: Kyshel
post_date: 2016-08-22 03:44:46
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/draw-a-pie-chart-by-d3-js-v4/
published: true
---
Draw a pie chart by d3.js v4.
Like this:

<iframe src="https://kyshel.github.io/GistLive/raw/?id=8cb1aaffc6b7111a9dcf00785e9eb175" scrolling="no" style="height: 300px;width:300px"></iframe>




<!--more-->
Code:
<pre>&lt;canvas width="300" height="300" id="pad"&gt;&lt;/canvas&gt;

&lt;script type="text/javascript"&gt; 
var data = [1, 1, 2, 3, 5, 8, 13];

var canvas = document.getElementById("pad"),
context = canvas.getContext("2d");

var width = canvas.width,
height = canvas.height,
radius = Math.min(width, height) / 2;

var colors = function(){
return "hsl(" + Math.random() * 360 + ",100%,50%)";
} 

var arc = d3.arc()
.outerRadius(radius - 10)
.innerRadius(0)
.context(context);

var labelArc = d3.arc()
.outerRadius(radius - 20)
.innerRadius(radius - 20)
.context(context);

var pie = d3.pie();
var arcs = pie(data);

// move to center
context.translate(width / 2, height / 2);   

// pie
context.globalAlpha = 0.5;
arcs.forEach(function(d, i) {
  context.beginPath();
  arc(d);
  context.fillStyle = colors();
  context.fill();
});

// num
context.textAlign = "center";
context.textBaseline = "middle";
context.fillStyle = "#000";
arcs.forEach(function(d, i) {
var c = labelArc.centroid(d);
context.fillText(data[i], c[0], c[1]);
});
</pre>