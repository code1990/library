##（一）Hive建表

	create table student(
		Sno int,
		string,
		Sex string,
		Sage int,
		Sdept string
	)row format delimited fields terminated by ','stored as textfile;
	create table course(
		Cno int,
		Cname string
	) row format delimited fields terminated by ',' stored as textfile;
	create table sc(
		Sno int,
		Cno int,
		Grade int
	)row format delimited fields terminated by ',' stored as textfile;




##（二）Hive装载数据

	load data local inpath '/home/hadoop/hivedata/students.txt' 
	overwrite into table student;
	load data local inpath '/home/hadoop/hivedata/sc.txt' 
	overwrite into table sc;
	load data local inpath '/home/hadoop/hivedata/course.txt' 
	overwrite into table course;
##（三）DDL

	查询全体学生的学号与姓名
		select Sno, Sname from student;
	查询选修了课程的学生姓名
		select Sname from student s where s.Sdept is not null;
	----hive的group by 和集合函数
	
	查询学生的总人数
		select count(*) from student;
	计算1号课程的学生平均成绩
		select avg(s.Grade) from sc s where s.Grade = 1;
	查询各科成绩平均分
		
	查询选修1号课程的学生最高分数
	　　
	求各个课程号及相应的选课人数 
	
	查询选修了3门以上的课程的学生学号
	
	----hive的Order By/Sort By/Distribute By
	查询学生信息，结果按学号全局有序
	
	查询学生信息，结果区分性别按年龄有序
	
	----Join查询,join只支持等值连接
	查询每个学生及其选修课程的情况
	　　hive> select student.*,sc.* from student join sc on (student.Sno =sc.Sno);
	查询学生的得分情况。
	　　hive>select student.Sname,course.Cname,sc.Grade from student join sc 
	on student.Sno=sc.Sno join course on sc.cno=course.cno;
	查询选修2号课程且成绩在90分以上的所有学生。
	　　
	----LEFT，RIGHT 和 FULL OUTER JOIN
	查询所有学生的信息，如果在成绩表中有成绩，则输出成绩表中的课程号
	　　
	----LEFT SEMI JOIN  Hive当前没有实现 IN/EXISTS 子查询，
		可以用 LEFT SEMI JOIN 重写子查询语句。
	重写以下子查询
	  SELECT a.key, a.value
	  FROM a
	  WHERE a.key in
	   (SELECT b.key
	    FROM B);
	
	查询与“刘晨”在同一个系学习的学生
	　　hive> select s1.Sname from student s1 left semi join 	
	student s2 on s1.Sdept=s2.Sdept and s2.Sname='刘晨';

##（四）分桶表示例：

show databases;

show tables;

desc test;
	
	set mapreduce.job.reduces=4;
	
	drop table stu_buck;
	create table stu_buck(
		Sno int,
		Sname string,
		Sex string,
		Sage int,
		Sdept string
    )
	clustered by(Sno) 
	sorted by(Sno DESC)
	into 4 buckets
	row format delimited
	fields terminated by ',';


	insert overwrite table student_buck
	select * from student cluster by(Sno) sort by(Sage);  
	--报错,cluster 和 sort 不能共存


	
	insert into table stu_buck
	select Sno,Sname,Sex,Sage,Sdept 
	from student distribute by(Sno) sort by(Sno asc);
	
	insert overwrite table stu_buck
	select * from student distribute by(Sno) sort by(Sno asc);
	
	insert overwrite table stu_buck
	select * from student cluster by(Sno);

##（五）多重插入

	from student
	insert into table student_p partition(part='a')
	select * where Sno<95011;
	insert into table student_p partition(part='a')
	select * where Sno<95011;

##（六）导出数据到本地
	insert overwrite local directory '/home/hadoop/student.txt'
	select * from student;
##（七）UDF案例
	create table rat_json(line string) row format delimited;
	load data local inpath '/home/hadoop/rating.json' into table rat_json;
	
	drop table if exists t_rating;
	create table t_rating(
		movieid string,
		rate int,
		timestring string,
		uid string
	) 	row format delimited fields terminated by '\t';
	
	insert overwrite table t_rating
	select split(parsejson(line),',')[0]as 
		movieid,split(parsejson(line),',')[1] as rate,
		split(parsejson(line),',')[2] as timestring,
		split(parsejson(line),',')[3] as uid from rat_json limit 10;
##（八）内置jason函数

	select get_json_object(line,'$.movie') as moive,
	get_json_object(line,'$.rate') as rate  
	from rat_json limit 10;

##（九）transform案例

	INSERT OVERWRITE TABLE u_data_new
	SELECT
	  TRANSFORM (movieid, rate, timestring,uid)
	  USING 'python weekday_mapper.py'
	  AS (movieid, rate, weekday,uid)
	FROM t_rating;
	
	select distinct(weekday) from u_data_new limit 10;

##（十）hive分区有关的操作

	-- 创建分区表
	create table t_sz_part(id int, name string)
	partitioned by(country string)
	row format delimited
	fields terminated by ',';
	--装载数据
	load data local inpath '/home/hadoop/sz.dat' 
		into table t_sz_part partitioned(country='China');
	--查看分区表
	show partitions t_sz_part；
	-- 添加分区
	alter table t_sz_part add partition(country='america');
	--删除分区
	alter table t_sz_part drop partition(country='america');