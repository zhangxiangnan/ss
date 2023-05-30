### choose
junit
mockito
powermock
TestNG
EasyMock

#### Mockito
##### MockBean
mockbean定义的对象如xx的所有方法不会真实调用
````sql
@MockBean(org.springframework.boot.test.mock.mockito)
private XX xx;
````

##### mock有返回值方法

```sql
@MockBean(org.springframework.boot.test.mock.mockito)
private XX xx;

Mockito.when(xx.aa()).thenReturn(..);
```

##### mock无返回值方法
``` java
org.mockito.Mockito.doNothing().when(xx).xxmethod();
```

##### 设置成员变量的属性
``` java
org.springframework.test.util.ReflectionTestUtils.setField(xxInstance, "xxfield", "xxval");
```

##### mock抛出异常
```sql
when(xx.doxx(any(), any())).thenThrow(new XXException("xx"));
```

##### assert list相等
```sql
Assertions.assertIterableEquals(xx,xx)
```