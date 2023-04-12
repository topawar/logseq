- #mybatis-plus#sql#springboot#yml#config
- ### 逻辑删除
	- #### 说明:
		- 只对自动注入的 sql 起效:
			- 插入: 不作限制
			  查找: 追加 where 条件过滤掉已删除数据,如果使用 wrapper.entity 生成的 where 条件也会自动追加该字段
			  更新: 追加 where 条件防止更新到已删除数据,如果使用 wrapper.entity 生成的 where 条件也会自动追加该字段
			  删除: 转变为 更新
			  例如:
			- 删除: `update user set deleted=1 where id = 1 and deleted=0`
			  查找: `select id,name,deleted from user where deleted=0`
			- 字段类型支持说明:
				- >支持所有数据类型(推荐使用 Integer,Boolean,LocalDateTime)
				  如果数据库字段使用datetime,逻辑未删除值和已删除值支持配置为字符串null,另一个值支持配置为函数来获取值如now()
				- 附录:
				- >逻辑删除是为了方便数据恢复和保护数据本身价值等等的一种方案，但实际就是删除。
				  如果你需要频繁查出来看就不应使用逻辑删除，而是以一个状态去表示。
	- #### 步骤1：配置application.yml
		- ```yml
		  mybatis-plus:
		    global-config:
		      db-config:
		        logic-delete-field: flag #全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
		        logic-delete-value: 1  #逻辑已删除值(默认为 1)
		        logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
		  ```
	- #### 步骤2：实体类字段上加上@TableLogic注解
		- id:: 638568d3-3ccc-44c4-82a9-e5226d1e9b2c
		  ```java
		  @TableLogic
		  private Integer deleted;
		  ```
### mapUnderscoreToCamelCase
	- 类型： boolean
	- 默认值： true
	- >是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN（下划线命名） 到经典 Java 属性名 aColumn（驼峰命名） 的类似映射。
	- ```yml
	  map-underscore-to-camel-case: false
	  ```