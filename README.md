# Versions

```xml
<cas.version>5.2.x</cas.version>
```

# Requirements

* JDK 1.8+

v1版-实现功能
============================
①HTTPS配置
============================
②添加cas-server-support-rest-authentication
===========================================

cas-server-support-rest-authentication模块中配置cas.authn.rest.uri可以将用户验证转移到另一模块

③添加cas-server-support-rest
===========================================

cas-server-support-rest模块提供APP接入获取ST的一系列接口

④修复用户名中文乱码的bug
===========================================

重写cas-server-support-rest-authentication模块添加UTF-8编码  
WEB-INF下的lib保存在网盘
链接：https://pan.baidu.com/s/1ZYT3wUKMDeKVd4TuDt0ksQ  
提取码：wl00  


# HTTPS配置

①生成证书  
keytool -genkey -alias cas -keyalg RSA -keysize 1024 -keypass 123456 -validity 365 -keystore d:\cas.keystore -storepass 123456  
-alias后面的别名可以自定义  
-keypass指定证书密钥库的密码  
-storepass和前面keypass密码相同  
-keystore指定证书的位置  
注：提示输入的“您的名字与姓氏是什么” 必须是域名 其他随意  
②导出证书  
keytool -export -alias cas -keystore d:\cas.keystore -file d:\cas.crt -storepass 123456  
③导入证书到jvm  
keytool -import -keystore %JAVA_HOME%\jre\lib\security\cacerts -file d:\cas.crt -alias cas

# Tomcat配置
①下载地址：https://tomcat.apache.org/download-80.cgi 推荐下载7或8版本  
②配置HTTPS  
找到conf\server.xml添加 
``` 
        <Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"  
            maxThreads="150" SSLEnabled="true" scheme="https" secure="true"  
            keystoreFile="d:/cas.keystore" keystorePass="123456"  
            clientAuth="false" sslProtocol="TLS" />  
```
              
keystoreFile即为生成的keystore路径 keystorePass即为对应的密码  
③Tomcat日志中文乱码解决  
conf\logging.properties内  
将java.util.logging.ConsoleHandler.encoding = UTF-8改为  
java.util.logging.ConsoleHandler.encoding = GBK  
### Tomcat启动  
win10下点击 startup.bat  
linux下执行 sh startup.sh  

注:inux环境启动 如果报The file is absent or does not have execute permission错误 则执行如下命令  
chmod  777 文件名
