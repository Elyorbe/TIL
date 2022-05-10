I noticed `doFilterInternal` method was being executed twice per request. This is not efficient and not expected behaviour.
I dived into solving the problem and this is what I learned.

In Spring applications we can register beans using various annotations like `@Component, @Controller, @Service` and etc.
We can register a filter class by putting `@Component` annotation on top of it.

Spring Security offers another way to register filters through configuration.
```java
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    
  @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .anyRequest().authenticated()
            .and()
            .addFilterBefore(new JwtAuthenticationFilter(jwtTokenProvider), UsernamePasswordAuthenticationFilter.class);
    }
}

```
In addition to above configuration, I had a filter class with `@Component` annotation on top of it.
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain)
            throws ServletException, IOException {
        //logic here
        chain.doFilter(request, response);
    }
}
```

That `@Component` was the headache. It was causing the `JwtAuthenticationFilter` class being registered for the second time which was resulting in the filter
being executed twice. However, once we register our filters using `addFilterBefore` method in the configuration class there is no need to annotate a filter class 
with `@Component`. Removing `@Component` annotation from `JwtAuthenticationFilter` solved the issue.
