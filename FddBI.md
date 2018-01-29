# [Fdd BI系统](http://bi.fangdd.cn/systemmanager/fine-report.jsp)

## 第一部分 WEB展示
- 前端查询：
    1. 系统权限：
        - 定义角色: 系统管理 -> 角色管理
        - 后台配置用户的系统角色: 系统管理 -> 用户管理
    2. 内容权限：
        - 后台配置用户查询报表数据的权限: 系统管理 -> 内容权限管理
- 报表管理：
    1. 配置报表节点 右击 -> 添加节点
    2. 关联相关Finereport服务端的cpt文件 右击-> 关联报表
- 报表制作：
    1. 详情见Finereport制作文档
- 上线流程
    1. 各自将cpt统一提交到ITG环境
    2. 管理员从ITG环境预览测试通过后保存到PRO环境
    3. 如果是新增报表前端需要 **新增节点** -> **关联CPT**，如果是更新则前端自动更新。

## 第二部分 邮件
- 发送机制：预发布和正式发布
    1. 预发布时间7:30，正式发布时间为9:30.
    2. 预发布的收件人是esfbigdata@fangdd.com，目的是每天早上能及时发现数据问题，为正式发送提供时间缓冲。
- 编辑功能
    1. 系统管理 -> 邮件调度
    2. 通过下拉框过滤查询邮件调度
    3. 功能有：
        - 新增: 新增邮件调度，每次新增一个邮件需要添加两次调度 **预发** 和 **正式**
        - 编辑: 修改邮件调度
        - 删除: 当一个调度不需要时可以删除
        - 停用: 可以临时停用指定的调度
        - 启用: 启用临时停用的调度
        - 发送: 发送指定的邮件
    4. 参数
        - 发送类型：正常，预发
        - 状态：正常，停止
        - finereport任务名称
        - 报表类型
        - 报表名称
        - cpt文件名
        - 附件名称
        - 收件人
        - 抄送人
        - 自动发送时间
        - 正文显示图片
        - 附件
        - 正文文本内容
- 邮件分类
    1. 日报
        - 每天发送，线上的报表类型为周报，发送时间为周一至周日
        - 对应finereport的目录bireport/Mail/daily
    2. 周报
        - 每周一发送，线上的报表类型为周报
        - 对应finereport的目录bireport/Mail/weekly
    3. 月报:
        - 每月1号发送，线上的报表类型为月报
        - 对应finereport的目录bireport/Mail/monthly
- 邮件制作
    - 详情见Finereport制作文档
- 后台
    1. 基本原理：
        - 通过访问finereport后台服务的指定报表的url，获取图片和EXCEL。
        - 通过worklow调度定时执行java程序。
    2. mysql 记录邮件调度状态和基本信息
        - host:10.50.23.208:3306/fangdd_esf_data
        - table:finereport_task_mapping
    3. Java代码地址
        - url: git@gitlab.esf.fangdd.net:esf_datawarehouse/bi-email.git
    4. workflow调度
        - edw_bi_email_pre/edw_bi_email
    


## 第三部分 钉钉BI
- 发送机制：
    - 与邮件类似，分预发布和正式发布。
    - 预发布钉钉只通知组内，对报表数据做预判。
- TAB页面
    - 新房C端(nh)
    - 二手房平台(esf)
    - 房市头条(news)
    - 新房平台(nhp)
- 访问权限
    - mysql: 
        - host: ro.esfdb.fdd:3307/fdd_data_job/
        - table: t_bi_ac
        - user: esf_report_dev
        - pwd: pwd_esf_report
    - 根据字段ac判断用户对TAB页的访问权限
        - {"nh":1,"esf":1,"nhp":1,"news":1}
    - 根据
- 后台
    1. 基本原理：
        - Python执行脚本生成HTML页面，向指定用户的钉钉发送URL链接
        - crontab定时调度执行Python
    2. 服务器
        - host: s1-hades.esf.fdd
        - dir: /data0/php/python/blacklist-job/job-web/
    3. Python代码
        - url: git@gitlab.esf.fangdd.net:esf_datawarehouse/bi-jobs.git