#### jackson
##### 引入
```sql
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.3</version>
</dependency>
```

##### 代码块
```sql
 private static final ObjectMapper OBJECT_MAPPER = new ObjectMapper();

    static {
        OBJECT_MAPPER.disable(SerializationFeature.FAIL_ON_EMPTY_BEANS);
        OBJECT_MAPPER.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
    }

    public static String toJson(Object obj) {
        try {
            return OBJECT_MAPPER.writeValueAsString(obj);
        } catch (Exception e) {
            log.error("xx", e);
        }
        return "";
    }

    public static <T> T toObj(String str, Class<T> cls) {
        try {
            return OBJECT_MAPPER.readValue(str, cls);
        } catch (Exception e) {
            log.error("xx", e);
        }
        return null;
    }

    public static <T> T toObj(String content, TypeReference<T> valueTypeRef) {
        try {
            return OBJECT_MAPPER.readValue(content, valueTypeRef);
        } catch (Exception e) {
            log.error("xx", e);
        }
        return null;
    }

    public static Map<String, Object> toMap(Object obj) {
        return toObj(toJson(obj), Map.class);
    }
```

##### 常用注解
###### 字段名称转换
```sql
@JsonProperty("num_count")
private Integer numCount;
```

###### 时间格式化
```sql
    @JsonFormat(timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
    @org.springframework.format.annotation.DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private Date endTime;
```

###### 序列化时忽略字段
```sql
    @JsonIgnore
    private Boolean xx;
```




