---
title: mybatis和enum的映射配置
date: 2019-10-22 10:35
author: lqpl66
categories: 
   - mybatis
   - 配置   
tags:  
   - mybatis
---

​	之前在开发中遇到数据库字段属性是分类的情况，如不同场景的分类，日期的种类（年月日）等，如果分类较多的话，同时前端也需要的相关的下拉框等业务需求的话，一般会设计类型的基类表设计，并与相关业务表做外键关联，但是有些分类较少，就无须这般设计，业务表设计时直接定义该属性，同时采用枚举映射的方式，这样直接可以返回给前端对应的数据库字段和对应的中文意义。

#### 配置

```java 
public interface IntEnum {
    /**
     * 获取枚举value
     * @return
     */
    String getValue();

}
```

```java
@MappedJdbcTypes({JdbcType.INTEGER,JdbcType.CHAR})
@MappedTypes({VenueType.class,***.class,***.class})

public class IntEnumTypeHandler extends BaseTypeHandler<IntEnum> {
	private Class<IntEnum> type;
	public IntEnumTypeHandler(Class<IntEnum> type) {
		if (type == null) {
			throw new IllegalArgumentException("Type argument cannot be null");
		}
		this.type = type;
	}
	private IntEnum convert(String status) {
		IntEnum[] objs = type.getEnumConstants();
		for (IntEnum em : objs) {
			if (em.getValue().equals(status) ) {
				return em;
			}
		}
		return null;
	}
	@Override
	public IntEnum getNullableResult(ResultSet rs, String columnName) throws SQLException {
		return convert(rs.getString(columnName));
	}

	@Override
	public IntEnum getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
		return convert(rs.getString(columnIndex));
	}

	@Override
	public IntEnum getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
		return convert(cs.getString(columnIndex));
	}

	@Override
	public void setNonNullParameter(PreparedStatement ps, int i, IntEnum enumObj, JdbcType jdbcType)
			throws SQLException {
		// baseTypeHandler已经帮我们做了parameter的null判断
		ps.setString(i, enumObj.getValue());

	}

```

继承BaseTypeHandler类做数据库字段属性的自定义转换

```java 
 *  1.@MappedJdbcTypes定义的是JdbcType类型，这里的类型不可自己随意定义，
 *   必须要是枚举类org.apache.ibatis.type.JdbcType所枚举的数据类型。
 *  2.@MappedTypes定义的是JavaType的数据类型，描述了哪些Java类型可被拦截。
 *  3.在我们启用了我们自定义的这个TypeHandler之后，数据的读写都会被这个类所过滤
```

举例枚举类型

```java 
@JSONType(serializeEnumAsJavaBean = true)
@JsonFormat(shape = JsonFormat.Shape.OBJECT)
public enum VenueType implements IntEnum {

	museum("1","博物馆"),
	panting("2","美术馆"),
	art("3","艺术馆"),
	library("4","图书馆");

	private String value;
	private String name;
	VenueType(String value, String name) {
		this.value = value;
		this.name = name;
	}

	@Override
	public String getValue() {
		return value;
	}

	public void setValue(String value) {
		this.value = value;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}

```

#### 使用

mybatis.type-handlers-package=com.***.domain.enums.handler

单个属性配置xml：

```java
　<result column="type" property="type" javaType="String" jdbcType="VARCHAR" typeHandler="com.***.IntEnumTypeHandler"/>
```

或者通用配置,在配置文件中定义：

```java 
mybatis.type-handlers-package=com.***.domain.enums.handler
```

