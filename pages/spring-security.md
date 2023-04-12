- #安全#config#自定义#spring#maven
- 1. 引入依赖
	- 使用springboot时（会根据springboot的版本选择对应的版本，不需要指定版本）
	- ``` maven
	  <dependencies>
	  	<dependency>
	  		<groupId>org.springframework.boot</groupId>
	  		<artifactId>spring-boot-starter-security</artifactId>
	  	</dependency>
	  </dependencies>
	  ```
	- 不使用springboot
		- ``` maven
		  <dependencies>
		  	<!-- ... other dependency elements ... -->
		  	<dependency>
		  		<groupId>org.springframework.security</groupId>
		  		<artifactId>spring-security-web</artifactId>
		  	</dependency>
		  	<dependency>
		  		<groupId>org.springframework.security</groupId>
		  		<artifactId>spring-security-config</artifactId>
		  	</dependency>
		  </dependencies>
		  ```
- 2. 自定义配置类