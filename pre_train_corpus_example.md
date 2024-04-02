# 数据格式

## sql—Dataset({features: ['sql'],num_rows: 975420})
```
'sql': '<reponame>Dragontalker/MySQL-study-notes\n#视图\n\n/*\n含义: 虚拟表, 和普通表一样使用\nmysql5.1版本出现的新特性, 是通过表动态生成的数据\n\n比如: 舞蹈班和普通班的对比\n\n\t\t创建语法的关键字\t是否实际占用物理空间\t\t使用\n视图\t\tcreate table\t 只保存了sql逻辑\t\t增删改查, 一般不能增删改\n表\t\tcreate view\t\t   保准了数据ALTER\t增删改查\n*/\n\n#案例: 查询姓张的学生名和专业名\nSELECT stuname, major_name\nFROM stuinfo AS s\nINNER JOIN major AS m \nON s.major_id = m.id\nWHERE s.stuname LIKE \'%张\';\n\n#创建视图\nCREATE VIEW v1\nAS\nSELECT stuname, major_name\nFROM stuinfo AS s\nINNER JOIN major AS m\nON s.major_id = m.id;\n\n#使用视图\nSELECT * FROM v1 WHERE s.stuname LIKE \'%张\';\n\n#一、创建视图\n/*\n语法:\ncreate view 视图名\nas\n查询语句;\n*/\n\n#1. 查询姓名中包含a字符的员工名、部门名和工种新消息\n#(1)创建\nCREATE VIEW myv1\nAS\nSELECT last_name, department_name, job_title\nFROM employees AS e\nJOIN departments AS d\nON e.department_id = d.department_id\nJOIN jobs AS j\nON j.job_id = e.job_id;\n\n#(2)使用\nSELECT *\nFROM myv1\nWHERE last_name LIKE \'%a%\';\n\n#2. 查询各部门的平均工资级别\n#(1)创建视图查看每个部门的平均工资\nCREATE VIEW myv2\nAS\nSELECT AVG(salary) AS ag, department_id\nFROM employees\nGROUP BY department_id;\n\n#(2)使用\nSELECT myv2.ag, g.grade_level \nFROM myv2\nJOIN job_grades AS g\nON myv2.ag BETWEEN g.lowest_sal AND g.highest_sal;\n\n#3. 查询平均工资最低的部门信息\nSELECT * \nFROM myv2\nORDER BY ag\nLIMIT 1;\n\n#4. 查询平均工资最低的部门名和工资\nCREATE VIEW myv3\nAS\nSELECT * \nFROM myv2\nORDER BY ag\nLIMIT 1;\n\nSELECT d.*, m.ag\nFROM myv3 AS m\nJOIN departments AS d\nON m.department_id = d.department_id;\n\n#二、视图的修 
改\n/*\n方式一:\ncreate or replace view 视图名\nas\n查询语句;\n*/\n\nSELECT * FROM myv3;\n\nCREATE OR REPLACE VIEW myv3\nAS\nSELECT AVG(salary), job_id\nFROM employees\nGROUP BY job_id;\n\n/*\n方式二:\n语法:\nalter view 视图名\nas\n查询 
语句;\n*/\n\nALTER VIEW myv3\nAS\nSELECT * FROM myv2;\n\n#三、删除视图\n\n/*\n语法: drop view 视图名1, 视图名2, ...;\n*/\n\nDROP VIEW myv1, myv2, myv3;\n\n#四、查看视图\nDESC myv1;\n\nSHOW CREATE VIEW myv1;\n\n#五、视图的更新\nCREATE OR 
REPLACE VIEW myv1\nAS\nSELECT last_name, email, salary*12*(1+IFNULL(commission_pct, 0)) AS "annual salary"\nFROM employees;\n\nCREATE OR REPLACE VIEW myv1\nAS\nSELECT last_name, email\nFROM employees;\n\nSELECT * FROM myv1;\n\n#1. 插入\nINSERT INTO myv1\nVALUES (\'张飞\', \'<EMAIL>\');\n\n#2. 修改\nUPDATE myv1\nSET last_name = \'张无忌\'\nWHERE last_name = \'张飞\';\n\n#3. 删除\nDELETE FROM myv1\nWHERE last_name = \'张无忌\';\n\n#具备以下特点的视图不允许更新\n/*\n(1)包 
含以下关键字的slq语句\n\t- 分组函数\n    - distinct\n    - group by\n    - having\n    - union\n    - union all\n*/\n\nCREATE OR REPLACE VIEW myv1\nAS\nSELECT MAX(salary) as m, department_id\nFROM employees\nGROUP BY department_id;\n\nSELECT * FROM myv1;\n\n#更新\nUPDATE myv1 \nSET m = 9000\nWHERE department_id = 10;\n\n#(2)常量视图\nCREATE OR REPLACE VIEW myv2\nAS\nSELECT \'john\' NAME;\n\nUPDATE myv2\nSET NAME = \'lucy\';'
```

## text2code—Dataset({features: ['text', 'code'],num_rows: 1866910})
```
'text': 'How to convert a list of multiple integers into a single integer?', 'code': 'sum(d * 10 ** i for i, d in enumerate(x[::-1]))'
```

## text2text—Dataset({features: ['input_text', 'output_text'],num_rows: 1026340})
```
'input_text': 'Give three tips for staying healthy.\n', 'output_text': '1. Eat a balanced and nutritious diet: Make sure your meals are inclusive of a variety of fruits and vegetables, lean protein, whole grains, and healthy fats. This helps to provide your body with the essential nutrients to function at its best and can help prevent chronic diseases.\n\n2. Engage in regular physical activity: Exercise is crucial for maintaining strong bones, muscles, and cardiovascular health. Aim for at least 150 minutes of moderate aerobic exercise or 75 minutes of vigorous exercise each week.\n\n3. Get enough sleep: Getting enough quality sleep is crucial for physical and mental well-being. It helps to regulate mood, improve cognitive function, and supports healthy growth and immune function. Aim for 7-9 hours of sleep each night.'
```
