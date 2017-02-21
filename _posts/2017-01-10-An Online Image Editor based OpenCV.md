---
title: 一个基于OpenCV的图像在线处理系统
tags: OpenCV PHP Python
---

## 目的
做这个系统的初衷是为完成课程设计。   
通过浏览器，它可以简单地处理图片，例如二值化、翻转、调整尺寸等。优势是图像处理过程在服务器完成，不占用本地计算资源，可以设计略复杂的图片处理程序交给服务器完成；劣势是必须联网才能使用。   
**项目地址： [github.com/kyshel/oie](https://github.com/kyshel/oie)**

## 原理
此系统的核心问题是浏览器读取用户的图像后，如何将网页上的操作传递给OpenCV函数去处理，并返回处理结果。   
这里利用Shell命令来连接前端事件响应和后端处理程序，当前端触发一个事件时，脚本语言执行对应事件的Shell命令，完成后端处理程序的调用，后端处理程序把处理结果以文件形式保存在服务器，最后由前端调出处理后的文件，显示出结果。

## 细节
###文件结构：
![aa](https://cloud.githubusercontent.com/assets/11898075/23148089/8f94a960-f81e-11e6-86f9-719f308bc0c5.png)

- index.php 系统首页，重定向至page/目录内
- page/ php页面文件存放地址
 - dist/ 存放前端库文件，
 - data.json 配置数据
 - functions.php 个人的php函数
 - filter.php 图像处理、对比主页面
 - js.php 通过JavaScript为filter.php提供事件逻辑
 - upload.php 上传页面，用来取得用户的图像
 - get_upload.php 保存用户图像
 - header.php 导航栏模板
 - load.php 加载session
 - ajax.php 返回异步加载内容
- python/
对应page目录下的data.json文件，查看filter对象得核心功能有8个：
  - 灰度变换 gray.py
  - 拉普拉斯算子 laplacian.py.py
  - 灰度化 blur.py
  - 二值化 threshold.py
  - 旋转 rotate.py
  - 改变大小 resize.py
  - 翻转 flip.py
  - 按权重叠加 add_weight.py
- upload/ 上传文件的保存目录
- processed/ 处理后文件的保存目录
- README.md 说明文件

### 程序流程
(1).在浏览器中输入系统地址后，页面重定向到 host/page/index.php
form 表单接收上传的图像：
``` html
<input type="file" name="file" id="file" class="btn btn-default" />
```
(2).上传后，如果文件合法，文件名用uniqid()函数作前缀保证文件唯一性，并保存到upload/文件下。文件的路径、尺寸、大小等信息保存在Session中，这样通过判断Session是否存在来判断是否已上传文件
``` php
$file_uid_name = uniqid().'.'.$extension;
$_SESSION["file_origin_path"] = $file_origin_path;
```
(3).文件保存后，可以点击Process按钮定向到filter页面，进行图像处理，也可以清除Session，重新上传图片
``` html
<a href="filter.php"><button class="btn btn-default">Click here to process</button></a>
```
(4).选择Process后，出现一张处理功能的按钮集合，每个按钮都绑定了事件，此集合的元素为遍历data.json文件的对象所得:
``` php
foreach ($array['filter'] as $item => $item_content) {
  echo '<button id="'.$item.'" onclick="select_argv(this.value)" value="'.$item.'" class="btn btn-default process_button">'.ucwords($item).'</button> ';
}
```
(5).点击某个按钮后，绑定的JS函数就会触发，JS通过读取data.json中的处理器类型信息，来决定渲染参数的显示方式，并向ajax发送异步请求
``` javascript
send_ajax(op,v3,v4);
```
(6).ajax.php接受请求后，根据来源链接、传递的参数、Session来拼接出一条Shell命令，并执行
``` php
$command = escapeshellcmd('python ../python/'.$operate.'.py '.$file_path.' '.$save_path.' '.$argv);
```
(7).Shell命令调用相应操作的python程序进行处理，得到结果文件
``` php
$output = shell_exec($command);
```
(8). ajax.php做出一个结果文件的img标签，返回response
``` php
echo '<img src="'.$save_path.'?'.time().'" style="max-width:300px;">';
```
(9). filter.php的JS将response显示到屏幕上，至此完成整个过程
``` js
$('#ajax_response').html(response);
```
## 参考资料
1. Python计算机视觉编程 Solem,J.E. 著 2014.7
2. Python-openCV文档 https://opencv-python-tutroals.readthedocs.io/en/latest/
3. OpenCV官方文档 http://docs.opencv.org/2.4/
4. 图像加权叠加方法 http://www.pyimagesearch.com/2016/03/07/transparent-overlays-with-opencv/
5. Professional JavaScript for Web Developers, 3rd Edition Nicholas C. Zakas 2011
6. PHP官方文档 http://www.php.net/manual/en/
7. Bootstrap框架 http://getbootstrap.com/
8. Windows利用Xming显示Xwindows窗口，方便OpenCV调试 https://wiki.centos.org/HowTos/Xming
