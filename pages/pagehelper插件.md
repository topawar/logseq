- #springboot#config#分页
- ## maven引入
	- springboot
	- ``` maven
	  <dependency>
	      <groupId>com.github.pagehelper</groupId>
	      <artifactId>pagehelper-spring-boot-starter</artifactId>
	      <version>1.2.5</version>
	  </dependency>
	  ```
	- spring
	- ``` maven
	  <dependency>
	      <groupId>com.github.pagehelper</groupId>
	      <artifactId>pagehelper</artifactId>
	      <version>5.3.0</version>
	  </dependency>
	  ```
- ## 代码实现
	- 注意：`PageHelper.startPage`要放到查询数据库的列表上面，否则无法分页效果无法实现。
	- ``` java
	  /**
	   * 分页查询
	   * @param pageNum 记录页数
	   * @param pageSize 单页记录数量
	   * @return
	   */
	  @ResponseBody
	  @RequestMapping("/findPage")
	  public List<Student> findPage(@RequestParam int pageNum, @RequestParam int pageSize) {
	      // 设置分页查询参数
	      PageHelper.startPage(pageNum,pageSize);
	      List<Student> studentList = studentService.findList();
	  
	      for(Student student : studentList) {
	          System.out.println("element : " + student);
	      }
	  
	      // 封装分页查询结果到 PageInfo 对象中以获取相关分页信息
	      PageInfo pageInfo = new PageInfo( studentList );
	      System.out.println("总页数: " + pageInfo.getPages());
	      System.out.println("总记录数: " + pageInfo.getTotal());
	      System.out.println("当前页数: " + pageInfo.getPageNum());
	      System.out.println("当前页面记录数量: " + pageInfo.getSize());
	  
	      return pageInfo.getList();
	  }
	  ```