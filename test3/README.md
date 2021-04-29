实验目的：  
掌握分区表的创建方法，掌握各种分区方式的使用场景。

实验内容：  
本实验使用3个表空间：USERS,USERS02,USERS03。在表空间中创建两张表：订单表(orders)与订单详表(order_details)。  
使用**你自己的账号创建本实验的表**，表创建在上述3个分区，自定义分区策略。  
你需要使用system用户给你自己的账号分配上述分区的使用权限。你需要使用system用户给你的用户分配可以查询执行计划的权限。  
表创建成功后，插入数据，数据能并平均分布到各个分区。每个表的数据都应该大于1万行，对表进行联合查询。  
写出插入数据的语句和查询数据的语句，并分析语句的执行计划。  
进行分区与不分区的对比实验。

实验步骤及结果：  
在主表orders和从表order_details之间建立引用分区  
在study用户中创建两个表：orders（订单表）和order_details（订单详表），两个表通过列order_id建立主外键关联。orders表按范围分区进行存储，order_details使用引用分区进行存储。  
1.首先创建自己的账号zjl_res_user，然后以system身份登录。  
![QQ截图20210329165017](https://user-images.githubusercontent.com/44339160/112825938-ebd93a00-90be-11eb-865d-25083482be8a.png)

2.然后以自己的账号zjl_res_user身份登录,并创建orders表与order_details表：  
$sqlplus zjl_res_user/123@202.115.82.8/pdborcl  
![QQ截图20210329171956](https://user-images.githubusercontent.com/44339160/112826006-027f9100-90bf-11eb-8d8f-f580d89c0cf0.png)

3.以system用户运行：  
![QQ截图20210329172132](https://user-images.githubusercontent.com/44339160/112826011-03b0be00-90bf-11eb-835d-f29c978644ba.png)
![QQ截图20210329172254](https://user-images.githubusercontent.com/44339160/112826229-4a9eb380-90bf-11eb-8c4e-2835a8e74001.png)

4.查看数据库的使用情况  
以下样例查看表空间的数据库文件，以及每个文件的磁盘占用情况。  
![QQ截图20210329172352](https://user-images.githubusercontent.com/44339160/112826281-5be7c000-90bf-11eb-8245-8b018ba0a97b.png)
![QQ截图20210329172413](https://user-images.githubusercontent.com/44339160/112826284-5e4a1a00-90bf-11eb-9b32-68e506d34ea7.png)
实验总结：  
通过本次实验，我熟练掌握了分区表的创建方法，掌握了各种分区方式的使用场景。同时，加强了自己对Oracle数据库的理解，对于自己的动手操作能力也有一定程度的提升。在实验过程中，我遇到了许多问题，但经过老师与同学们的帮助下，我成功解决了这些问题，十分感谢老师与同学的帮助。
