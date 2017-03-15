---
tags: [CSV,Excel]
title: CSV文件编码问题
---

csv文件全称为Comma-separated values，在用Excel处理csv文件时，经常会遇到一些棘手的编码问题。本文从查看csv文件和制作csv文件两个方面讨论。

## 查看csv文件
下面是用Python把数据写入csv文件的代码。

``` python
with open(file_name,'wb') as f:
    writer = csv.writer(f)
    writer.writerows(data_list)
```

如果用记事本打开写入后的csv文件，得到的是以逗号为分隔符的raw data，如果某个字段的数据过长，整体的浏览体验很差。
![image](https://cloud.githubusercontent.com/assets/11898075/23133457/1b280aba-f7cc-11e6-98a6-12a06e509468.png)

为更好地查看和处理这些数据，我们用excel打开，结果英文显示正常，中文显示不正常:
![image](https://cloud.githubusercontent.com/assets/11898075/23133628/be43f7d6-f7cc-11e6-9b7e-da6749530b79.png)

出现这种问题的原因是Python写入的文件默认为UTF-8编码，而Excel在打开csv文件时并不会以UTF-8编码解析，所以显示不正常。这里提供两种解决方法。

- 方法一，先用记事本打开csv文件，另存为选择ANSI编码，然后用Excel打开另存后的文件即可正常显示。缺点是用Excel处理保存之后的csv文件编码不再为UTF-8。
- 方法二，把csv文件直接拖到OpenOffice中打开即可。OpenOffice是一款开源的办公软件，可以直接去官网下载使用。如果处理数据的话，要学习下OpenOffice，好多公式都与Office不同。

相对来说，我推荐方法二。虽然方法一不需要借助其他软件，但编码得来回转。 方法二虽然需要安装OpenOffice，但它能原生地支持UTF-8格式的csv文件的解析与保存，满足强迫症患者。

## 制作csv文件

在dms投入使用之前，需要导入全校的宿舍数据，从后勤处获得的是xls文件，数据是这样的：
![image](https://cloud.githubusercontent.com/assets/11898075/23133933/da3f8c6a-f7cd-11e6-9104-d0506d9e9c03.png)

而我设计的dorm表字段是这样的：    
![image](https://cloud.githubusercontent.com/assets/11898075/23134264/179c47a0-f7cf-11e6-9344-931ebdf9202b.png)

开发dms时，我一般是在SublimeText里徒手写几行宿舍测试数据，然后保存为csv文件，通过phpmyadmin把csv导入到数据表中。
近3000行的Excel数据，如何转换为csv文件呢？ 我依旧是借用OpenOffice，把步骤分为以下4步：

- 第一步是直接copy一份源文件，保护原样本
- 第二步，利用Excel公式，提取所需字段，制作所需字段列，并进行dorm_num字段的拼接    
![image](https://cloud.githubusercontent.com/assets/11898075/23134430/c5ed1faa-f7cf-11e6-95a9-e36565d56195.png)

- 第三步，将第二步得到的目标字段列复制到一个新文件中，只保留值，不复制公式
- 第四步，将第三步得到的文件内容全选，直接复制到OpenOffice中，移动列的顺序和数据表对应，保存为csv

最后得到的文件如下：    
![image](https://cloud.githubusercontent.com/assets/11898075/23134502/fef16a5e-f7cf-11e6-8928-cbe824601031.png)

从上图也可以看出：
- 存放相同的数据，xlsx的文件体积比xls小
- csv文件是单纯的文本序列，占用空间更小

可以用SublimeText打开保存后的csv文件，验证数据的完整性。

## 总结

wikipedia上说：
>  The CSV file format is not standardized. The basic idea of separating fields with a comma is clear, but that idea gets complicated when the field data may also contain commas or even embedded line-breaks. CSV implementations may not handle such field data, or they may use quotation marks to surround the field. Quotation does not solve everything: some fields may need embedded quotation marks, so a CSV implementation may include escape characters or escape sequences.

虽然csv文件没有被标准化，但我认为csv文件在存储少量数据时非常有用。逗号作为字段分隔符，更容易被脚本语言结构化解析。同时csv文件是纯文本文件，更符合KISS原则，不需要额外工具，人很容易读、写、编辑，数据都是透明的。

愿Excel对csv更加友好~








