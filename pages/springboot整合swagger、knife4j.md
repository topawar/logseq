- ### springboot3版本
- 1. 引入maven
	- ``` maven
	  <dependency>
	      <groupId>com.github.xiaoymin</groupId>
	      <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
	      <version>4.1.0</version>
	  </dependency>
	  ```
- 2. 在yaml文件中配置
	- ```yaml
	  # springdoc-openapi项目配置
	  springdoc:
	    swagger-ui:
	      path: /swagger-ui.html
	      tags-sorter: alpha
	      operations-sorter: alpha
	    api-docs:
	      path: /v3/api-docs
	    group-configs:
	      - group: 'default'
	        paths-to-match: '/**'
	        packages-to-scan: com.xiaominfo.knife4j.demo.web
	  # knife4j的增强配置，不需要增强可以不配
	  knife4j:
	    enable: true
	    setting:
	      language: zh_cn
	  ```
- 3. 自定义swagger配置类
	- ```java
	  @Configuration
	  public class SwaggerConfig {
	      /**
	       * 根据@Tag 上的排序，写入x-order
	       *
	       * @return the global open api customizer
	       */
	      @Bean
	      public GlobalOpenApiCustomizer orderGlobalOpenApiCustomizer() {
	          return openApi -> {
	              if (openApi.getTags() != null) {
	                  openApi.getTags().forEach(tag -> {
	                      Map<String, Object> map = new HashMap<>();
	                      map.put("x-order", RandomUtil.randomInt(0, 100));
	                      tag.setExtensions(map);
	                  });
	              }
	              if (openApi.getPaths() != null) {
	                  openApi.addExtension("x-test123", "333");
	                  openApi.getPaths().addExtension("x-abb", RandomUtil.randomInt(1, 100));
	              }
	  
	          };
	      }
	  
	      @Bean
	      public OpenAPI customOpenAPI() {
	          return new OpenAPI()
	                  .info(new Info()
	                          .title("seed用户系统API")
	                          .version("1.0")
	                          .description("seed后台管理系统")
	                          .termsOfService("http://locahost:8082")
	                          .license(new License().name("Apache 2.0")
	                                  .url("http://locahost:8082")));
	      }
	  ```
	- @Configuration
	- public class SwaggerConfig {
	- /**
	- * 根据@Tag 上的排序，写入x-order
	- *
	- * @return the global open api customizer
	- */
	- @Bean
	- public GlobalOpenApiCustomizer orderGlobalOpenApiCustomizer() {
	- return openApi -> {
	- if (openApi.getTags() != null) {
	- openApi.getTags().forEach(tag -> {
	- Map<String, Object> map = new HashMap<>();
	- map.put("x-order", RandomUtil.randomInt(0, 100));
	- tag.setExtensions(map);
	- });
	- }
	- if (openApi.getPaths() != null) {
	- openApi.addExtension("x-test123", "333");
	- openApi.getPaths().addExtension("x-abb", RandomUtil.randomInt(1, 100));
	- }
	- };
	- }
	- @Bean
	- public OpenAPI customOpenAPI() {
	- return new OpenAPI()
	- .info(new Info()
	- .title("seed用户系统API")
	- .version("1.0")
	- .description("seed后台管理系统")
	- .termsOfService("http://locahost:8082")
	- .license(new License().name("Apache 2.0")
	- .url("http://locahost:8082")));
	- ```java
	  @Configuration
	  public class SwaggerConfig {
	      /**
	       * 根据@Tag 上的排序，写入x-order
	       *
	       * @return the global open api customizer
	       */
	      @Bean
	      public GlobalOpenApiCustomizer orderGlobalOpenApiCustomizer() {
	          return openApi -> {
	              if (openApi.getTags() != null) {
	                  openApi.getTags().forEach(tag -> {
	                      Map<String, Object> map = new HashMap<>();
	                      map.put("x-order", RandomUtil.randomInt(0, 100));
	                      tag.setExtensions(map);
	                  });
	              }
	              if (openApi.getPaths() != null) {
	                  openApi.addExtension("x-test123", "333");
	                  openApi.getPaths().addExtension("x-abb", RandomUtil.randomInt(1, 100));
	              }
	  
	          };
	      }
	  
	      @Bean
	      public OpenAPI customOpenAPI() {
	          return new OpenAPI()
	                  .info(new Info()
	                          .title("seed用户系统API")
	                          .version("1.0")
	                          .description("seed后台管理系统")
	                          .termsOfService("http://locahost:8082")
	                          .license(new License().name("Apache 2.0")
	                                  .url("http://locahost:8082")));
	      }
	  ```
-
- 完成后就可以在浏览器中输入 当前项目地址+端口`/doc.html`访问了
- [[$red]]==如果自定义了拦截器，要放行swagger的资源==
	- `"/swagger-resources/**", "/webjars/**", "/v3/**", "/swagger-ui.html/**"`
- [[$red]]==`controller`层接受的参数是封装的类要在参数前面加上`@ParameterObject`注解==
-