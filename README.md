<p align="center">
    张天合 个人简历 C/GO 、系统及数据库开发
</p>

张天合 <kbd>[点击此处跳转网页版](https://github.com/zhtianhe/resume/tree/master)</kbd>
***
##  联系方式
---
*  手 机：17600xxxx72
*  Email：ztianhe@dingtalk.com
*  QQ/微信号：手机同号

## 个人信息
---
  * 张天合/男/19901210/河南省南阳市
  * 专科/郑州电力学院/机电工程系/机电一体化
  * 工作年限：6年（2014年7月毕业）
  * 期望职位：C/GO、系统及数据库开发
  * 期望薪资：
  * 期望城市：


## 工作经历
---
### 北京新宇合创（ 2016年9月 ~ 今 ）
* 职位： 软件开发
* 坐标： 邮政储蓄银行人行网上跨行支付清算系统项目(简称:邮储超网)

#### PG-HA 集群
* 时间：2017年08月-2018年1月25上线-同年6月稳定
   耗时3个月测试环境部署+2个月准生产部署测试及各种异常验证+4个月生产环境实各种战演练及向行方人员解惑答疑。
* 功能描述：
  超级网银亦庄中心云部署的核心应用部分采用32个主节点和32个备节点，每个主节点对应一个备节点，分别部署32个主数据库（master）和32个备数据库（slave）。主库和备库之间采用流复制技术实现实时备份。每个节点除了部署主库或备库外，还要部署核心应用系统和内联前置系统。
  亦庄中心云部署对核心数据采用hash分区进行数据库划分，划分为32个分库，后续合肥中心上线后，将会再增加32个分库。按照当前交易量，亦庄中心每台数据库中每日实际存储的各类流水数据小于1G，单个数据库全备份（采用pg_dump）小于1G。对于行内系统发送的往账采取前端流水号进行hash分区，对于人行发来的来账采取报文标识号进行hash分区，对于行内往账产生的报文标识号、通讯级标识号等则按各个分库进行分段设置。
  采用corosync+pacemaker实现主/备库之间的自动切换：
  * 主节点失效切换
  master宕掉时，RA检测到该问题并将master标记为stop，随后将slave提升为新的master；
  * 初始启动时自动识别新旧数据
  当两个或多个节点上的Pacemaker同时初始启动时，RA通过每个节点上最近的replay location进行比较，找出最新数据节点。这个拥有最新数据的节点将被认为是master。若在一个节点上启动pacemaker或者该节点上 的pacemaker是第一个被启动的，那么它也将成为master。RA依据停止前的数据状态进行裁定。
  * 读负载均衡
  由于slave节点可以处理只读事务，因此对于读操作可以通过虚拟另一个虚拟IP来实现读操作的负载均衡。

* 项目收获：
  熟练掌握postgres 流复制、主/备切换规则、Wall日志、数据库备份与恢复、全量/增量恢复。corosync 心跳检测及仲裁规则、pacemaker资源管理功能、crm/pcs 工具的应用。通过文件共享锁避免双节点脑裂，为邮储节约了硬件资源和管理运维成本。感谢项目组的同事及公司的专家团队支持与帮助。

#### 海南自贸数据报送
* 功能描述：
    根据海南自贸系统要求将超网记账性交易流水从多表中抽离处理通过cpds平台以 JSON 数据格式实时报送。利用go、cgo语音编写、使用grom包操作数据库，将原C架构中的加密解密抽离出用 go 重写，将 cpds 的 C接口用CGO进行重写实现go 调用 cpds 动态库。利用go 携程特性实现业务处理高并发，独立运行无依赖原有架构。快速灵活应对关联体统各种数据格式、及编码格式要求。

* 项目收获：
    掌握了 go和C库 互相调用的技巧利用新语言的生态及特性体高了开发速度。
    掌握了 C 平台数据库密钥口令的规律及爆破技巧。
    掌握了 grom 包的使用技巧、连接池、无SQL化事务处理等
    体验到了行内各个系统间没有统一的编码规范JSON格式都用gbk感觉无奈。
    感谢直属领导对新技术的推广和支持。

#### 自动化运维工具 pshg
* 项目描述：
    同事测试换版，在多台机器上反复操作，看着都痛苦，自己操作起来也很苦恼。我为什么不搞个什么工具解决这种现状呢？网上看了一边，要么配置繁琐要么需要互信。觉得不适合，就仿照psh工具的功能开发了pshg工具。
    该工具以 ssh、scp 命令为核心使用bash、expect、openssl 编写了运维测试批量管理工具。有一下特点：
    * 解决无互信文件时，获取密钥自动补充口令执行指令。
    * 单用户 远程执行命令、文件上传下载、远程登录、
    * 简单分组并发处理：批量换版、批量启停服务、量执行命令
    * 安全性：配置文件加密处理。（看懂脚本能读完的例外）

* 项目收获：
    将自己定位于提高项目组整体工作效率的润滑剂，注意观察项目组开发测试需求。在提高bash脚本的技能同时又解决了同事换版痛点，也赢得了同事和领导的赞许。


#### 参与超网其他项目及简要描述
* 核心程序迁移：pro*c 移植 ecpg
* 程序优化架构修改：公共与分库标数即时同步、串行改并发处理
* 接口程序优化：接口资源使用高、工行335 CA 验签异常等
* 超网接大数据平台：抽离数据、格式转换、编码处理

  ##### 人行相关类项目
  * 人行网银二代改造: 参与部分开发报文1.5 升级 1.6.0  兼容性版本控制、程序模块优化
  * 人行手机号码支付：参与部分开发 1.6.3 报文(目前:1.6.4)
  * 人行验签升级改造：配合人测试验证 RSA 切换 国密SM2
  * 人行前置程序移植：独立完成 AIX程序 迁移 Suse Linux 程序改造
  * 人行前置程序升级：独立完成 多进程、多线程优化处理支持多端口发送
  * 人行前置准生产部署:独立完成 pmts、IBM MQ 安装部署、配合人行现场验
  * 人行手续费功能：参与部分开发 日终、月终数据抽取及汇总统计部分
  * MBFE 双活改造：参与部分测试与反馈、利用go+MQ+缓存数据库开发

  ##### 系统环境搭建类项目
  * 亦庄准生产部署：主要负责PG-HA 异常演练测试、参数优化、参与压测及性能测试
  * 合肥准生产部署：参与数据库处理 、系统压测、异常测试接口应用及数据库
  * 超网开发环境搭建：开发编译、报文ide工具、git 安装
  * 测试环境部署维护：人行前置、外联接口、外联代理、核心应用、内联接口
  * 工程文档编写：2017工程、2018工程、 需求分析、详细设计等文档编写
  * 超网接数据管控平台：每周五是实时生成表结构文件报送
  ##### 数据解析及字符编码处理类项目
  * 交易数据移植：参与部分开发 Oracle 转 postgresql
  * 户名掩码功能：独立完成统一按照国标GB18030-2005 标准处理。原gb2312、gbk 无法解决生僻字乱码乱码问题。
  * 接入短信平台：独立完成 数据报解析组包及编码处理
  * 超网接大数据平台：独立完成 抽离数据、格式转换、编码处理
  * linux vim、git 等工具中文乱码处理
  *
  ##### 独立开发测试运维工具类项目
  * gogo工具开发： 参照oracle gogo 改为 pg gogo
  *  PgsqlCopy开发:实现远程数据导出本地文件、本地文件导入远程数据库，解决双中心，应用、文件、数据库不在通一台虚拟机上无法使用嵌入式SQL COPY 命令。
  * 短信处理工具开发：测试环境，短信报文解析、组包、手工发送。
  * 数据消息报送cpds工具开发：测试环境，人工验证测试使用。
  * 数据文件报送EFTP工具开发：数据报送异常时、人工补发数据文件
  * 双中心数据迁移工具开发:shell + FpPgsqlCopy 实现（支持密文、明文口令）后期选用开源项目
  * 双中心标数同步工具开发:正在进行中...

## 开源项目和作品
 * 向postgresql开源社区反馈 9.6.2 版本 1 处BUG问题，已在 9.6.8 版本中得到修复。

## 技术文章
---
 * [tuxedo fml编程](https://github.com/zhtianhe/Notes/blob/master/TUXEDO/Tuxedo-Fml/Tuxedo_Fml32%E6%95%99%E7%A8%8B.c.md)

## 演讲和讲义
---
- 2018年3月 超网运维人员 PG-HA 培训活动。
- 2018年6月 国家会议中心参加 阿里公益PG培训交流讨论活动
- 2018年8月 72号院向其他厂商及行方运维领导宣讲PG-HA 切换演练、答疑解惑活动。
- 2018年12月 邀请阿里云德哥在永丰基地会议室进行postgresql数据库交流活动。

## 技能清单
---
* 开发语言：Linux C 、Makefile、GO(学习中) 、CGO 、
* 嵌入式sql: Pro*C、ECPG
* 脚本语言：bash、
* 编译调试：gcc、gdb
* 开发工具：vi/vim、Source Insight(自定义编写宏)、Atom(Markdown)、goland(go ide)、UE
* 中间件：Tuxedo 、WebSphere MQ
* 数据库：postgrsql、oracle、mysql、sqllite
* 环境搭建：podtgresql 高可用集群（corosync+crm|pcs+pacmaker）、gitlab 等
* 开发平台 ：Linux(Asianux3.4-7.3、CentOS6.4-7.8、SUSE11-12、Ubuntu)、了解AIX
* 版本管理 ：git 、gitlab搭建 、svn、接触过CVS
* 数据处理：XML、JSON、Tuxedo FML
* 自动化部署工具：Ansible、pshg
* 系统编程：TCP/IP协议、套接字、进程间通信、多任务处理(子进程/线程）
* 系统编程：进程间通信、多进程、多线程
* 网络编程：socket、原始套接字、TCP/IP协议
* 了解图形界面：GTK
* 了解浏览器 + Web（C CGI）+  mysql数据库的开发、https 协议
* 了解HTML、JS、CSS、AJAX、XML、JSON 等开发技术。
* 了解开发板 Bootloader 启动过程、内核剪裁与移植、根文件系统制作。

---

### 致谢
感谢您花时间阅读我的简历。

<p align="center">
    张天合 个人简历 C/GO 、系统及数据库开发
</p>
