# SQLyog Community - 64 bit
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

