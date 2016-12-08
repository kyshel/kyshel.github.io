>关于sams，近期需求方要求添加导出功能，k有感而发写下了这篇post。


## Data Management

不同时间，人对数据的理解也不同。如何组织数据？用什么结构存储？

### Stage 1
在最初的认识中，数据是以二维表形式组织的，比如：

学号|姓名
-|-
101|亚索
102|盲僧
103|锐雯
104|锤石
105|炼金


### Stage 2
接触数据库后，二维表正好能对应数据库的表，一个数据库中有多个表。
把二维表的行看成x，列看成y，那么数据库又多了一个维度z。

通过外键，表与表之间增加了约束，这样表之间就不是单独存在，而是相互依赖的了，字段之间建立起了联系，有利于数据引用的准确性。
这个阶段中，我在dms中大量使用了外键，例如log表的user_name字段的外键为 user表的 user_name字段，如下图所示
![dms_users_logs_foreign_key](https://cloud.githubusercontent.com/assets/23516161/20884493/8f4788fe-bb26-11e6-95b7-0b03f00ef8d5.png)

其实这个外键定义的并不好，在后期使用中，我想删除一个用户但保存这个用户的日志，由于存在外键，这个想法并不合理，因为破坏了数据的完整性。所以外键的使用也分场合。

当然，像wordpress中并没有定义外键，看起来表与表中并没有联系，但实际上数据完整性同样可以通过后台逻辑来约束。


### Stage 3
sams也是用的传统的关系型数据库，但是在接触Json和面向对象的思想后，k不再满足于关系型数据库的结构，原因如下：

- 一方面，数据的联系是靠不同表的字段对应起来，一个relation就要涉及到两张表，对数据进行操作也要将两张表进行连接，有没有别的数据存储方法呢？
- 另一方面，数据的组织、提取要徒手写SQL语句，有其它简单的方法吗？（不可否认的是，标准SQL为不同的RDBMS提供了操作数据的统一接口）

对于后者的需求，ORM的出现解决了这个问题，它把数据转化为对象看待，在程序逻辑里操作对象就可以了，剩下的SQL语句由ORM代劳。

对于前者的需求，一种方案是面向文档的数据库，在关系型数据库中，键-值存储中“值”的部分，存储的是单纯的字符串；而面向文档的数据库“值”的部分是拥有结构的文档，文档的内容可以查询。典型代表如MongoDB。
还有一种方案是面向对象的数据库，它将面向对象语言中的对象直接进行永久保存。

目前还没有stage3相关的repo，考虑过年做个。

### Stage 4
>在进入云计算时代后，数据库服务器成为了云计算系统的瓶颈，有很多场景只是需要通过键来获取值这样简单的操作，并不一定要动用RDB。  
在这样的背景下，就出现了只管理键值对并将数据保存在内存中的缓存系统memcached。

数据库中的数据大多放在磁盘上，当对数据库进行查询时，操作开销很大，而memcached将查询结果缓存在内存中，这样可以高性能。

## SAMS Export
回到sams的数据导出问题上来，sams需要导出的数据为考勤结果。考勤课程有多个，每个考勤课程的学生都是自定义添加的。要将结果导出使用二维表的形式难以展示数据。如果用上面提到的面向文档或面向对象的数据库来重做，那么整个系统的逻辑也要重新写。那应该怎样组织并呈现数据？

联系到Github API的响应数据，k自然而然地想到了Json，先不改变数据库，把数据库中需要的数据提取出来放到一个json对象中，把此json数据导出即可。
像GET仓库列表返回的数据结构一样，把每个{考勤课程}放在数组中，每个考勤课程作为数组的元素，同时也是一个实体，就像这样：

``` json
[
	{
		"pro_id":1,
		"pro_name":"proA"
	},
	{
		"pro_id":2,
		"pro_name":"proB"
	},
	{
		"pro_id":3,
		"pro_name":"proC"
	}
]
```

备注：project语义化为 考勤课程，pro_id为 考勤课程编号，pro_name为 考勤课程名，数组中的每个对象看成是一个project实体

接下来，把考勤课程中的每个学生及对应的缺勤作为一个{学生对象}，放在数组attend[]中，作为project实体的一个属性：

``` json
[
	{
		"pro_id":1,
		"pro_name":"proA",
		"attend":[
			{
				"stu_id":101,
				"stu_name":"亚索",
				"no_sum":5
			},
			{
				"stu_id":102,
				"stu_name":"盲僧",
				"no_sum":8
			},
			{
				"stu_id":103,
				"stu_name":"锤石",
				"no_sum":3
			}		
		]
	},
	{
		"pro_id":2,
		"pro_name":"proB",
		"attend":[
			{
				"stu_id":102,
				"stu_name":"盲僧",
				"no_sum":9
			}
		]
	},
	{
		"pro_id":3,
		"pro_name":"proC",
		"attend":[
			{
				"stu_id":102,
				"stu_name":"盲僧",
				"no_sum":7
			},
			{
				"stu_id":103,
				"stu_name":"锤石",
				"no_sum":1
			}
		]
	}
]
```

这样基本的json结构已经形成，一个json对象包含汇总了各门课程具体的考勤情况，即使每门课程数据结构不相同，也不影响数据的组织。完善其他数据后，把此json对象导出即可。目标数据结构如下：

``` json 
{
	"time":"160101 000000",
	"info":"",
	"data":[
		{
			"pro_id": 1,
			"year": "",
			"term": "",
			"course_id": "",
			"course_name": "",
			"stu_grade": "",
			"stu_major": "",
			"tea_id": "",
			"tea_name": "",
			"hour": "",
			"last_update": "",
			"status": "",
			"off_time": "",
			"attend": [
				{
					"stu_id": "",
					"stu_name": "",
					"no_sum": 0
				},
				{
					"stu_id": "",
					"stu_name": "",
					"no_sum": 0
				}
			]
		},
		{
			"pro_id": 2,
			"year": "",
			"term": "",
			"course_id": "",
			"course_name": "",
			"stu_grade": "",
			"stu_major": "",
			"tea_id": "",
			"tea_name": "",
			"hour": "",
			"last_update": "",
			"status": "",
			"off_time": "",
			"attend": [
				{
					"stu_id": "",
					"stu_name": "",
					"no_sum": 0
				},
				{
					"stu_id": "",
					"stu_name": "",
					"no_sum": 0
				}
			]
		}
	]
}
```

在数据库中提取数据拼接出上面的json字符串并不难，php代码如下：

``` php
<?php
function getJson(){
	global $db;
	$project = array();
	$attend = array();
	$response = array();
	$sql="SELECT * from project";
	$result = $db->query($sql);
	if ($result->num_rows == 0) {
		echo "table project is empty!";
    } else {
		while($row = $result->fetch_array(MYSQLI_ASSOC)){
			$pro_id = $row['pro_id'];
			$sql2 = "SELECT * from attend WHERE pro_id = ".$pro_id;
			$result2 = $db->query($sql2);
			if ($result2->num_rows == 0) {
				echo "table attend is empty!";
			} else {
				while($row2 = $result2->fetch_array(MYSQLI_ASSOC)){
					$attend[] = array(
						'stu_id' => $row2['stu_id'],
						'stu_name' => getStuName($row2['stu_id']),
						'no_sum' => $row2['no_sum']
					);
				}
			}
			$project[] = array(
				'pro_id' => $row['pro_id'], 
				'year' => $row['year'],
				'term' => $row['term'],
				'course_id' => $row['course_id'],
				'course_name' => $row['course_name'],
				'stu_grade' => $row['stu_grade'],
				'stu_major' => $row['stu_major'],
				'tea_id' => $row['tea_id'],
				'tea_name' => $row['tea_name'],
				'hour' => $row['hour'],
				'last_update' => $row['last_update'],
				'status' => $row['status'],
				'off_time' => $row['off_time'],
				'attend' => $attend
			);
			$attend = array(); // empty attend
		}
	}
	$response['time'] = getNowTime() ;
	$response['info'] = 'this is info';
	$response['data'] = $project ;
	$json_str = json_encode($response, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
	return $json_str;
}
```

运行之后结果如下（数据就这么被组织起来了）：

``` json
{
    "time": "2016-11-29 22:55:30",
    "info": "this is info",
    "data": [
        {
            "pro_id": "17",
            "year": "2016-2017",
            "term": "1",
            "course_id": "B23012003",
            "course_name": "德语强化",
            "stu_grade": "2015",
            "stu_major": "不分专业",
            "tea_id": "t03173",
            "tea_name": "王老师",
            "hour": "128",
            "last_update": "2016-11-29 14:58:22",
            "status": "on",
            "off_time": "never",
            "attend": [
                {
                    "stu_id": "1507010312",
                    "stu_name": "亚索",
                    "no_sum": "1"
                },
                {
                    "stu_id": "1523040102",
                    "stu_name": "盲僧",
                    "no_sum": "6"
                },
                {
                    "stu_id": "1523040106",
                    "stu_name": "锐雯",
                    "no_sum": "2"
                },
                {"...":"..."}
            ]
        },
        {"...":"..."}
    ]
}
```

但是英文没有语义化，第一次查看的人可能会懵逼，所以应该把json中的key改成中文。
开始我想写一个转换函数getCnFromEnJson($en_json_str)，输入英文，输出中文，但如果以后数据库表结构改动的话，getJson()和getCnFromEnJson($en_json_str)都要改动。所以想了下，直接在getJson()里把key换成中文了，例如 `'year' => $row['year']` 直接改成 `'学年' => $row['year']` 改动后运行结果如下：

``` json
{
    "统计时间": "2016-12-01 17:25:02",
    "备注": "若旷课次数达到总课时的1\/6(一节大课为2个课时)，取消考试资格",
    "数据": [
        {
            "编号": "17",
            "学年": "2016-2017",
            "学期": "1",
            "课程编号": "B23012003",
            "课程名": "德语强化",
            "年级": "2015",
            "专业": "不分专业",
            "工号": "t03173",
            "教师名": "王老师",
            "学时": "128",
            "更新时间": "2016-11-30 14:23:45",
            "状态": "off",
            "结课时间": "2016-12-01 16:29:39",
            "列表": [
                {
                    "学号": "1507010312",
                    "姓名": "亚索",
                    "缺勤次数": "3"
                },
                {
                    "学号": "1523040102",
                    "姓名": "盲僧",
                    "缺勤次数": "6"
                },
                {
                    "学号": "1523040106",
                    "姓名": "锐雯",
                    "缺勤次数": "2"
                },
                {"...":"..."}
            ]
        },
        {"...":"..."}
    ]
}

```

看起来还不错。 

但是读起来似乎没那么赏心悦目，能否将json数据转换为表格？ 当然不是单纯的二维表，而是嵌套的表格，就像下面这样？

![json2table_example](https://cloud.githubusercontent.com/assets/23516161/20892470/5c333684-bb49-11e6-80ae-9551bd9a4074.png)

Absolutely! 上图就是利用json2table网站生成的表格，把目标数据通过json2table处理后结果如下所示：

![table_example](https://cloud.githubusercontent.com/assets/23516161/20893166/c25578a8-bb4b-11e6-95dd-e0ab500ae837.png)

这样看的话数据就pretty了。当然最后还要加个导出按钮，方便一键导出成xls文件~

## 总结
这一路折腾的不少，主线如下：

1. json数据结构制作
2. php代码实现
3. 生成真正的json文件
4. 把json转成html表格
5. 将html表格转成xls

-END-


