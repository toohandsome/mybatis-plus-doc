# 动态表名 SQL 解析器

> 该功能解决动态表名支持 `3.1.1` 以上版本

源码文件：

👉 [DynamicTableNameParser](https://github.com/baomidou/mybatis-plus/tree/3.0/mybatis-plus-extension/src/main/java/com/baomidou/mybatisplus/extension/parsers)

- 具体使用参考多租户

实现 ITableNameHandler 接口注入到 DynamicTableNameParser 处理器链中，将动态表名解析器注入到 MP 解析链。


``` 
::: warning 注意！
- 原理为解析替换设定表名为处理器的返回表名，表名建议可以定义复杂一些避免误替换
- 例如：真实表名为 user 设定为 mp_dt_user 处理器替换为 user_2019 等
:::

参考代码：

```
@Bean
	public PaginationInterceptor paginationInterceptor() {
		PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
		List<ISqlParser> sqlParserList = new ArrayList<>();

		DynamicTableNameParser dynamicTableNameParser = new DynamicTableNameParser();

		ITableNameHandler myTableNameHandler = new ITableNameHandler() {
      // 此处定义表名替换规则
			@Override
			public String dynamicTableName(MetaObject metaObject, String sql, String tableName) {
				return tableName + "01";
			}
		};

		Map<String, ITableNameHandler> tableNameHandlerMap = new HashMap<>();
    // 此处的 user 要与原始表名保持一致
		tableNameHandlerMap.put("user", myTableNameHandler);
		dynamicTableNameParser.setTableNameHandlerMap(tableNameHandlerMap);
		sqlParserList.add(dynamicTableNameParser);
		paginationInterceptor.setSqlParserList(sqlParserList);

		return paginationInterceptor;
	}
```
