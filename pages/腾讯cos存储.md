- ## 使用cos必须引入的maven依赖
  ``` maven
  <dependency>
       <groupId>com.qcloud</groupId>
       <artifactId>cos_api</artifactId>
       <version>5.6.97</version>
  </dependency>
  ```
- ## 推荐使用临时密钥
	- ### ==why？==
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
	- ### 临时密钥
		- [临时密钥（临时访问凭证）](https://www.tencentcloud.com/document/product/1150/49452) 是通过 CAM 云 API 提供的接口，获取到权限受限的密钥。
		- COS API 可以使用临时密钥计算签名，用于发起 COS API 请求。
		- COS API 请求使用临时密钥计算签名时，需要用到获取临时密钥接口返回信息中的三个字段，如下：
		  collapsed:: true
			- TmpSecretId
			- TmpSecretKey
			- Token
			- 代码示例
			- ``` java
			  // 根据 github 提供的 maven 集成方法导入 java sts sdk，使用 3.1.0 及更高版本
			  public class Demo {
			      public static void main(String[] args) {
			          TreeMap<String, Object> config = new TreeMap<String, Object>();
			  
			          try {
			              //这里的 SecretId 和 SecretKey 代表了用于申请临时密钥的永久身份（主账号、子账号等），子账号需要具有操作存储桶的权限。
			              String secretId = System.getenv("secretId");//用户的 SecretId，建议使用子账号密钥，授权遵循最小权限指引，降低使用风险。子账号密钥获取可参见 https://www.tencentcloud.com/document/product/598/37140?from_cn_redirect=1
			               String secretKey = System.getenv("secretKey");//用户的 SecretKey，建议使用子账号密钥，授权遵循最小权限指引，降低使用风险。子账号密钥获取可参见 https://www.tencentcloud.com/document/product/598/37140?from_cn_redirect=1
			              // 替换为您的云 api 密钥 SecretId
			              config.put("secretId", secretId);
			              // 替换为您的云 api 密钥 SecretKey
			              config.put("secretKey", secretKey);
			  
			              // 设置域名: 
			              // 如果您使用了腾讯云 cvm，可以设置内部域名
			              //config.put("host", "sts.internal.tencentcloudapi.com");
			  
			              // 临时密钥有效时长，单位是秒，默认 1800 秒，目前主账号最长 2 小时（即 7200 秒），子账号最长 36 小时（即 129600）秒
			              config.put("durationSeconds", 1800);
			  
			              // 换成您的 bucket
			              config.put("bucket", "examplebucket-1250000000");
			              // 换成 bucket 所在地区
			              config.put("region", "ap-guangzhou");
			  
			              // 这里改成允许的路径前缀，可以根据自己网站的用户登录态判断允许上传的具体路径
			              // 列举几种典型的前缀授权场景：
			              // 1、允许访问所有对象："*"
			              // 2、允许访问指定的对象："a/a1.txt", "b/b1.txt"
			              // 3、允许访问指定前缀的对象："a*", "a/*", "b/*"
			              // 如果填写了“*”，将允许用户访问所有资源；除非业务需要，否则请按照最小权限原则授予用户相应的访问权限范围。
			              config.put("allowPrefixes", new String[] {
			                      "exampleobject",
			                      "exampleobject2"
			              });
			  
			              // 密钥的权限列表。必须在这里指定本次临时密钥所需要的权限。
			              // 简单上传、表单上传和分块上传需要以下的权限，其他权限列表请参见 https://www.tencentcloud.com/document/product/436/30580
			              String[] allowActions = new String[] {
			                       // 简单上传
			                      "name/cos:PutObject",
			                      // 表单上传、小程序上传
			                      "name/cos:PostObject",
			                      // 分块上传
			                      "name/cos:InitiateMultipartUpload",
			                      "name/cos:ListMultipartUploads",
			                      "name/cos:ListParts",
			                      "name/cos:UploadPart",
			                      "name/cos:CompleteMultipartUpload"
			              };
			              config.put("allowActions", allowActions);
			                  /**
			               * 设置condition（如有需要）
			               //# 临时密钥生效条件，关于condition的详细设置规则和COS支持的condition类型可以参考 https://www.tencentcloud.com/document/product/436/71307?from_cn_redirect=1
			               final String raw_policy = "{\n" +
			               "  \"version\":\"2.0\",\n" +
			               "  \"statement\":[\n" +
			               "    {\n" +
			               "      \"effect\":\"allow\",\n" +
			               "      \"action\":[\n" +
			               "          \"name/cos:PutObject\",\n" +
			               "          \"name/cos:PostObject\",\n" +
			               "          \"name/cos:InitiateMultipartUpload\",\n" +
			               "          \"name/cos:ListMultipartUploads\",\n" +
			               "          \"name/cos:ListParts\",\n" +
			               "          \"name/cos:UploadPart\",\n" +
			               "          \"name/cos:CompleteMultipartUpload\"\n" +
			               "        ],\n" +
			               "      \"resource\":[\n" +
			               "          \"qcs::cos:ap-shanghai:uid/1250000000:examplebucket-1250000000/*\"\n" +
			               "      ],\n" +
			               "      \"condition\": {\n" +
			               "        \"ip_equal\": {\n" +
			               "            \"qcs:ip\": [\n" +
			               "                \"192.168.1.0/24\",\n" +
			               "                \"101.226.100.185\",\n" +
			               "                \"101.226.100.186\"\n" +
			               "            ]\n" +
			               "        }\n" +
			               "      }\n" +
			               "    }\n" +
			               "  ]\n" +
			               "}";
			  
			               config.put("policy", raw_policy);
			               */                
			            
			            
			              Response response = CosStsClient.getCredential(config);
			              System.out.println(response.credentials.tmpSecretId);
			              System.out.println(response.credentials.tmpSecretKey);
			              System.out.println(response.credentials.sessionToken);
			          } catch (Exception e) {
			              e.printStackTrace();
			              throw new IllegalArgumentException("no valid secret !");
			          }
			      }
			  }
			  
			  ```
	- ### 使用临时密钥访问 COS
		- COS API 使用临时密钥访问 COS 服务时，通过 ==x-cos-security-token 字段传递临时 sessionToken==，通过临时 SecretId 和 SecretKey 计算签名。
			- ``` java
			  // 根据 github 提供的 maven 集成方式导入 cos xml java sdk
			  import com.qcloud.cos.*;
			  import com.qcloud.cos.auth.*;
			  import com.qcloud.cos.exception.*;
			  import com.qcloud.cos.model.*;
			  import com.qcloud.cos.region.*;
			  public class Demo {
			      public static void main(String[] args) throws Exception {
			  
			          // 用户基本信息
			          String tmpSecretId = "COS_SECRETID";   // 替换为 STS 接口返回给您的临时 SecretId 
			          String tmpSecretKey = "COS_SECRETKEY";  // 替换为 STS 接口返回给您的临时 SecretKey
			          String sessionToken = "Token";  // 替换为 STS 接口返回给您的临时 Token
			  
			          // 1 初始化用户身份信息(secretId, secretKey)
			          COSCredentials cred = new BasicCOSCredentials(tmpSecretId, tmpSecretKey);
			          // 2 设置 bucket 区域,详情请参见 COS 地域 https://www.tencentcloud.com/document/product/436/6224?from_cn_redirect=1
			          ClientConfig clientConfig = new ClientConfig(new Region("ap-guangzhou"));
			          // 3 生成 cos 客户端
			          COSClient cosclient = new COSClient(cred, clientConfig);
			          // bucket 名需包含 appid
			          String bucketName = "examplebucket-1250000000";
			  
			          String key = "exampleobject";
			          // 上传 object, 建议 20M 以下的文件使用该接口
			          File localFile = new File("src/test/resources/text.txt");
			          PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, key, localFile);
			  
			          // 设置 x-cos-security-token header 字段
			          ObjectMetadata objectMetadata = new ObjectMetadata();
			          objectMetadata.setSecurityToken(sessionToken);
			          putObjectRequest.setMetadata(objectMetadata);
			  
			          try {
			              PutObjectResult putObjectResult = cosclient.putObject(putObjectRequest);
			              // 成功：putobjectResult 会返回文件的 etag
			              String etag = putObjectResult.getETag();
			          } catch (CosServiceException e) {
			              //失败，抛出 CosServiceException
			              e.printStackTrace();
			          } catch (CosClientException e) {
			              //失败，抛出 CosClientException
			              e.printStackTrace();
			          }
			  
			          // 关闭客户端
			          cosclient.shutdown();
			  
			      }
			  }
			  
			  ```
		- 创建连接时建议直接指定`sessionToken`
		- ``` java
		   COSCredentials cred = new BasicSessionCredentials(tmpSecretId, tmpSecretKey, tmpSessionToken);
		  ```