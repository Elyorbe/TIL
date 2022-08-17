## Testing only web layer

To test that Spring Webflux controllers are working as expected, use the `@WebFluxTest` annotation.
It auto-configures only Spring Webflux infrastructure and scannes only
`@Controller`, `@ControllerAdvice`, `@JsonComponent`, `Converter`, `GenericConverter`, `WebFilter`, and `WebFluxConfigurer` beans.
Regular `@Component` and `@ConfigurationProperties` beans are not scanned when `@WebFluxTest` annotation is used. In other words,
whole Spring application is not loaded.

Use `@Import` to register extra components 
