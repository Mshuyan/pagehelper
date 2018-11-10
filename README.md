# pagehelper
>  学习使用pagehelper做mybatis分页
>
> 本例程结合springboot使用

## 代码示例

+ pom

  ```xml
  <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper-spring-boot-starter</artifactId>
  </dependency>
  ```

+ controller

  ```java
  @GetMapping("/product/list")
  public PageInfo<ProductListItemDto> productList(PageParam pageParam, ProductListParamDto paramDto){
      List<ProductListItemDto> list = productService.findListItemByClassificationAndBrandOrderBySales(pageParam, paramDto);
      return new PageInfo<>(list);
  }
  ```

  + `PageParam`是自定义的实体类，用于接受关于分页的请求参数

    ```java
    @Data
    public class PageParam {
        private Integer pageSize = 12;
        private Integer pageNum = 1;
    }
    ```

  + `PageInfo`是`pagehelper`自带的类，功能如下：

    + 对返回的集合进行分页，将分页后的结果封装到`PageInfo`对象中

    + 将分页过程中已知的狠毒哦分页信息封装到`PageInfo`对象中

      部分代码如下：

      ```java
      public class PageInfo<T> extends PageSerializable<T> {
          private int pageNum;
          private int pageSize;
          private int size;
          private int startRow;
          private int endRow;
          private int pages;
          private int prePage;
          private int nextPage;
          private boolean isFirstPage;
          private boolean isLastPage;
          private boolean hasPreviousPage;
          private boolean hasNextPage;
          private int navigatePages;
          private int[] navigatepageNums;
          private int navigateFirstPage;
          private int navigateLastPage;
      ```

+ service

  ```java
  @Override
  public List<ProductListItemDto> findListItemByClassificationAndBrandOrderBySales(PageParam pageParam, ProductListParamDto paramDto) {
      PageHelper.startPage(pageParam.getPageNum(), pageParam.getPageSize());
      return productSpuMapper.findListItemByClassificationAndBrandOrderBySales(paramDto);
  }
  ```

  + 将接收到的关于分页的请求参数设置到`PageHelper`工具类中

  + 查询出需要进行分页的所有数据

+ dao

  > 正常查询出需要进行分页的所有数据即可
