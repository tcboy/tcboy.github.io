---
layout: post
title:  "在mysql中添加用户并授权"
date:   2014-07-30 17:30:00
categories: Code
---

1 -> 选择数据表

`use mysql;`

2 -> 增加用户

`GRANT USAGE ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

3 -> 授权

`GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON tablename.*  TO 'username'@'localhost' IDENTIFIED BY 'password';`

4 -> 刷新

`FLUSH PRIVILEGES;`


