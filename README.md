# SQLyog Community - 64 bit
53.为什么在软件开发中需要编写模块设计概要文档？
正确答案：编写模块设计概要文档能清晰记录模块的功能和设计思路，便于团队沟通理解，降低开发风险，同时为代码实现提供指导和参考。
54.在SQL中，如何使用JOIN操作连接两个表？
正确答案：在SQL中，可以使用JOIN操作连接两个表，例如：`SELECT * FROM table1 JOIN table2 ON table1.common_column = table2.common_column`，这将基于共同的列名`common_column`合并table1和table2的数据。

55.解释什么是REVOKE语句在数据库权限管理中的作用。
正确答案：REVOKE语句用于撤销已经授予用户的特定权限，帮助管理者控制和更新用户访问数据库资源的权限。

56.在选择自动化测试框架时，应考虑哪些关键因素？
正确答案：选择自动化测试框架时要考虑的因素包括：兼容性（与项目技术栈的匹配）可扩展性、社区支持学习曲线文档质量以及对持续集成/持续部署(CI/CD)的支持。
57.职业理想在编程实践中如何体现？
正确答案：体现在持续学习新技术，追求代码质量，关注用户体验，以及在项目中考虑安全隐私和可持续性等方面。

五、论述题
58.请阐述反汇编调试工具在程序性能分析中的作用和方法。
正确答案：反汇编调试工具在程序性能分析中起到关键作用，它能帮助程序员查看和理解底层机器代码的执行效率。通过分析汇编代码，可以识别出可能导致性能瓶颈的指令序列，如冗余计算内存访问模式不良等。程序员可以通过调整算法优化数据结构或利用特定硬件特性来改进这些代码段，从而提升程序运行速度和资源利用率。此外，反汇编工具还能配合内存和CPU使用情况的监控，进一步定位和解决问题。
59.论述在计算机程序设计员培训中，如何系统地进行培训效果的后期跟进和持续改进？
正确答案：在计算机程序设计员培训中，首先，通过定期的技能测试和项目实践来跟踪学员的进步；其次，收集反馈，了解培训内容的实用性及理解难度；然后，根据反馈调整教学方法和内容，确保与时俱进；最后，建立长期的学习支持机制，如答疑平台在线资源更新，以持续提升学员的技能水平。

扎气球……len(nums)，[1] + nums + [1]，left >= right - 1，val[left] * val[i] * val[right]，solve(left, i) + solve(i, right)

……B、A[i]，E、C[k]+1，G、1，I、k和C[k]，C、3，2 4，0

……C、试探法，B、回溯法，D、对分查找法，E、归纳法，A、演绎法

拱形回文数……C、A[n-i+1] ，E、T&&A[i]>0，F、T  ，  I、n  ， F、T

需求工程分为……B、需求管理，D、变更控制，G、版本控制，F、需求跟踪，E、需求状态跟踪

软件在它的生命周期……bug，软件错误，软件缺陷，软件故障，软件失效

随着软件开发的不断发展……版本控制，变更控制，团队支持，审计控制，构造支持
扑克牌……this，face，4，new DeckOfCards()，player
软件开发工具是……描述客观系统，存储和管理开发工程中的信息，代码的编写或生成，文档的编制或生成，软件工程管理

##前期准备。

use supermarket;

##1、视图的操作 （共8分）

CREATE VIEW v_goods_sell_sum AS

SELECT t1.goods_id,t1.goods_name,t1.sum_num,t1.sum_amount,t2.supplier_id,t2.warehouse_id  

FROM

(SELECT goods_id,goods_name,SUM(goods_number) AS sum_num ,SUM(sell_goods_amount) AS sum_amount            #(2分)

FROM sell_list_info

WHERE SUBSTR(sell_date,1,10)='2023-01-16' #(1分)

GROUP BY goods_id ,goods_name ) t1    #(1分)

LEFT JOIN goods_info t2  ON  t1.goods_id =t2.goods_id    #(2分)

ORDER BY sum_num DESC;   #(2分)

注意：WHERE SUBSTR(sell_date,1,10)='2023-01-16' GROUP BY goods_id ,goods_name ) t1也可以改成where sell_date=’2023_01_16’ group by  goods_id,goods_name) t1

## 2、新建用户并授权访问

2.1连接数据库后，进入SQL命令界面。创建用户user002,密码为database@123。

create user 'user002'@'%' identified by 'database123';    #(2分)

注意： 创建用户名为user002，密码是database123的用户，'user002'@'%' 里的%是指允许从任何主机（'%'）连接；如果要是指定主机的话可以把%换为要指定的IP即可   

2.2 给用户user002授予supermarket数据库下的订货单信息表(order_info)的查询和插入权限。

grant select, insert  on supermarket.order_info   to 'user002'@'%';   #(2分) @'%'可不写

   授予用户名为user002，数据库为` supermarket `的查询、插入、更新和删除操作的权限。（supermarket 是数据库名称，可指定）;
   
   如果想授予新用户在所有数据库和所有表上选择和锁定表的权限的话可改、 supermarket.* 为  *.* 即可
   
2.3刷新权限缓存。

  flush privileges;      #(2分) 
  
3.触发器的使用

下架一件商品，需要先删除该款手机的进货和销售记录（删除“purchase_goods_info表”，order_info表，和sell_list_info表的记录),请根据要求创建触发器trig_del_goods

DELIMITER $

CREATE TRIGGER trig_del_goods  BEFORE DELETE  ON   goods_info  #2(分)

FOR EACH ROW 

BEGIN

SET @id='CS001';              #1(分) 

DELETE FROM purchase_goods_info WHERE goods_id=id;    #1(分)

DELETE FROM order_info WHERE goods_id=id;              #1(分)

DELETE FROM sell_list_info WHERE goods_id=id;             #1(分)

END $

DELIMITER ;

