---
title: 汇总二维数据表，导出多维Excel表格
subtile: export multidimensional excel table from database 
tags: [JSON,PHP,Excel]

---

近期,需求方要求在[sams](https://github.com/kyshel/sams)中添加导出功能,需要导出的数据为考勤结果。    
考勤课程有多个，每个考勤课程的学生都是自定义添加的，要将结果导出为多张二维表，难以展示数据，那应该怎样组织并呈现数据？

联系到Github API的响应数据，k自然而然地想到了JSON.把数据库中需要的数据提取出来放到一个json对象中，把此json数据导出即可。

## 开始
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

## 添加学生及其考勤情况
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
## 完善
基本的json结构已经形成，一个json对象包含汇总了各门课程具体的考勤情况，即使每门课程数据结构不相同，也不影响数据的组织。完善其他数据后，把此json对象导出即可。目标数据结构如下：

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
## 制作JSON字符串
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
## JSON转xls

数据看起来还不错，但是读起来似乎没那么赏心悦目，可以通过json2table网站可以转化JSON为表格，可是JSON是由JS读入的，如何把PHP得到的JSON传入JS？

如果用PHP把JSON写入一个文本文件，再调用jQuery的getJSON方法去读取文件，就可以实现传递了。处理结果如下所示：
![table_example](https://cloud.githubusercontent.com/assets/23516161/20893166/c25578a8-bb4b-11e6-95dd-e0ab500ae837.png)

这样看的话数据就pretty了。最后加个导出按钮，方便一键导出成xls文件~





