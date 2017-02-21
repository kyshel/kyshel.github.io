---
title: Jekyll Wordpress Sync - 一个Wordpress插件的提交跟踪  
tags: Wordpress
---


Jekyll Wordpress Sync 是一个可以导入Jekyll文章的Wordpress插件    
插件地址: [github.com/kyshel/jekyll-wordpress-sync](https://github.com/kyshel/jekyll-wordpress-sync)     
详细信息： [JWS - 一个可同步Jekyll文章的Wordress插件](http://kyshel.me/2017/02/14/A-wordpress-plugin-that-can-import-posts-from-jekyll/)       
本文主要记录该插件的提交情况。

## 2017-02-14
我于2月14号提交插件，原插件名为Jekyll-Wordpress-Sync，提交的时候被告知名字中不能带有Wordpress，所以我把名字改成了Jekyll-WP-Sync

## 2017-02-16
收到了wordpress.org的第一封邮件，内容如下：        
[第一封邮件截图](https://cloud.githubusercontent.com/assets/11898075/23151232/42345c2c-f834-11e6-863a-efc7cedd9e33.jpg)    
他说插件名字有问题，我不能代表Jekyll，所以不能以Jekyll开头，我又把名字改成了WP-Jekyll-Sync，重新提交。

## 2017-02-17 12:08
收到第二封邮件，内容如下：   
[第二封邮件截图](https://cloud.githubusercontent.com/assets/11898075/23151235/4861e52e-f834-11e6-86d9-c4806b21df05.jpg)    
他提出了3个问题：
- 插件函数名应该唯一，在kutil.php中，我定义的kbn,kgreen,kred都不合理
- 应该禁止对插件文件的直接访问，来避免安全问题
- 应该为POST请求加入nonce字段，来预防未授权的访问。

可以看出Wordpress插件团队还是相当有耐心来review提交的代码，我也解决关于使用PHP的一个困惑：用url直接访问文件是否会有问题，从邮件来看，问题还不小。

对于上面3个问题，我的应对方法:
- 删除kutil.php，它是方便我调试程序写的临时函数，现在可以去掉。
- 按邮件中说的，在每个文件顶端加入：
``` php
if(!defined('ABSPATH')) exit;  // Exit if accessed directly
```
- 加上nonce字段，form直接加入`wp_nonce_field( 'delete-comment_'.$comment_id );`,ajax的话如下所示：
``` php
<?php
//Set Your Nonce
$ajax_nonce = wp_create_nonce( "my-special-string" );
?>

<script type="text/javascript">
jQuery(document).ready(function($){
  var data = {
  action: 'my_action',
  security: '<?php echo $ajax_nonce; ?>',
  my_string: 'Hello World!'
  };
  $.post(ajaxurl, data, function(response) {
  alert("Response: " + response);
  });
});
</script>
```

目前正在修改中~
