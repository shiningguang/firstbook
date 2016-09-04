## hibernate聊聊第二讲

 hibernate应用中需要用的依赖有：
 
 
```
	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate-core</artifactId>
		<version>${hibernate.version}</version>
	</dependency>

	<dependency>
		<groupId>org.hibernate</groupId>
		<artifactId>hibernate-ehcache</artifactId>
		<version>${hibernate.version}</version>
	</dependency>
	<dependency>
		<groupId>com.googlecode.genericdao</groupId>
		<artifactId>dao-hibernate</artifactId>
		<version>${dao-hibernate.version}</version> <!-- use current version -->
	</dependency>
  	<dependency>
	  	<groupId>org.hibernate</groupId>
	  	<artifactId>hibernate-entitymanager</artifactId>
	  	<version>${hibernate.version}</version>
  	</dependency>
  	<dependency>
	  	<groupId>org.hibernate</groupId>
	  	<artifactId>hibernate-commons-annotations</artifactId>
	  	<version>3.2.0.Final</version>
  	</dependency>
```

${hibernate.version}版本依赖是4.2.0.Final
上述如果完成了的话现在可以开始进行hibernate的详细分析了。


下面是hibernate核心配置文件


```
## 配置文件
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>

        <!-- Database connection settings -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/blog</property>
        <property name="connection.username">root</property>
        <property name="connection.password">root</property>
        <property name="javax.persistence.validation.mode">none</property>

        <property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>

        <property name="current_session_context_class">thread</property>

        <property name="show_sql">true</property>
        <property name="format_sql">true</property>

        <property name="hbm2ddl.auto">update</property>

        <mapping class="com.chen.login.model.LoginUser"/>
    </session-factory>
</hibernate-configuration>
```

创建表：


```
CREATE TABLE IF NOT EXISTS `login_user` (
  `id` varchar(40) NOT NULL COMMENT 'user主键',
  `email` varchar(255) NOT NULL UNIQUE COMMENT '邮箱',
  `name` varchar(255) DEFAULT NULL COMMENT '姓名',
  `nickname` varchar(255) DEFAULT NULL COMMENT '昵称',
  `qq` varchar(255) DEFAULT NULL COMMENT 'qq',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

mapping 中对应的实体类对象。xml的编程格式


```
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
       <class name="com.chen.login.model.LoginUser" table="login_user">
              <id name="id" column="id">
                     <generator class="uuid"/>   
              </id>
              <property name="name" column="name"/>
              <property name="email" column="email"/>
              <property name="nickname" column="nickname"/>
              <property name="qq" column="qq"/>
       </class>
</hibernate-mapping>
```

注解模式下的LoginUser:


```
package com.chen.login.model;

import javax.persistence.*;

import com.chen.common.db.model.EntityModel;
import org.hibernate.annotations.GenericGenerator;


@Entity
@Table(name="login_user")
public class LoginUser extends EntityModel<String>{
	
	/**
	 * 
	 */
	private static final long serialVersionUID = 7645817301567868145L;
	
	@Id
	@GenericGenerator(name = "idGenerator", strategy = "uuid")
	@GeneratedValue(generator = "idGenerator")
	@Column(length = 40)
	private String id;
	
	@Column(name="name")
	private String name;
	
	@Column(name="nickname")
	private String nickname;

	@Column(name = "qq")
	private String qq;
	
	@Column(name="email")
	private String email;
	
	
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getNickname() {
		return nickname;
	}
	public void setNickname(String nickname) {
		this.nickname = nickname;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}

	public String getQq() {
		return qq;
	}

	public void setQq(String qq) {
		this.qq = qq;
	}

	@Override
	public String toString() {
		StringBuilder builder = new StringBuilder();
		builder.append("LoginUser : {id=");
		builder.append(id);
		builder.append(", name=");
		builder.append(name);
		builder.append(", nickname=");
		builder.append(nickname);
		builder.append(", qq=");
		builder.append(qq);
		builder.append(", email=");
		builder.append(email);
		builder.append("}");
		return builder.toString();
	}
	

}
```

测试增删改查：

HibernateUtil类

```
/**
 * Author ： chen
 * Date ： 16/7/22
 * Time : 下午10:10
 */
import org.apache.log4j.Logger;
import org.hibernate.HibernateException;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.cfg.Configuration;


public class HibernateUtil {

    public static final ThreadLocal<Session> SESSIONMAP = new ThreadLocal<Session>();
    private static final SessionFactory sessionFactory;
    private static final Logger LOGGER = Logger.getLogger(HibernateUtil.class);

    static {
        try {
            LOGGER.debug("HibernateUti.static - loading cofig");
            sessionFactory = new Configuration().configure("hibernate.cfg.xml")
                    .buildSessionFactory();
            LOGGER.debug("HibernateUtil.static - end");
        } catch (Throwable ex) {
            ex.printStackTrace();
            LOGGER.error("HibernateUti error : ExceptionInInitializerError");
            throw new ExceptionInInitializerError(ex);
        }
    }

    private HibernateUtil() {

    }

    public static Session getSession() throws HibernateException {
        Session session = SESSIONMAP.get();

        if(session == null) {
            session = sessionFactory.openSession();
            SESSIONMAP.set(session);
        }

        return session;
    }

    public static void closeSession() throws HibernateException {
        Session session = SESSIONMAP.get();
        SESSIONMAP.set(null);

        if(session != null) {
            session.close();
        }
    }

}
```

测试类：


```
/**
 * Author ： chen
 * Date ： 16/7/22
 * Time : 下午10:11
 */

import java.util.List;
import com.chen.login.model.LoginUser;
import org.hibernate.Session;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;
import org.hibernate.tool.hbm2ddl.SchemaExport;
import org.junit.Assert;
import org.junit.Test;

public class ModelTest {



    @Test
    public void testGetSession() {
        Session session = HibernateUtil.getSession();

        Assert.assertNotNull(session);

        HibernateUtil.closeSession();
    }

    @Test
    public void testExport() {
        new SchemaExport(new Configuration().configure()).create(true, true);
    }

    @Test
    public void testSave() {
        LoginUser person = new LoginUser();
        person.setId("100");
        person.setName("路飞");

        Session session = HibernateUtil.getSession();
        Transaction tx = session.beginTransaction();

        session.save(person);

        tx.commit();
        HibernateUtil.closeSession();
    }

    @Test
    public void testQuery() {

        Session session = HibernateUtil.getSession();
        session.beginTransaction();

        @SuppressWarnings("unchecked")
        List<LoginUser> personList = session.createQuery("select p from LoginUser  p ").list();

        System.err.println(personList.size());

        for(LoginUser eachPerson : personList) {
            System.err.println(eachPerson);
        }

        session.getTransaction().commit();
        HibernateUtil.closeSession();
    }
}

```


以上简单的关于hibernate的上手已经可以进行单表的插入数据和查询数据了。下次聊聊对象的映射。

