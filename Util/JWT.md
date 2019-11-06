## JWT

#### 1 导入依赖

```xml
        <!--JWT(Json Web Token)登录支持-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>0.9.0</version>
        </dependency>
```

#### 2 用法

作为一个工具类，定义在util包中，一般该类定义以下几种属性和方法：

> 属性：
>
> 1. 签名方法
> 2. 期限时间长短
>
> 方法：
>
> 1. 生成token的方法
> 2. 解析token的方法
> 3. 获取用户名的方法
> 4. 获取到期时间的方法
> 5. 生成到期时间的方法
> 6. 是否可以刷新toekn的方法
> 7. 判断token是否有效的方法
> 8. 判断token是否到期的方法