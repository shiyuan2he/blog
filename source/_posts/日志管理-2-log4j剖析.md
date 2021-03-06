---
title: (日志管理-2)log4j剖析
date: 2017-04-26 11:31:25
categories: 日志管理之美
tags: log4j,java,日志,logback
---
    Log4J的配置文件就是用来设置记录器的级别、存放器和布局的，它可接key=value格式的设置
或xml格式的设置信息。通过配置，可以创建出Log4J的运行环境。log4j.properties ==> src同级创建并设置log4j.properties
　　log4j.rootLogger = 日志级别，appender1, appender2,...

　　　　日志级别：ALL < DEBUG < INFO < WARN < ERROR < FATAL < OFF，不区分大小写

　　　　注意，需在控制台输入，只需将其中一个appender定义为stdout即可
　　　　注意，rootLogger默认是对整个工程生效
　　　　注意，如果只想对某些包操作，
		那么：log4j.logger.hsy.utils = info, stdout，表示该日志对package hsy.util生效
　　　　注意，这样做可以区分dev/线上，也可以减小性能影响：if(log.isDebugEnabled()){log.debug();}
　　　　-----------------------

　　　　log4j.appender.appender1 = org.apache.log4j.日志输出到哪儿
　　　　　　ConsoleAppender（控制台）
　　　　　　FileAppender（文件）
　　　　　　DailyRollingFileAppender（每天产生一个日志文件）
　　　　　　RollingFileAppender（文件大小到达指定尺寸时产生一个新的文件）
　　　　　　WriteAppender（将日志信息以流格式发送到任意指定的地方）
　　　　　　JDBCAppender（将日志信息保存到数据库中）
　　　　　　log4j.appender.appender1.File=文件目录及文件
　　　　    log4j.appender.appender1.MaxFileSize=最大文件大小
　　　　    log4j.appender.appender1.MaxBackupIndex=备份文件个数

　　　　其中，appender1是在第一行定义过的；
　　　　文件目录及文件，例如，/home/admin/logs/jar/debug.log
	最大文件大小，例如，100KB
　　　　备份文件个数，例如，1

　　　　-----------------
　　　　log4j.appender.ServerDailyRollingFile.DatePattern=日志后缀格式
　　　　例如，'.'yyyy-MM-dd
　　　　　　log4j.appender.appender1.layout=org.apache.log4j.日志布局格式
　　　　　　HTMLLayout（以HTML表格形式布局）
　　　　　　SimpleLayout（包含日志信息的级别和信息字符串）
　　　　　　TTCCLayout（包含日志产生的时间，执行绪，类别等信息）
　　　　　　PatternLayout（可以灵活的指定布局格式，常用）
　　　　------------------
　　　　log4j.appender.appender1.layout.ConversionPattern=日志输出格式
　　　　打印参数: Log4J采用类似C语言中的printf函数的打印格式格式化日志信息，如下:
　　　　例如，%d{yyyy-MM-dd HH:mm:ss SSS} %t %5p %l - %m%n
　　　　　　%c 输出日志信息所属的类的全名
　　　　　　%d 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，
		比如：%d{yyy-M-dd HH:mm:ss SSS}，输出类似：2002-10-18 22:10:28,673
　　　　　　%f 输出日志信息所属的类的类名
　　　　　　%l 输出日志事件的发生位置，即输出日志信息的语句处于它所在的类的第几行
　　　　　　%m 输出代码中指定的信息，如log(message)中的message
　　　　　　%n 输出一个回车换行符，Windows平台为“rn”，Unix平台为“n”
　　　　　　%p 输出优先级，即DEBUG，INFO，WARN，ERROR，FATAL。
		如果是调用debug()输出的，则为DEBUG，依此类推
　　　　　　%r 输出自应用启动到输出该日志信息所耗费的毫秒数
　　　　　　%t 输出产生该日志事件的线程名
　　　　　　-----------------
　　　　　　log4j.appender.ServerDailyRollingFile.Append=true
　　　　　　不解释，追加往后写便是
　　　　　　总结一下：
　　　　　　　　Logger类：完成日志记录，设置日志信息级别
　　　　　　　　Appender类：决定日志去向，终端、DB、硬盘
　　　　　　　　Layout类：决定日志输出的样式，例如包含当前线程、行号、时间
　　　　　　　　------------------
　　　　　　　　在代码中使用log4j，初始化Logger:
　　　　　　　　　　1）在程序中调用BasicConfigurator.configure()方法：
			给根记录器增加一个ConsoleAppender，
			输出格式通过PatternLayout设为"%-4r [%t] %-5p %c %x - %m%n"，
			还有根记录器的默认级别是Level.DEBUG.
　　　　　　　　　　2）配置放在文件里，通过命令行参数传递文件名字，
			通过PropertyConfigurator.configure(args[x])解析并配置；
　　　　　　　　　　3）配置放在文件里，通过环境变量传递文件名等信息，
			利用log4j默认的初始化过程解析并配置；
　　　　　　　　　　4）配置放在文件里，通过应用服务器配置传递文件名等信息，
			利用一个特殊的servlet来完成配置。
	　　　　--------------------------------------------------------------------------
　　　　　　　　3. 为不同的 Appender 设置日志输出级别：
　　　　　　　　　　当调试系统时，我们往往注意的只是异常级别的日志输出，
			但是通常所有级别的输出都是放在一个文件里的，
			如果日志输出的级别是BUG！？那就慢慢去找吧。
　　　　　　　　　　这时我们也许会想要是能把异常信息单独输出到一个文件里该多好啊。
			当然可以，Log4j已经提供了这样的功能，我们只需要在配置中修改Appender的
		Threshold 就能实现,比如下面的例子：
　　　　　　　　[配置文件]
　　　　　　　　　　### set log levels ###
　　　　　　　　　　log4j.rootLogger = debug , stdout , D , E

　　　　　　　　　　### 输出到控制台 ###
　　　　　　　　　　log4j.appender.stdout = org.apache.log4j.ConsoleAppender
　　　　　　　　　　log4j.appender.stdout.Target = System.out
　　　　　　　　　　log4j.appender.stdout.layout = org.apache.log4j.PatternLayout
　　　　　　　　　　log4j.appender.stdout.layout.ConversionPattern = 
				%d{ABSOLUTE} %5p %c{ 1 }:%L - %m%n

　　　　　　　　　　### 输出到日志文件 ###
　　　　　　　　　　log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
　　　　　　　　　　log4j.appender.D.File = logs/log.log
　　　　　　　　　　log4j.appender.D.Append = true
　　　　　　　　　　log4j.appender.D.Threshold = DEBUG ## 输出DEBUG级别以上的日志
　　　　　　　　　　log4j.appender.D.layout = org.apache.log4j.PatternLayout
　　　　　　　　　　log4j.appender.D.layout.ConversionPattern = 
				%-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n

　　　　　　　　　　### 保存异常信息到单独文件 ###
　　　　　　　　　　log4j.appender.D = org.apache.log4j.DailyRollingFileAppender
　　　　　　　　　　log4j.appender.D.File = logs/error.log ## 异常日志文件名
　　　　　　　　　　log4j.appender.D.Append = true
　　　　　　　　　　log4j.appender.D.Threshold = ERROR ## 只输出ERROR级别以上的日志!!!
　　　　　　　　　　log4j.appender.D.layout = org.apache.log4j.PatternLayout
　　　　　　　　　　log4j.appender.D.layout.ConversionPattern = 
				%-d{yyyy-MM-dd HH:mm:ss} [ %t:%r ] - [ %p ] %m%n

　　　　　　　　　　[代码中使用]
　　　　　　　　　　public class TestLog4j {
　　　　　　　　　　　　public static void main(String[] args) {
　　　　　　　　　　　　　　PropertyConfigurator.configure( " D:/Code/conf/log4j.properties " );
　　　　　　　　　　　　　　Logger logger = Logger.getLogger(TestLog4j. class );
	　　　　　　　　　　logger.debug( " debug " );
　　　　　　　　　　　　　　logger.error( " error " );
　　　　　　　　　　　　}
　　　　　　　　　　}
　　　　　　-----------------------------------------------------------------------------------
　　　　　　public class Test {
　　　　　　　　private static Logger logger = Logger.getLogger(Test.class);
　　　　　　　　public static void main(String[] args) {
　　　　　　　　　　// System.out.println("This is println message.");

　　　　　　　　　　// 记录debug级别的信息
　　　　　　　　　　logger.debug("This is debug message.");
　　　　　　　　　　// 记录info级别的信息
　　　　　　　　　　ogger.info("This is info message.");
　　　　　　　　　　// 记录error级别的信息
　　　　　　　　　　logger.error("This is error message.");
　　　　　　　　}
　　　　　　}
　　　　-----------------------------------------------------------------------------
　　　　最后粘上本人用的测试log4j的配置文件内容，仅供参考。希望对您有所帮助
　　　　　　log4j.rootLogger=INFO, stdout, file

　　　　　　log4j.appender.stdout=org.apache.log4j.ConsoleAppender
　　　　　　log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
　　　　　　log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss SSS} %t %5p %l - %m%n

　　　　　　log4j.appender.file=org.apache.log4j.DailyRollingFileAppender
　　　　　　log4j.appender.file.Threshold=debug
　　　　　　log4j.appender.file.layout=org.apache.log4j.PatternLayout
　　　　　　log4j.appender.file.layout.ConversionPattern= %d{yyyy-MM-dd HH:mm:ss SSS} %t %5p %l - %m%n
　　　　　　log4j.appender.file.File=D:/others/logs/jar/debug.log
　　　　　　#log4j.appender.file.File=/home/test_zzs_dzfp/logs/zzssl/zzssl.log
　　　　　　log4j.appender.file.MaxFileSize=200MB
　　　　　　log4j.appender.file.MaxBackupIndex=20

　　　　　　#project defalult level
　　　　　　#log4j.logger.hsy.utils=INFO

