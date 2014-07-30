---
layout: post
title:  "Guava用法总结(实时更新)"
date:   2014-07-22 16:30:00
categories: Code
---

1 -> 选择数据表

`use mysql;`

2 -> 增加用户

`GRANT USAGE ON *.* TO 'username'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;`

3 -> 对数据库授权

`GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP ON tablename.*  TO 'username'@'localhost' IDENTIFIED BY 'password';`

4 -> 刷新

`FLUSH PRIVILEGES;`


