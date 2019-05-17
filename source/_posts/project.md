---
title: 留学申请与审核软件
categories: slides
date: 2019-05-16 13:02:32
tags: hexo
layout: slides
slide:
  theme: night
---

### 留学申请与审核软件 <small>v5.2</small>
<!-- .slide: data-background="#1B9EF3" -->


===
<!-- .slide: data-transition="convex" data-background="#C7916B" -->
### 数据库部署在服务器
 - 只要有网, 有软件就能使用.
 - 同步更改, 能事实显示服务器的更改(比如增添学校)
 - 支持多人同时在线

===
<!-- .slide: data-transition="fade" data-background="#00C4B6" -->
![软件设计框架](https://i.loli.net/2019/05/17/5cde9745ddbac11921.png)

===

### 登录界面
<!-- .slide: data-transition="zoom" data-background="#F47466" -->

1. 提供用户名密码进行登录数据库, 
代码中不含有密码(安全性)
```python
try:
    self.db = mysql.connector.connect(
        host="142.93.91.173",
        user=username,
        passwd=password,
        database="app"
    )
    self.cur = self.db.cursor()
except mysql.connector.errors.ProgrammingError:
    return 'null'
```
2. 可以更改用户的密码
3. 若输入的用户名和密码不对，则无法登陆

===

### 申请表
<!-- .slide: data-transition="convex" data-background="#69C282" -->

1. 学生同时只能申请一个学校
2. 撤销后不允许再次申请同一所学校

3. 能上传任意格式的文件, 后续审核批复能够下载.
4. 学校和专业都是动态查询的, 能实时更改(添加学校,专业等).

==

### 查看申请动态

1. 查看本专员提交的申请表
2. 撤销申请表(之后不能再申请同一学校)
3. 可以查看审批和批复内容

===

### 审核专员窗口
<!-- .slide: data-transition="concave" data-background="#1B9EF3" -->

 - 实时更新的申请表
 - 下载申请表内的文件
 - 更改审核状态(合格或不合格) 

===

### 批复专员
<!-- .slide: data-transition="fade" data-background="#ff944d" -->

1. 下载学生资料
2. 实时更新的数据
3. 对申请表进行批复操作

===

### 管理员
<!-- .slide: data-transition="concave" data-background="#C7916B" -->

 - 能添加用户(包括三类角色), 可以登录.
 - 添加学校(能实时反映在申请表界面)
 - 更新学校专业

===
<!-- .slide: data-transition="convex" data-background="#F47466" -->

# Thanks