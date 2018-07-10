---
title: 框架篇-Spring+Mybatis整合Druid连接池，并配置SQL监控
date: 2017-10-30 15:13:30
categories: "开发"
tags:
	- Java
	- 技术
	- GitHub
	- MySQL
	- SQL

---

1.Druid简介

Druid是Java语言中最好的数据库连接池。Druid能够提供强大的监控和扩展功能。Java程序很大一部分要操作数据库，为了提高性能操作数据库的时候，有不得不使用数据库连接池。数据库连接池有很多选择，c3p0、dhcp、proxool等，druid作为一名后起之秀，凭借其出色的性能，也逐渐印入了大家的眼帘。企业中接触到的项目几乎都是在使用druid作为数据源。

Druid 提供了很多配置参数：

<table>
 <tbody>
  <tr>
   <td>配置</td>
   <td>缺省值</td>
   <td>说明</td>
  </tr>
  <tr>
   <td>name</td>
   <td><br></td>
   <td>配置这个属性的意义在于，如果存在多个数据源，<p>监控的时候可以通过名字来区分开来。 </p><p>如果没有配置，将会生成一个名字， 格式是：”DataSource-” + System.identityHashCode(this)</p></td>
  </tr>
  <tr>
   <td>jdbcUrl</td>
   <td><br></td>
   <td>连接数据库的url，不同数据库不一样。例如： <p>mysql : jdbc:mysql://10.20.153.104:3306/druid2 </p><p>oracle : jdbc:oracle:thin:@10.20.149.85:1521:ocnauto</p></td>
  </tr>
  <tr>
   <td>username</td>
   <td><br></td>
   <td>连接数据库的用户名</td>
  </tr>
  <tr>
   <td>password</td>
   <td><br></td>
   <td>连接数据库的密码。如果你不希望密码直接写在配置文件中，<p>可以使用ConfigFilter。详细看这里：https://github.com/alibaba/druid/wiki/%E4%BD%BF%E7%94%A8ConfigFilter</p></td>
  </tr>
  <tr>
   <td>driverClassName</td>
   <td>根据url自动识别</td>
   <td>这一项可配可不配，如果不配置druid会根据url自动识别dbType，<p>然后选择相应的driverClassName(建议配置下)</p></td>
  </tr>
  <tr>
   <td>initialSize</td>
   <td>0</td>
   <td>初始化时建立物理连接的个数。<p>初始化发生在显示调用init方法，或者第一次getConnection时</p></td>
  </tr>
  <tr>
   <td>maxActive</td>
   <td>8</td>
   <td>最大连接池数量</td>
  </tr>
  <tr>
   <td>maxIdle</td>
   <td>8</td>
   <td>已经不再使用，配置了也没效果</td>
  </tr>
  <tr>
   <td>minIdle</td>
   <td><br></td>
   <td>最小连接池数量</td>
  </tr>
  <tr>
   <td>maxWait</td>
   <td><br></td>
   <td>获取连接时最大等待时间，单位毫秒。<p>配置了maxWait之后，缺省启用公平锁，并发效率会有所下降，</p><p>如果需要可以通过配置useUnfairLock属性为true使用非公平锁。</p></td>
  </tr>
  <tr>
   <td>poolPreparedStatements</td>
   <td>false</td>
   <td>是否缓存preparedStatement，也就是PSCache。<p>PSCache对支持游标的数据库性能提升巨大，</p><p>比如说oracle。在mysql下建议关闭。</p></td>
  </tr>
  <tr>
   <td>maxOpenPreparedStatements</td>
   <td>-1</td>
   <td>要启用PSCache，必须配置大于0，当大于0时，<p>poolPreparedStatements自动触发修改为true。</p><p>在Druid中，不会存在Oracle下PSCache占用内存过多的问题，</p><p>可以把这个数值配置大一些，比如说100</p></td>
  </tr>
  <tr>
   <td>validationQuery</td>
   <td><br></td>
   <td>用来检测连接是否有效的sql，要求是一个查询语句。<p>如果validationQuery为null，testOnBorrow、testOnReturn、</p><p>testWhileIdle都不会其作用。</p></td>
  </tr>
  <tr>
   <td>testOnBorrow</td>
   <td>true</td>
   <td>申请连接时执行validationQuery检测连接是否有效，<p>做了这个配置会降低性能。</p></td>
  </tr>
  <tr>
   <td>testOnReturn</td>
   <td>false</td>
   <td>归还连接时执行validationQuery检测连接是否有效，<p>做了这个配置会降低性能</p></td>
  </tr>
  <tr>
   <td>testWhileIdle</td>
   <td>false</td>
   <td>建议配置为true，不影响性能，并且保证安全性。申请连接的时候<p>检测，如果空闲时间大于timeBetweenEvictionRunsMillis，</p><p>执行validationQuery检测连接是否有效。</p></td>
  </tr>
  <tr>
   <td>timeBetweenEvictionRunsMillis</td>
   <td><br></td>
   <td>有两个含义： <p>1) Destroy线程会检测连接的间隔时间2) </p><p>testWhileIdle的判断依据，详细看testWhileIdle属性的说明</p></td>
  </tr>
  <tr>
   <td>numTestsPerEvictionRun</td>
   <td><br></td>
   <td>不再使用，一个DruidDataSource只支持一个EvictionRun</td>
  </tr>
  <tr>
   <td>minEvictableIdleTimeMillis</td>
   <td><br></td>
   <td><br></td>
  </tr>
  <tr>
   <td>connectionInitSqls</td>
   <td><br></td>
   <td>物理连接初始化的时候执行的sql</td>
  </tr>
  <tr>
   <td>exceptionSorter</td>
   <td>根据dbType自动识别</td>
   <td>当数据库抛出一些不可恢复的异常时，抛弃连接</td>
  </tr>
  <tr>
   <td>filters</td>
   <td><br></td>
   <td>属性类型是字符串，通过别名的方式配置扩展插件，<p>常用的插件有： </p><p>监控统计用的filter:stat日志用的filter:log4j</p><p>防御sql注入的filter:wall</p></td>
  </tr>
  <tr>
   <td>proxyFilters</td>
   <td><br></td>
   <td><p>类型是List&amp;lt;com.alibaba.druid.filter.Filter&amp;gt;，</p><p>如果同时配置了filters和proxyFilters，是组合关系，并非替换关系</p></td>
  </tr>
 </tbody>
</table>

2.与Druid的整合（SpringMVC+Mybatis框架的搭建可以查看之前的文章，内有github地址）

2.1 Maven pom.xml加入Druid相关jar包

> <!--druid连接池-->
> 
> <druid.version>1.0.27</druid.version>
> 
> <!-- 连接池 -->
> 
> <dependency>
> 
> <groupId>com.alibaba</groupId>
> 
> <artifactId>druid</artifactId>
> 
> <version>$\{druid.version\}</version>
> 
> </dependency>

2.2 修改原有的数据源配置文件jdbc.properties

> \#\#\#\#\#DataSource Global Setting\#\#\#\#
> 
> druid.driverClassName=com.mysql.jdbc.Driver
> 
> \#\#\\u6D4B\\u8BD5\\u73AF\\u5883\#\#
> 
> druid.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8
> 
> druid.username=root
> 
> druid.password=root
> 
> druid.initialSize = 10
> 
> druid.minIdle=6
> 
> druid.maxActive=50
> 
> druid.maxWait=60000
> 
> druid.timeBetweenEvictionRunsMillis=60000
> 
> druid.minEvictableIdleTimeMillis=300000
> 
> druid.validationQuery = SELECT 1
> 
> druid.testWhileIdle=true
> 
> druid.testOnBorrow=false
> 
> druid.testOnReturn=false
> 
> druid.poolPreparedStatements=false
> 
> druid.maxPoolPreparedStatementPerConnectionSize = 100
> 
> druid.filters=wall,stat

2.3 修改applicationContext.xml中数据源配置（注释原有配置）

> <!-- 数据源 -->
> 
> <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
> 
> init-method="init" destroy-method="close">
> 
> <!-- 数据库基本信息配置 -->
> 
> <property name="url" value="$\{druid.url\}" />
> 
> <property name="username" value="$\{druid.username\}" />
> 
> <property name="password" value="$\{druid.password\}" />
> 
> <property name = "driverClassName" value = "$\{druid.driverClassName\}" />
> 
> <!-- 初始化连接数量 -->
> 
> <property name="initialSize" value="$\{druid.initialSize\}" />
> 
> <!-- 最小空闲连接数 -->
> 
> <property name="minIdle" value="$\{druid.minIdle\}" />
> 
> <!-- 最大并发连接数 -->
> 
> <property name="maxActive" value="$\{druid.maxActive\}" />
> 
> <!-- 配置获取连接等待超时的时间 -->
> 
> <property name="maxWait" value="$\{druid.maxWait\}" />
> 
> <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
> 
> <property name="timeBetweenEvictionRunsMillis" value="$\{druid.timeBetweenEvictionRunsMillis\}" />
> 
> <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
> 
> <property name="minEvictableIdleTimeMillis" value="$\{druid.minEvictableIdleTimeMillis\}" />
> 
> <property name="validationQuery" value="$\{druid.validationQuery\}" />
> 
> <property name="testWhileIdle" value="$\{druid.testWhileIdle\}" />
> 
> <property name="testOnBorrow" value="$\{druid.testOnBorrow\}" />
> 
> <property name="testOnReturn" value="$\{druid.testOnReturn\}" />
> 
> <!-- 打开PSCache，并且指定每个连接上PSCache的大小 如果用Oracle，则把poolPreparedStatements配置为true，mysql可以配置为false。 -->
> 
> <property name="poolPreparedStatements" value="$\{druid.poolPreparedStatements\}" />
> 
> <property name="maxPoolPreparedStatementPerConnectionSize"
> 
> value="$\{druid.maxPoolPreparedStatementPerConnectionSize\}" />
> 
> <!-- 配置监控统计拦截的filters -->
> 
> <property name="filters" value="$\{druid.filters\}" />
> 
> </bean>

2.4 配置druid监控，修改web.xml(Druid 提供了StatViewServlet，用于查看对数据库的监控，为了测试监控数据需要通过web访问数据库，所以接下来需要集成SpringMVC。)

> <servlet>
> 
> <servlet-name>DruidStatView</servlet-name>
> 
> <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
> 
> </servlet>
> 
> <servlet-mapping>
> 
> <servlet-name>DruidStatView</servlet-name>
> 
> <url-pattern>/druid/\*</url-pattern>
> 
> </servlet-mapping>
> 
> <filter>
> 
> <filter-name>druidWebStatFilter</filter-name>
> 
> <filter-class>com.alibaba.druid.support.http.WebStatFilter</filter-class>
> 
> <init-param>
> 
> <param-name>exclusions</param-name>
> 
> <param-value>/public/\*,\*.js,\*.css,/druid\*,\*.jsp,\*.swf</param-value>
> 
> </init-param>
> 
> <init-param>
> 
> <param-name>principalSessionName</param-name>
> 
> <param-value>sessionInfo</param-value>
> 
> </init-param>
> 
> <init-param>
> 
> <param-name>profileEnable</param-name>
> 
> <param-value>true</param-value>
> 
> </init-param>
> 
> </filter>
> 
> <filter-mapping>
> 
> <filter-name>druidWebStatFilter</filter-name>
> 
> <url-pattern>/\*</url-pattern>
> 
> </filter-mapping>

2.5 测试是否整合成功

执行junit测试，看能否操作数据库

![框架篇-Spring+Mybatis整合Druid连接池，并配置SQL监控][-Spring_Mybatis_Druid_SQL]

打开druid 监控页面（http://127.0.0.1:8081/springmvc/druid/index.html）

![框架篇-Spring+Mybatis整合Druid连接池，并配置SQL监控][-Spring_Mybatis_Druid_SQL 1]

3.源码下载github地址 https://github.com/ty1972873004/springmvc-mybatis-druid.git


[-Spring_Mybatis_Druid_SQL]: /pro/os/crawler/V22M-AQ6F-UIQ2.jpg
[-Spring_Mybatis_Druid_SQL 1]: /pro/os/crawler/AMMN-63YF-7ZIE.jpg
 *  **原文作者：** Trazen
 *  **原文链接：** https://www.toutiao.com/item/6482598624659243534/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。