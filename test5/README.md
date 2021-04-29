实验目的：  
  了解PL/SQL语言结构  
  了解PL/SQL变量和常量的声明和使用方法  
  学习条件语句的使用方法  
  学习分支语句的使用方法  
  学习循环语句的使用方法  
  学习常用的PL/SQL函数  
  学习包，过程，函数的用法。  
  
实验场景：  
假设有一个生产某个产品的单位，单位接受网上订单进行产品的销售。通过实验模拟这个单位的部分信息：员工表，部门表，订单表，订单详单表。  
本实验以实验四为基础  

实验内容：  
创建一个包(Package)，包名是MyPack。  
```sql
create or replace PACKAGE MyPack IS
  FUNCTION SaleAmount(departmen_id number) RETURN NUMBER;
END MyPack;
```
![QQ截图20210429173831](https://user-images.githubusercontent.com/44339160/116535075-ec5f2d80-a915-11eb-8225-974d1770b201.png)
在MyPack中创建一个函数SaleAmount ，查询部门表，统计每个部门的销售总金额，每个部门的销售额是由该部门的员工(ORDERS.EMPLOYEE_ID)完成的销售额之和。函数SaleAmount要求输入的参数是部门号，输出部门的销售金额。
```sql
create or replace PACKAGE BODY MyPack IS
  FUNCTION SaleAmount(departmen_id NUMBER) RETURN NUMBER
  AS
    sales_amount NUMBER(20,2); 
    BEGIN
      select sum(product_num*product_price) into sales_amount from order_details where order_id in(
        select order_id from orders where employee_id in(
            select employee_id from employees where department_id in (DEPARTMENT_ID)));
      RETURN sales_amount;
    END;
END MyPack;
```
![QQ截图20210429173858](https://user-images.githubusercontent.com/44339160/116535091-eff2b480-a915-11eb-90cc-48e32fa9e975.png)
在MyPack中创建一个过程，在过程中使用游标，递归查询某个员工及其所有下属，子下属员工。过程的输入参数是员工号，输出员工的ID,姓名，销售总金额。信息用dbms_output包中的put或者put_line函数。输出的员工信息用左添加空格的多少表示员工的层次（LEVEL）。比如下面显示5个员工的信息：
```sql
create or replace PACKAGE BODY MyPack IS
  FUNCTION SaleAmount(departmen_id NUMBER) RETURN NUMBER
  AS
    sales_amount NUMBER(20,2); 
    BEGIN
      select sum(product_num*product_price) into sales_amount from order_details where order_id in(
        select order_id from orders where employee_id in(
            select employee_id from employees where department_id in (DEPARTMENT_ID)));
      RETURN sales_amount;
    END;
  
  PROCEDURE GET_EMPLOYEES(V_EMPLOYEE_ID NUMBER)
  AS
    LEFTSPACE VARCHAR(2000);
    begin
      --通过LEVEL判断递归的级别
      LEFTSPACE:=' ';
      --使用游标
      for v in
      (SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees
      START WITH EMPLOYEE_ID = V_EMPLOYEE_ID
      CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID)
      LOOP
        DBMS_OUTPUT.PUT_LINE(LPAD(LEFTSPACE,(V.LEVEL-1)*4,' ')||V.EMPLOYEE_ID||' '||v.NAME);
      END LOOP;
    END;
  END MyPack;
/
```
![QQ截图20210429175741](https://user-images.githubusercontent.com/44339160/116535102-f41ed200-a915-11eb-8e66-e200e312280a.png)
由于订单只是按日期分区的，上述统计是全表搜索，因此统计速度会比较慢，如何提高统计的速度呢？
Oracle递归查询：
```sql
SELECT LEVEL,EMPLOYEE_ID,NAME,MANAGER_ID FROM employees 
START WITH EMPLOYEE_ID = V_EMPLOYEE_ID 
CONNECT BY PRIOR EMPLOYEE_ID = MANAGER_ID
```
![QQ截图20210429175929](https://user-images.githubusercontent.com/44339160/116535198-0c8eec80-a916-11eb-9446-abc85bf2dc05.png)

实验总结：  
通过本次实验，我了解了PL/SQL语言结构，了解了PL/SQL变量和常量的声明和使用方法；学习了条件语句的使用方法、分支语句的使用方法、循环语句的使用方法、常用的PL/SQL函数，也学习了包，过程，函数的用法。同时，加强了自己对Oracle数据库的理解，对于自己的动手操作能力也有一定程度的提升。在实验过程中，我遇到了许多问题，但经过老师与同学们的帮助下，我成功解决了这些问题，十分感谢老师与同学的帮助。
