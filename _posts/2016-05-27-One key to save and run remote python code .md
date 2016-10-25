---
layout: default
---


#One key to save and run remote python code 

###Condition
- Local PC:      Windows 8.1
- Remote Server: CentOS 7.0 
- Remote Service: Samba (Tutorial click here)

When I debug remote python code, I need putty to connect server, open vim, edit it, save, run.

But if I need to debug more times, it is a waste time to save and run. And as a win-rely user, I prefer SublimeText to edit code than vim.

So, I want to find a simple way to debug remote python code.

The function I want to acheive is, press one key, code will compile and run, I can see the result or error message, like a simple IDE(Integrate Develop Environment).

###How to do it?
It is easy to do this, all the thing that you need is a small soft: **AutoHotKey**.
Follow some simple steps as below:

1. Download and install AutoHotKey
2. Rightclick desktop > create new file > New >Text Document
3. rename the extention from .txt to .ahk, we rename save_run.ahk here
4. rightclick save_run.ahk > edit
5. save below code to the file:

<pre>
#IfWinActive, ahk_class PX_WINDOW_CLASS
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
		MsgBox "Window >>>root@localhost:~<<< not exist"
	}
}	
</pre>

###Have a try
1. When you done, double click save_run.ahk, then you can see a green icon in taskbar:
2. open the putty and connect to server, cd to ~, touch t.py
3. open the file in samba shared folders with sublimetext 
4. press F5, then you can see result in putty

###Why I do this?
Maybe you think I am walking a far way to the target, cause you can install python in windows and everything is very simple.

But, I prefer original things, I always think that running python code in linux will robust. When I debug done, I can put it to use instantly in my remoe server, there is no compatible problem. 

I am a crazy Unixer. Instantly I fall in love with the simple white character printing on black background when I first exploring Unix world.(I cannot save myself anymore, enh?) And you know that's called bash. 

I like running code in shell. So even in windows, I just only want to modify code. Running task handle to server.

I did not use vim cause it's a little hard to walk in door, and SublimeText's sexy face attracted me(Feel myself a bitch *.*), I do not want to loose the mouse in addtion(So you love the little mouse, I think you need a cat ~.~).

###Leading Actor
Last let's see the leading actor today - AutoHotKey

I call it window's shell script ,like the shell script in linux. Cause I can do many auto things by it. It's a big tool to me and it helped me a lot. If you are interested to this software, I recommend you see this post:



###Some tips:
1. Samba's config is vital, or security issues will happen.
2. Set two LED display is better, then you can edit in one, show results in another.

###About
This post is created by [kyshel](http://kyshel.com)

If you have any advice, comment below or leave me a message.
Looking foward to be your audience~
