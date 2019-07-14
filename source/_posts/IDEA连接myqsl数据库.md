---
title: IDEA连接myqsl数据库
date: 2019-05-24 19:11:00
tags:
    - JAVA
categories:
    - JAVA
---
第一步
![](https://i.loli.net/2019/05/24/5ce7d4f3786a726517.png)
然后右边就会出现一个database
![](https://i.loli.net/2019/05/24/5ce7d1b46e03017489.png)

这个地方可能要更新一下驱动，idea 有自带更新，点一下下面那个 MySQL 就行
![](https://i.loli.net/2019/05/24/5ce7d28eb4d3585811.png)
![](https://i.loli.net/2019/05/24/5ce7d2b8eafc561716.png)
添加连接数据库的Java包
![](https://i.loli.net/2019/06/18/5d08870b8b73460833.png)
选择找到的包
![](https://i.loli.net/2019/06/18/5d0887761e75d15200.png)
添加好后应该会在下面多一个这样的包
连接数据库就成功了
测试一下，具体测试根据你自己数据库来，后面我打有注释
![](https://i.loli.net/2019/05/24/5ce7d39876cc433865.png)
``` java
package longpf;

import java.sql.*;

public class My {
    public static void main(String[] args) {
        Connection conn = null; //建立一个数据库连接
  try {
            Class.forName(
                    "com.mysql.jdbc.Driver"); //加载驱动
  System.out.println("数据库驱动加载成功");
 long start=System.currentTimeMillis(); //记录开始时间

  conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/mygamedb","root","root"); //连接数据库，参数数据库位置，用户名 密码
  long end=System.currentTimeMillis(); // 记录结束时间
  System.out.println(conn); // 打印连接
  System.out.println("建立用时："+(end-start)+"ms");
  // 创建Statement对象
  Statement stmt = conn.createStatement();
  // 执行SQL语句
  ResultSet rs = stmt.executeQuery("select * from users");
  System.out.println("id\tusername\tpwd\t\tregTime");
 while (rs.next()) {
                System.out.println(rs.getInt(1) + "\t" + rs.getString(2)
                        + "\t\t" + rs.getString(3));
  }
        } catch (Exception e) {
            e.printStackTrace();
  }
        return;
  }
}
```