## Spring Security

#### WebSecurityConfigurerAdapter 

一个适配器，需要自己写个配置类去继承他,然后编写自己所特殊需要的配置

* 配置方法
  1. 每个配置使用==and==结尾
  2. authorizeRequests()配置路径拦截，表明路径访问所对应的权限，角色，认证信息。
  3. formLogin()对应表单认证相关的配置
  4. logout()对应了注销相关的配置
  
* 需要覆写的三个方法

  1. 认证管理器配置方法

     void configure(AuthenticationManagerBuilder auth)

  2. 核心过滤器配置方法

     void configure(WebSecurity web)用来配置WebSecurity，多用ignoring()方法来忽略Spring Security对静态资源的控制

  3. 安全过滤器链配置方法

     void configure(HttpSecurity http)

* HttpSecurity配置

  ```java
  protected void configure(HttpSecurity http) throws Exception {
            logger.debug("Using default configure(HttpSecurity). If subclassed this will potentially override subclass configure(HttpSecurity).");
  
            http
                .authorizeRequests()
                    .anyRequest().authenticated()
                    .and()
                .formLogin().and()
                .httpBasic();
        }
  ```

  上面是Spring Security的默认配置，该应用的功能如下：

  1. 所有请求访问都需要授权
  2. 使用form表单进行登陆
  3. 防止CSRF工具、XSS工具
  4. 启动Http Basic认证

* 自定义登陆

  自定义访问控制主要是通过HttpSecurity来构建，默认提供了三种登陆方式：

  1. `fromLogin()`普通表单登陆
  2. `oauth2Login()`基于OAuth2.0认证/授权下协议
  3. `openidLogin()`基于openID身份认证规范

  from表单登陆

  1. `apply(C configurer)`构造一个`AbstractAuthenticationFilterConfigurer`
  2. `fromLogin()`方法自定义`FormLoginConfigurer`
     - **`loginPage(String loginPage)`** : 登录 **页面**而并不是接口，对于前后分离模式需要我们进行改造 默认为 `/login`。
     - **`loginProcessingUrl(String loginProcessingUrl)`** 实际表单向后台提交用户信息的 `Action`，再由过滤器`UsernamePasswordAuthenticationFilter` 拦截处理，该 `Action` 其实不会处理任何逻辑。
     - **`usernameParameter(String usernameParameter)`** 用来自定义用户参数名，默认 `username` 。
     - **`passwordParameter(String passwordParameter)`** 用来自定义用户密码名，默认 `password`
     - **`failureUrl(String authenticationFailureUrl)`** 登录失败后会重定向到此路径， 一般前后分离不会使用它。
     - **`failureForwardUrl(String forwardUrl)`** 登录失败会转发到此， 一般前后分离用到它。 可定义一个 `Controller` （控制器）来处理返回值,但是要注意 `RequestMethod`。
     - **`defaultSuccessUrl(String defaultSuccessUrl, boolean alwaysUse)`** 默认登陆成功后跳转到此 ，如果 `alwaysUse` 为 `true` 只要进行认证流程而且成功，会一直跳转到此。一般推荐默认值 `false`
     - **`successForwardUrl(String forwardUrl)`** 效果等同于上面 `defaultSuccessUrl` 的 `alwaysUse` 为 `true` 但是要注意 `RequestMethod`。
     - **`successHandler(AuthenticationSuccessHandler successHandler)`** 自定义认证成功处理器，可替代上面所有的 `success` 方式
     - **`failureHandler(AuthenticationFailureHandler authenticationFailureHandler)`** 自定义失败处理器，可替代上面所有的 `failure` 方式
     - **`permitAll(boolean permitAll)`** **form** 表单登录是否放开

* 自定义退出登陆

  Spring Security钟的退出登录

  1. LogoutFilter

     退出登录逻辑是由过滤器==LogoutFilter==来执行的。它持有三个接口类型的属性：

     * **`RequestMatcher logoutRequestMatcher`** 这个用来拦截退出请求的 `URL`

     * **`LogoutHandler handler`** 用来处理退出的具体逻辑
     * **`LogoutSuccessHandler logoutSuccessHandler`** 退出成功后执行的逻辑

  2. LogoutConfigurer

     一般不会直接操作 `LogoutFilter` ，而是通过 `LogoutConfigurer` 来配置 `LogoutFilter`。 你可以通过 `HttpSecurity#logout()` 方法来初始化一个 `LogoutConfigurer` 。

     * 退出登录请求URL

        `logoutRequestMatcher(RequestMatcher logoutRequestMatcher)`、`logoutUrl(Sring logoutUrl)` 两种方式来定义退出登录请求的 `URL` 

     * 处理具体逻辑

       默认提供了以下配置：

       - `clearAuthentication(boolean clearAuthentication)` 是否在退出时清除当前用户的认证信息
       - `deleteCookies(String... cookieNamesToClear)` 删除指定的 `cookies`
       - `invalidateHttpSession(boolean invalidateHttpSession)` 是否移除 `HttpSession`

       如果以上不能满足需求，需要自定义==LogoutHandler==

     * 退出成功逻辑

       - `logoutSuccessUrl(String logoutSuccessUrl)` 退出成功后会被重定向到此 `URL` ，**你可以写一个Controller 来完成最终返回，但是需要支持 `GET` 请求和 匿名访问** 。 通过 `setDefaultTargetUrl` 方法注入到 `LogoutSuccessHandler`
       - `defaultLogoutSuccessHandlerFor(LogoutSuccessHandler handler, RequestMatcher preferredMatcher)` 用来构造默认的 `LogoutSuccessHandler` 我们可以通过添加多个来实现从不同 `URL` 退出执行不同的逻辑。
       - `LogoutSuccessHandler logoutSuccessHandler` 退出成功后执行的逻辑的抽象根本接口。

#### UserDetailsService

该接口只有一个方法==loadUserByUsername==需要实现

#### UserDetails

该接口提供了常用的用户信息，如果不够使用，可以进行扩展。默认有个实现类可以使用==User==

* 默认提供的属性和方法
  - 用户的权限集， 默认需要添加`ROLE_` 前缀
  - 用户的加密后的密码， 不加密会使用`{noop}`前缀
  - 应用内唯一的用户名
  - 账户是否过期
  - 账户是否锁定
  - 凭证是否过期
  - 用户是否可用



#### 路径Uri中的Ant风格

==?==代替任何一个字符；==*==代替0~任意字符；==**==代替任意目录

