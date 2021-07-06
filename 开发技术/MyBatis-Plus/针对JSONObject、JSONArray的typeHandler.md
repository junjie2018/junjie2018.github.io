实际上我已经有技术实现针对List的TypeHandler了，之所以还开发这个TypeHandler是因为我们的旧代码使用了JSONArray、JSONObject，旧代码之前的使用方案是先将数据检索成字符串，然后再用fastjson转成JSONObject或者JSONArray，为了兼顾这部分需求，所以我开发了这个TypeHandler。

~~~ java

@Slf4j
@MappedTypes({JSONObject.class, JSONArray.class})
public class FastJsonTypHandler extends BaseTypeHandler<Object> {
    @Override
    public void setNonNullParameter(PreparedStatement preparedStatement, int i, Object parameter, JdbcType jdbcType) throws SQLException {
        PGobject insertObj = new PGobject();
        insertObj.setType("jsonb");
        insertObj.setValue(JSON.toJSONString(parameter));
        preparedStatement.setObject(i, insertObj);
    }

    @Override
    public Object getNullableResult(ResultSet resultSet, String columnName) throws SQLException {
        final String jsonInDb = resultSet.getString(columnName);
        return StringUtils.isEmpty(jsonInDb) ? null : JSON.parse(jsonInDb);
    }

    @Override
    public Object getNullableResult(ResultSet resultSet, int columnIndex) throws SQLException {
        final String jsonInDb = resultSet.getString(columnIndex);
        return StringUtils.isEmpty(jsonInDb) ? null : JSON.parse(jsonInDb);
    }

    @Override
    public Object getNullableResult(CallableStatement callableStatement, int columnIndex) throws SQLException {
        final String jsonInDb = callableStatement.getString(columnIndex);
        return StringUtils.isEmpty(jsonInDb) ? null : JSON.parse(jsonInDb);
    }
}

~~~

这里踩了一个坑，我之间将`@MappedTypes`设置成`@MappedTypes({Object.class})`，结果MyBatis-Plus生成出来的SQL都包含一个双引号，估计是每个字段都走了这个TypeHandler。

![2021-06-16-09-35-47](https://junjie2018sz.oss-cn-shenzhen.aliyuncs.com/images/2021-06-16-09-35-47.png)

框架中原来的TypeHandler生成的jsonb字段的值，双引号前会加反斜线，不是太方便使用。