- ## 使用cos必须引入的maven依赖
	- [[#red]]==cos云存储功能==
	  ``` maven
	  <dependency>
	       <groupId>com.qcloud</groupId>
	       <artifactId>cos_api</artifactId>
	       <version>5.6.97</version>
	  </dependency>
	  ```
- ## 推荐使用临时密钥
	- ### [[$red]]==why？==
		- >腾讯云 COS 服务在使用时需要对请求进行访问管理。通过临时密钥机制，您可以临时授权您的 App 访问您的存储资源，而不会泄露您的永久密钥。密钥的有效期由您指定，过期后自动失效。通过生成的临时密钥来对上传或者下载请求进行签名，从而保证您数据的安全性。
	- ### 临时密钥架构
		- ![架构](https://camo.githubusercontent.com/1e646823234a70454b796ba35de46cbc634c749cbf2c4c72b6ebe6839e9ad130/687474703a2f2f6d632e71636c6f7564696d672e636f6d2f7374617469632f696d672f62316531383761396563313239666663373636633037613733336566346464362f696d6167652e6a7067)
		- 应用 APP：即用户手机上的 App。
		- COS：[腾讯云对象存储](https://cloud.tencent.com/product/cos)，负责存储 App 上传的数据。
		- CAM：[腾讯云访问管理](https://cloud.tencent.com/product/cam)，用于生成 COS 的临时密钥。
		- 应用服务器：用户自己的后台服务器，这里用于获取临时密钥，并返回给应用 App。
	- ### 获取永久密钥
		- 临时密钥需要通过永久密钥才能生成。请登录 [腾讯云访问管理控制台](https://console.cloud.tencent.com/cam/capi) 获取，包含：
			- SecretId
			- SecretKey
	- ### maven依赖
		- ```maven
		  <dependency>
		      <groupId>com.qcloud</groupId>
		      <artifactId>cos-sts_api</artifactId>
		      <version>3.1.0</version>
		  </dependency> 
		  ```
	-