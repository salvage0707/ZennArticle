---
title: "Springのバリデーションの実装時の導入でのエラー" 
emoji: "🇰🇼"
type: "tech" 
topics: ["Java","spring","Validation"]
published: true
---
# springでのバリデーション実装エラー

## 環境

```xml:pom.xml
<properties>
	<!-- Generic properties -->
	<java.version>1.7</java.version>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>

	<!-- Web -->
	<jsp.version>2.2</jsp.version>
	<jstl.version>1.2</jstl.version>
	<servlet.version>3.1.0</servlet.version>


	<!-- Spring -->
	<spring-framework.version>3.2.3.RELEASE</spring-framework.version>

	<!-- Hibernate / JPA -->
	<hibernate.version>4.2.1.Final</hibernate.version>

	<!-- Logging -->
	<logback.version>1.0.13</logback.version>
	<slf4j.version>1.7.5</slf4j.version>

	<!-- Test -->
	<junit.version>4.11</junit.version>
</properties>
```

##実装時でエラーが出るまでの手順
pom.xmlに2つのjarファイルを追加

```xml:pom.xml
<!-- validation-api(入力値検証で使用) -->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.1.0.Final</version>
</dependency>
<!-- Hibernate(入力値検証で使用) -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.0.1.Final</version>
</dependency>
```

## 2つのエラーが出現

1, サーブレットがinit()エラー

```
HTTPステータス 500 - サーブレット dispatcherServlet のServlet.init()が例外を投げました
type 例外レポート
------------------------------------------------------------------------------------------

メッセージ サーブレット dispatcherServlet のServlet.init()が例外を投げました

説明 The server encountered an internal error that prevented it from fulfilling this request.

例外-------------------------------------------------------------------------------------------

javax.servlet.ServletException: サーブレット dispatcherServlet のServlet.init()が例外を投げました
	org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:505)
	org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:103)
	org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:956)
	org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:436)
	org.apache.coyote.http11.AbstractHttp11Processor.process(AbstractHttp11Processor.java:1078)
	org.apache.coyote.AbstractProtocol$AbstractConnectionHandler.process(AbstractProtocol.java:625)
	org.apache.tomcat.util.net.JIoEndpoint$SocketProcessor.run(JIoEndpoint.java:316)
	java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
	java.lang.Thread.run(Thread.java:745)

<!-- 以下省略 -->
```

2, @NotEmptyが使用できない

## エラーの解消

原因は追加したjarファイルのバージョンの関係
pom.xmlを変更し、再度プロジェクトの更新を行ったら２つとも解消

```xml:pom.xml
<!-- validation-api(入力値検証で使用) -->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
<!-- Hibernate(入力値検証で使用) -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.3.4.Final</version>
</dependency>
``` 

## !!!エラーは消えたが、使用できない!!!

今回はバージョンの関係ということはわかったが、根本的な解決には至らなかったので、別の機会に再検討

