---
ID: 38
post_title: 'Team Explorer for Visual Studio 2015  Fatal Error'
author: Kyshel
post_date: 2016-08-06 02:40:25
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/team-explorer-for-visual-studio-2015-fatal-error/
published: true
---
When Install visual studio 2015, an error occurred:

<img class="alignnone size-full wp-image-41" src="https://raw.githubusercontent.com/kyshel/file/master/blog/vs2015_setup_failed.png" width="458" height="640" />

To fix this, follow steps below:

<!--more-->
<ol>
 	<li>click <em>close</em> or <em>restart</em> button</li>
 	<li>uninstall <strong>MS VC++ 2015 Redistributables</strong> in <strong>Programs and Features</strong></li>
 	<li>download and install this file: <a href="https://www.microsoft.com/en-us/download/details.aspx?id=48145">Visual C++ Redistributable for Visual Studio 2015</a></li>
 	<li>restart the Visual Studio 2015 installation</li>
</ol>
Install will be success.

<em>This passage is made by <a href="http://kyshel.com">kyshel</a>, </em><em>Hope it's useful to U.</em>