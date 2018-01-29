# Finereport制作

## Finereport特点
- FineReport采用Excel形式的设计界面
- [官方帮助文档8.0](http://help.finereport.com/finereport8.0/)
## 远程服务器
- Pro
    1. host: 10.50.23.213:8089
    2. Project Path: /data0/apache-tomcat-8.0.30/custom_webs/bi_web/webapps/ROOT/WEB-INF
- ITG
    1. host:10.50.23.204:8090
    2. Project Path: /usr/local/tomcat8/custom_webs/bi_web_pre_online/webapps/fangdd-bi
- 工作目录配置</p>
    打开设计器，文件 -> 切换工作目录，选择其他 -> 点击“+”添加远程服务器
    1. 主机名和端口: 见上面
    2. WEB应用: ITG为fangdd-bi,Pro置空
    3. Servlet: 统一为ReportSever
    4. 用户名: demo
    5. 密码: demo
## 数据连接
Finereport可以通过JDBC、JNDI、SAP、XMLA和FineIndex五种方式连接数据库，当报表执行时需要访问数据库时这些连接才会被激活。</p>
所有信息都保存在%FR_HOME%/WebReport/WEB-INF/resources/datasource.xml配置文件中。
- 已有
    1. vertica
    2. presto
    3. mysql
- 新增
    1. 登录服务端
    2. 在%FR_HOME%/WEB-INF/lib目录下添加JDBC的JAR包
    3. 切换到root用户，重启服务
    ```
    sudo su -
    cd /usr/local/tomcat8/custom_webs/bi_web_pre_online # 项目所在的目录
    sh bin/shutdown.sh
    sh bin/startup.sh
    ```
    4. 如出现中文论码问题，检查$JAVA_HOME/jre/lib/fonts是否含有对应字体。添加字体需要重启服务。
- 配置方法
    1. [JDBC](http://help.finereport.com/finereport8.0/doc-view-101.html)
## 远程设计
1. 远程设计可以直接修改服务器上面的报表，且保存的模板都是直接上传到服务器上面的。
2. 多人同时制作一张报表时，对应的cpt会显示锁定。
## 制作规范
- 邮件
    1. cpt参数只有一个，$dt，不支持多个参数和其他参数。
    2. cpt文件命名最好使用英文，防止服务端出现乱码问题。
- WEB报表
    1. 参数要按照顺序排版：二手房（城市，区域，板块，小区）、~~~新房（商户类型，商户，军区，战区，运营经理）~~~
## 遇到的问题
1. 扩展的时候，上下相同的值只显示一个 -> 单元格设置从group改为list
2. 多个数据集联合扩展的时候出现夸行 -> 手动指定父单元格为维度所在的单元格
3. 联合下拉框参数 -> 不同的参数指定不同的数据集，通过数据集参数来定义关联关系
5. 自定义javascript -> 可以在参数控件中
