# wnameless/oracle-xe-11g

运行 Oracle XE 容器：

	$ docker run -d -p 9090:8080 -p 1521:1521 wnameless/oracle-xe-11g

将容器中的 Oracle XE 管理界面的 8080 端口映射为本机的 9090 端口，将 Oracle XE 的 1521 端口映射为本机的 1521 端口。

本容器提供如下安装信息：

	hostname: localhost
	端口: 1521
	SID: XE
	username: system/sys
	password: oracle

管理界面访问：

	url: http://localhost:9090/apex
	workspace: internal
	username: admin
	password: oracle
	
首次访问 Oracle XE 管理界面时，要求修改密码。例如，修改为 oraclexe。