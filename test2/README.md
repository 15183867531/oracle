实验目的：  
掌握用户管理、角色管理、权根维护与分配的能力，掌握用户之间共享对象的操作技能。  

实验内容:  
Oracle有一个开发者角色resource，可以创建表、过程、触发器等对象，但是不能创建视图。本训练要求：  

在pdborcl插接式数据中创建一个新的本地角色con_res_view，该角色包含connect和resource角色，同时也包含CREATE VIEW权限，这样任何拥有con_res_view的用户就同时拥有这三种权限。  
创建角色之后，再创建用户new_user，给用户分配表空间，设置限额为50M，授予con_res_view角色。  
最后测试：用新用户new_user连接数据库、创建表，插入数据，创建视图，查询表和视图的数据。  

实验步骤及结果:  

第1步：以system登录到pdborcl，创建角色con_res_view和用户new_user，并授权和分配空间：   
![QQ截图20210322212032](https://user-images.githubusercontent.com/44339160/112001044-871b5e00-8b59-11eb-867c-c94db2f150a5.png)

第2步：新用户zjl_res_user连接到pdborcl，创建表mytable和视图myview，插入数据，最后将myview的SELECT对象权限授予hr用户。  
![QQ截图20210322212624](https://user-images.githubusercontent.com/44339160/112002200-9bac2600-8b5a-11eb-802b-b3b222eb7b50.png)

第3步：用户hr连接到pdborcl，查询zjl_res_user授予它的视图myview
![QQ截图20210322212721](https://user-images.githubusercontent.com/44339160/112002246-a5ce2480-8b5a-11eb-9bad-c341ab7371a9.png)

第4步：查看数据库的使用情况  
 ![QQ截图20210322212934](https://user-images.githubusercontent.com/44339160/112002263-a9fa4200-8b5a-11eb-93d8-bd74a67876d2.png)

实验总结：  
通过本次实验，我熟练掌握了用户管理、角色管理、权根维护与分配的能力，掌握并用户之间共享对象的操作技能。同时，加强了自己对Oracle数据库的理解。在实验过程中，我遇到了许多问题，但经过老师与同学们的帮助下，我成功解决了这些问题。
