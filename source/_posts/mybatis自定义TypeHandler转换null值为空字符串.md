---

title: mybatis自定义TypeHandler转换null值为空字符串

---

## 背景
目前接手的一个新项目中生成报表需求中，读取数据库数据之后的非常多String类型的字段，在写入csv报表的时候如果字段是null值 会默认显示为null，显示字符串null在报表中显然不合适

考虑可以在sql查询时使用ifnull函数，但此类型字段过多，写查询语句太过繁琐
因此想到自定义一个typeHandler来转换数据库中的null字段为 空字符串


## 解决方案
###  1、自定义handler

#### 注意事项：
除了正常typeHandler继承的一些方法之外还需要重写BaseTypeHandler的getResult方法，因为原方法获取自定义handler处理后的result之后还会再做一层判断，如下：
```java
 	 if (rs.wasNull()) {
      return null;
    } else {
      return result;
    }
  }
```
rs.wasNull() 判断结果集是否为null，此判断仍然为true导致返回结果仍然为null因此还需要重写该方法去掉这一判断,下面上代码

```java
import org.apache.ibatis.executor.result.ResultMapException;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * @author liwanping
 * @since 2019-05-15
 */
@MappedTypes({String.class})
@MappedJdbcTypes(JdbcType.VARCHAR)
public class CustomStringTypeHandler extends BaseTypeHandler<String> {

	
    @Override
    public String getResult(ResultSet rs, String columnName) {
        String result;
        try {
            result = getNullableResult(rs, columnName);
        } catch (Exception e) {
            throw new ResultMapException("Error attempting to get column '" + columnName + "' from result set.  Cause: " + e, e);
        }
        return result;
    }

    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType)
            throws SQLException {
        ps.setString(i, parameter);
    }

    @Override
    public String getNullableResult(ResultSet rs, String columnName)
            throws SQLException {
        return rs.getString(columnName) == null? "" : rs.getString(columnName);
    }

    @Override
    public String getNullableResult(ResultSet rs, int columnIndex)
            throws SQLException {
        return rs.getString(columnIndex) == null? "" : rs.getString(columnIndex);
    }

    @Override
    public String getNullableResult(CallableStatement cs, int columnIndex)
            throws SQLException {
        return cs.getString(columnIndex) == null? "" : cs.getString(columnIndex);
    }
}
```
### 2、springboot配置
mybatis.type-handlers-package=handler所在包名
启动springboot应用会自动扫描到该handler并加载

## Reference
 [mybatis official doc - typehandler](http://www.mybatis.org/mybatis-3/configuration.html#typeHandlers)