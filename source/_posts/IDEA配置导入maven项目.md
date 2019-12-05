---
title: IDEA配置导入maven项目
categories: IDEA
tags: IDEA
---

### 1、安装配置本地maven
配置文件路径：
E:\apache-maven-3.3.9\conf\settings.xml

需要修改以下两个配置：

	<localRepository>E:/m2/repository</localRepository>
	<mirrors>
	    <!-- mirror
	     | Specifies a repository mirror site to use instead of a given repository. The repository that
	     | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
	     | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
	     |
	    <mirror>
	      <id>mirrorId</id>
	      <mirrorOf>repositoryId</mirrorOf>
	      <name>Human Readable Name for this Mirror.</name>
	      <url>http://my.repository.com/repo/path</url>
	    </mirror>
	     -->
		<mirror>
			<id>repo-aliyun</id>
			<name>aliyun maven</name>
			<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
			<mirrorOf>central</mirrorOf>
		</mirror>
	</mirrors>


### 2、项目检出配置

从svn检出项目到工作空间，检出时不创建maven项目，检出之后导入maven项目，选中pom文件导入

![avatar](/images/idea/idea_maven2.png)

![avatar](/images/idea/idea_maven3.png)

IDEA中配置本地maven路径

![avatar](/images/idea/idea_maven1.png)


### 3、热部署

打开settings 配置热部署2

![avatar](/images/idea/idea_deploy1.png)

![avatar](/images/idea/idea_deploy2.png)