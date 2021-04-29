实验目的：  
了解Oracle表和视图的概念，学习使用SQL语句Create Table创建表，学习Select语句插入，修改，删除以及查询数据，学习使用SQL语句创建视图，学习部分存储过程和触发器的使用。  

实验场景：  
假设有一个生产某个产品的单位，单位接受网上订单进行产品的销售。通过实验模拟这个单位的部分信息：员工表，部门表，订单表，订单详单表。  

实验内容：  
录入数据：  
要求至少有1万个订单，每个订单至少有4个详单。至少有两个部门，每个部门至少有1个员工，其中只有一个人没有领导，一个领导至少有一个下属，并且它的下属是另一个人的领导（比如A领导B，B领导C）。  
1.创建表  
![QQ截图20210413104631](https://user-images.githubusercontent.com/44339160/116525334-fd567180-a90a-11eb-837c-cc7a7c9f91a8.png)
![QQ截图20210413104700](https://user-images.githubusercontent.com/44339160/116525350-00516200-a90b-11eb-9f87-a38419f35ced.png)
![QQ截图20210413104740](https://user-images.githubusercontent.com/44339160/116525374-08a99d00-a90b-11eb-97bd-aaade92bd5c9.png) 
![QQ截图20210413104825](https://user-images.githubusercontent.com/44339160/116525862-8b325c80-a90b-11eb-8df6-bae79f44375c.png)
![QQ截图20210413104955](https://user-images.githubusercontent.com/44339160/116526256-fb40e280-a90b-11eb-8b95-9232f8549e49.png)
2.插入数据
![QQ截图20210413105021](https://user-images.githubusercontent.com/44339160/116526299-07c53b00-a90c-11eb-9a20-ab8a34c966b9.png)
![QQ截图20210413105150](https://user-images.githubusercontent.com/44339160/116526332-127fd000-a90c-11eb-99ec-a8ca44d741d2.png)

序列的应用：  
插入ORDERS和ORDER_DETAILS 两个表的数据时，主键ORDERS.ORDER_ID, ORDER_DETAILS.ID的值必须通过序列SEQ_ORDER_ID和SEQ_ORDER_ID取得，不能手工输入一个数字。  
![QQ截图20210413104938](https://user-images.githubusercontent.com/44339160/116525951-a69d6780-a90b-11eb-8782-ad21142b3fda.png)

触发器的应用：  
维护ORDER_DETAILS的数据时（insert,delete,update）要同步更新ORDERS表订单应收货款ORDERS.Trade_Receivable的值。  
![QQ截图20210413104853](https://user-images.githubusercontent.com/44339160/116526050-c16fdc00-a90b-11eb-8f05-fe2bf9725fb5.png)
![QQ截图20210413104907](https://user-images.githubusercontent.com/44339160/116526054-c2a10900-a90b-11eb-9ed2-730748a83a6e.png)
![QQ截图20210413104925](https://user-images.githubusercontent.com/44339160/116526058-c3d23600-a90b-11eb-8a03-46e8c4d4e336.png)

查询数据：  
1.查询某个员工的信息。  
```sql
select * from employees where employee_id=11
```
![QQ截图20210429172118](https://user-images.githubusercontent.com/44339160/116530070-30e7ca80-a910-11eb-89ff-2e6711bd3c58.png)
2.递归查询某个员工及其所有下属，子下属员工。  
```sql
WITH A (EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID) AS
  (SELECT EMPLOYEE_ID,NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,SALARY,MANAGER_ID,DEPARTMENT_ID
    FROM employees WHERE employee_ID = 11
    UNION ALL
  SELECT B.EMPLOYEE_ID,B.NAME,B.EMAIL,B.PHONE_NUMBER,B.HIRE_DATE,B.SALARY,B.MANAGER_ID,B.DEPARTMENT_ID
    FROM A, employees B WHERE A.EMPLOYEE_ID = B.MANAGER_ID)
SELECT * FROM A;
```
![QQ截图20210429172152](https://user-images.githubusercontent.com/44339160/116530116-3d6c2300-a910-11eb-83e2-5869bab88266.png)
3.查询订单表，并且包括订单的订单应收货款: Trade_Receivable= sum(订单详单表.ProductNum*订单详单表.ProductPrice)- Discount。  
```sql
select orders.customer_name,orders.customer_tel,product_type from (
select products.product_type,order_id from order_details left join products on order_details.product_name=products.product_name)a
left join orders on orders.order_id=a.order_id
```
![QQ截图20210429172217](https://user-images.githubusercontent.com/44339160/116530199-57a60100-a910-11eb-8950-912fb17f6162.png)
4.查询订单详表，要求显示订单的客户名称和客户电话，产品类型用汉字描述。  
```sql
select orders.order_id from orders where order_id not in(
select order_id from order_details)
```
![QQ截图20210429172444](https://user-images.githubusercontent.com/44339160/116530241-65f41d00-a910-11eb-939e-fa48ab574dc8.png)
5.查询出所有空订单，即没有订单详单的订单。  
```sql
select department_name,count(price) from departments left join(
select employees.department_id,order_id,price from employees left join(
select orders.employee_id, orders.order_id,price from orders left join( 
select order_id, product_num*product_price as price from order_details)a on a.order_id=orders.order_id)b on b.employee_id=employees.employee_id)c on c.department_id=departments.department_id
group by departments.department_name
```
![QQ截图20210429172500](https://user-images.githubusercontent.com/44339160/116530281-6f7d8500-a910-11eb-895a-37067afb6118.png) 

实验总结：  
通过本次实验，我了解了Oracle表和视图的概念，学习到了使用SQL语句Create Table创建表，利用Select语句插入，修改，删除以及查询数据，学习到了使用SQL语句创建视图，学习到了部分存储过程和触发器的使用。同时，加强了自己对Oracle数据库的理解，对于自己的动手操作能力也有一定程度的提升。在实验过程中，我遇到了许多问题，但经过老师与同学们的帮助下，我成功解决了这些问题，十分感谢老师与同学的帮助。
