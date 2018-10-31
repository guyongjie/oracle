
实验2：用户及权限管理

## 实验目的：

掌握用户管理、角色管理、权根维护与分配的能力，掌握用户之间共享对象的操作技能。

## 实验内容：
Oracle有一个开发者角色resource，可以创建表、过程、触发器等对象，但是不能创建视图。本训练要求：
- 在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。
- 创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。
- 最后测试：用新用户new_user连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。

## 实验步骤

对于以下的对象名称con_res_view，new_user，在实验的时候应该修改为自己的名称。

- 第1步：以system登录到pdborcl，创建角色con_res_view_gyj1和用户new_user_gyj1，并授权和分配空间：

```sql

SQL> CREATE ROLE con_res_view_gyj1;

SQL> GRANT connect,resource,CREATE VIEW TO con_res_view_gyj1;

SQL> CREATE USER new_user_gyj1 IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;

SQL> ALTER USER new_user_gyj1 QUOTA 50M ON users;

SQL> GRANT con_res_view_gyj1 TO new_user_gyj1;
.
SQL> exit
```
> 语句“ALTER USER new_user_gyj QUOTA 50M ON users;”是指授权new_user_gyj用户访问users表空间，空间限额是50M。
![运行结果](https://github.com/guyongjie/oracle/blob/master/test2/1.png)

- 第2步：新用户new_user_gyj连接到pdborcl，创建表nametable和视图nameview，插入数据，最后将nameview的SELECT对象权限授予hr用户。

```sql

SQL> show user;

SQL> CREATE TABLE nametable (id number,name varchar(50));

SQL> INSERT INTO nametable(id,name)VALUES(1,'wang');

SQL> INSERT INTO nametable(id,name)VALUES (2,'gu');

SQL> CREATE VIEW nameview AS SELECT name FROM nametable;

SQL> SELECT * FROM nameview;

SQL> GRANT SELECT ON nameview TO hr;

SQL>exit
```
![运行结果](https://github.com/guyongjie/oracle/blob/master/test2/2.png)
- 第3步：用户hr连接到pdborcl，查询new_user_gyj1授予它的视图myview

```sql

SQL> SELECT * FROM new_user.nameview;

SQL> exit
```
![运行结果](https://github.com/guyongjie/oracle/blob/master/test2/3.png)
