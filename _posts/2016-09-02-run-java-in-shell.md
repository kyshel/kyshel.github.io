---
ID: 86
post_title: Run java in shell
author: Kyshel
post_date: 2016-09-02 19:44:08
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/run-java-in-shell/
published: true
---
Run java code in shell is simple.

Before start, please make sure you has installed jdk.
In CentOS7, run command below(here is 1.8.0):
<pre><code># yum install java-1.8.0-devel
</code></pre>
Then, Let's begin.

<!--more-->
<ol>
 	<li>Create new file named Box.class and add below code:
<pre><code>public class Box {
    public static void main(String[] args){
        System.out.println("Hello, world !");
    }
}
</code></pre>
</li>
 	<li>Run
<pre><code># javac Box.java
</code></pre>
</li>
 	<li>Run
<pre><code># java Box
</code></pre>
</li>
 	<li>You will see the console print <code>Hello,world!</code></li>
</ol>