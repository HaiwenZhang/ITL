e#Java 数据库编程(JDBC)

##1、MySQL数据库

```
create databse mydb;
use mydb;
CREATE TABLE customers ()
	custmerID char(8) primary key,
	name char(40)  default null,
	phone char(40) default null
);
INSERT INTO customers VALUES ('ADDIFK01','Frank Addinsell','(718) 555-3911');
```

## 2、JDBC编程基本步骤
###Java Application -----------MySQL(Database, DBMS)
最重要的是连接到数据库
##1)数据库访问驱动程序

mysql-connector-java-xxx.jar

##2)加载驱动程序

```
String driver="com.mysql.jdbc.Driver";
Class.forName(driver);
```
##3)创建连接对象Connection
```
String url = "jdbc:mysql://localhost:3036/mydb";
String user = "root";
String pwd="123456";
Connection conn  = DriverManager.getConnection(url,user,pwd);
```

##4)创建命令对象Statement 

```
Statement cmd = conn.createStatement();
String sql = "SELECT * FROM customers";
//执行SQL(select)语句, 返回结果集ResultSet
//cursor 游标 
ResultSet rs = cmd.executeQuery(sql);
```

##5)执行select 语句， 读取ResultSet内容
```
rs.next(); //将游标移动到下一行
while(rs.next()) {
	String cid = rs.getString(1); //当前行的第一列内容
	String cname = rs.getString(2);
	String cphone = rs.getString(3);
	System.out.println(cid + " " + cname + " " + cphone);
}
```

##6)关闭连接对象

```
conn.close();
```

#3、执行update, delete, insert等语句，写数据库，不需要返回结果集

```
String sql = "insert into customers values('1001', 'zhou', '021-1111)";
int x = cmd.executeUpdate(sql); //返回影响的行数
eg:

import java.sql.*;
public class CustomerDao {
	private static String driver = "com.mysql.jdbc.Driver";
	private static String url = "jdbc:mysql://localhost:3306/mydb", user = "root", pwd = "4846";
	private static Connection conn=null;
	static{
		try{
		  Class.forName(driver);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	private static void createConnection() {
		try{
		 conn = DriverManager.getConnection(url, user, pwd);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	private static void closeConnection() {
		try{
		 conn = DriverManager.getConnection(url, user, pwd);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	public static int addCustomer(String cid,String cname,String cphone) throws Exception{
		int r=0;
		createConnection();
		Statement cmd = conn.createStatement();
		String sql="insert into customers values('"+cid+"','"+cname+"','"+cphone+"')";
		r=cmd.executeUpdate(sql);//·µ»ØÓ°ÏìÐÐÊý
		closeConnection();
		return r;
	}
	public static int deleteCustomerByCid(String cid) throws Exception{
		int r=0;
		createConnection();
		Statement cmd = conn.createStatement();
		String sql="delete from customers where customerid='"+cid+"'";
		r=cmd.executeUpdate(sql);//·µ»ØÓ°ÏìÐÐÊý
		closeConnection();
		return r;
	}
	public static void queryAllCustomers() throws Exception{
		createConnection();
		Statement cmd = conn.createStatement();
		String sql="select * from customers";
		ResultSet rs=cmd.executeQuery(sql);
		while(rs.next()){
			System.out.printf("%-20s%-20s%-20s\n", rs.getString(1),rs.getString(2),rs.getString(3));
		}
		closeConnection();
	}
}
```

#4、数据库分层设计
##1)数据库访问工具
```
import java.sql.*;
public class SQLHelper {
	private static String driver = "com.mysql.jdbc.Driver";
	private static String url = "jdbc:mysql://localhost:3306/mydb", user = "root", pwd = "4846";
	private static Connection conn=null;
	static{
		try{
		  Class.forName(driver);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	private static void createConnection() {
		try{
		 conn = DriverManager.getConnection(url, user, pwd);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	public static void closeConnection() {
		try{
		 conn = DriverManager.getConnection(url, user, pwd);
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	public static int executeUpdate(String sql){
		int r=0;
		try{
			createConnection();
			Statement cmd = conn.createStatement();
			r=cmd.executeUpdate(sql);//·µ»ØÓ°ÏìÐÐÊý
			closeConnection();
		}catch(Exception ex){
			ex.printStackTrace();
		}
		return r;
	}
	public static ResultSet executeQuery(String sql){
		ResultSet rs=null;
		try{
			createConnection();
			Statement cmd = conn.createStatement();
			rs=cmd.executeQuery(sql);//·µ»ØÓ°ÏìÐÐÊý
		}catch(Exception ex){
			ex.printStackTrace();
		}
		return rs;
	}
}
```


##2)编写实体类(映射数据库中的表)

```
public class Customer {
	private String cid, cname, cphone;
	public Customer(){
	}
	public Customer(String cid, String cname, String cphone) {
		this.cid = cid;
		this.cname = cname;
		this.cphone = cphone;
	}

	public String getCid() {
		return cid;
	}

	public void setCid(String cid) {
		this.cid = cid;
	}

	public String getCname() {
		return cname;
	}

	public void setCname(String cname) {
		this.cname = cname;
	}

	public String getCphone() {
		return cphone;
	}

	public void setCphone(String cphone) {
		this.cphone = cphone;
	}
   }
```


##3)实体操作类(CustomerDao)
```
import java.util.*;
   import java.sql.ResultSet;

   public class CustomerDao {
	public static int addCustomer(String cid,String cname,String cphone) throws Exception{
		int r=0;
		String sql="insert into customers values('"+cid+"','"+cname+"','"+cphone+"')";
		r=SQLHelper.executeUpdate(sql);
		return r;
	}
	public static int deleteCustomerByCid(String cid){
		int r=0;
		String sql="delete from customers where customerid='"+cid+"'";
		r=SQLHelper.executeUpdate(sql);
		return r;
	}
	public static ArrayList<Customer> queryCustomers(){
		ArrayList<Customer> list=new ArrayList<Customer>();
		String sql="select * from customers";
		ResultSet rs=SQLHelper.executeQuery(sql);
		try{
		  while(rs.next()){
		       String cid= rs.getString(1);//µ±Ç°ÐÐµÄµÚÒ»ÁÐÄÚÈÝ
		       String cname=rs.getString(2);
		       String cphone=rs.getString(3);
		       Customer cus=new Customer(cid,cname,cphone);
		       list.add(cus);
	      }
		  SQLHelper.closeConnection();
		}
		catch(Exception ex){
			
		}
		return list;
	}
    }
```
##4)View界面层

```
 import java.util.*;
   public class DBDemo {
	
	public static void main(String[] args) throws Exception {
		ArrayList<Customer> list=CustomerDao.queryCustomers();
		for(Customer cus:list){
			System.out.println(cus.getCid());
		}
	}
   }

```


