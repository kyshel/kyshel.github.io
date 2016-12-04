---
ID: 101
post_title: Install Lavarel5 On CentOS7
author: Kyshel
post_date: 2016-09-07 11:41:23
post_excerpt: ""
layout: post
permalink: >
  http://kyshel.com/blog/install-lavarel5-on-centos7/
published: true
---
<p><strong>1.Requirement</strong></p>
<ul>
<li>PHP &gt;= 5.6.4<br />
<a href="http://kyshel.com/blog/install-php-5-6-by-yum-on-centos7/">Install PHP 5.6 by yum on CentOS7</a></li>
<li>Mbstring PHP Extension<br />
<code>yum install php-mbstring</code></li>
<li>Others<br />
<code>yum install php-xml</code></li>
</ul>
<!--more-->


<p><strong>2.Install Composer</strong></p>
<pre><code>php -r &quot;copy('https://getcomposer.org/installer', 'composer-setup.php');&quot;
php -r &quot;if (hash_file('SHA384', 'composer-setup.php') === 'e115a8dc7871f15d853148a7fbac7da27d6c0030b848d9b3dc09e2a0388afed865e6a3d6b3c0fad45c48e2b5fc1196ae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;&quot;
php composer-setup.php
php -r &quot;unlink('composer-setup.php');&quot;
</code></pre>

<p><strong>3.Global Composer</strong></p>
<pre><code>mv composer.phar /usr/local/bin/composer
</code></pre>

<p><strong>4.Download Laravel-Installer</strong></p>
<pre><code>composer global require &quot;laravel/installer&quot;
</code></pre>

<p><strong>5.Global Laravel-Installer</strong></p>
<pre><code>ln -s ~/.config/composer/vendor/bin/laravel /usr/local/bin/
</code></pre>

<p><strong>6.Create a fresh Laravel installation</strong></p>
<pre><code>laravel new blog
</code></pre>

<p><strong>7.Config Permission</strong></p>
<pre><code>cd blog
chown apache:apache storage/ -R
</code></pre>

<p><strong>8.Open <a href="localhost/blog/public">localhost/blog/public</a> , and enjoy~</strong></p>
<p><img src="https://kyshel.github.io/file/blog/Lavarel_install_kyshel.png" alt="Lavarel_install_ok" /></p>