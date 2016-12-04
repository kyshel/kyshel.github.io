---
ID: 7
post_title: >
  One key to save and run remote python
  code
author: jack
post_date: 2016-05-27 08:55:06
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/one-key-to-save-and-run-remote-python-code/
published: true
---
<h3>Condition</h3>
<ul>
 	<li>Local PC: Windows 8.1</li>
 	<li>Remote Server: CentOS 7.0</li>
 	<li>Remote Service: Samba (Tutorial click here)</li>
</ul>
<!--more-->

When I debug remote python code, I need putty to connect server, open vim, edit it, save, run.

But if I need to debug more times, it is a waste time to save and run. And as a win-rely user, I prefer SublimeText to edit code than vim.

So, I want to find a simple way to debug remote python code.

The function I want to acheive is, press one key, code will compile and run, I can see the result or error message, like a simple IDE(Integrate Develop Environment).
<h3>How to do it?</h3>
It is easy to do this, all the thing that you need is a small soft: <strong>AutoHotKey</strong>.
Follow some simple steps as below:
<ol>
 	<li>Download and install AutoHotKey</li>
 	<li>Rightclick desktop &gt; create new file &gt; New &gt;Text Document</li>
 	<li>rename the extention from .txt to .ahk, we rename save_run.ahk here</li>
 	<li>rightclick save_run.ahk &gt; edit</li>
 	<li>save below code to the file:</li>
</ol>
<pre>#IfWinActive, ahk_class PX_WINDOW_CLASS
	F5::
	st_save()
	sleep, 500
	putty_run()
	return
#If

st_save(){
	send,^s
}

putty_run(){
	IfWinExist, root@localhost:~/py/pd
	{
	    ;Format: ControlSend,, KEYS, WIN_TITLE_BY_SPY
		ControlSend,, p{space}t.py{enter}, root@localhost:~    
	}else{
		MsgBox "Window &gt;&gt;&gt;root@localhost:~&lt;&lt;&lt; not exist"
	}
}	
</pre>
<h3>Have a try</h3>
<ol>
 	<li>When you done, double click save_run.ahk, then you can see a green icon in taskbar:</li>
 	<li>open the putty and connect to server, cd to ~, touch t.py</li>
 	<li>open the file in samba shared folders with sublimetext</li>
 	<li>press F5, then you can see result in putty</li>
</ol>
<h3>Why I do this?</h3>
Maybe you think I am walking a far way to the target, cause you can install python in windows and everything is very simple.

But, I prefer original things, I always think that running python code in linux will robust. When I debug done, I can put it to use instantly in my remoe server, there is no compatible problem.

I am a crazy Unixer. Instantly I fall in love with the simple white character printing on black background when I first exploring Unix world.(I cannot save myself anymore, enh?) And you know that's called bash.

I like running code in shell. So even in windows, I just only want to modify code. Running task handle to server.

I did not use vim cause it's a little hard to walk in door, and SublimeText's sexy face attracted me(Feel myself a bitch <em>.</em>), I do not want to loose the mouse in addtion(So you love the little mouse, I think you need a cat ~.~).
<h3>Leading Actor</h3>
Last let's see the leading actor today - AutoHotKey

I call it window's shell script ,like the shell script in linux. Cause I can do many auto things by it. It's a big tool to me and it helped me a lot. If you are interested to this software, I recommend you see this post:
<h3>Some tips:</h3>
<ol>
 	<li>Samba's config is vital, or security issues will happen.</li>
 	<li>Set two LED display is better, then you can edit in one, show results in another.</li>
</ol>
<h3>About</h3>
This post is created by <a href="http://kyshel.com">kyshel</a>

If you have any advice, comment below or leave me a message.
Looking foward to be your audience~