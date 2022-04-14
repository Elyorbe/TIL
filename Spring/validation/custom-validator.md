# Why custom validator?
Usually, when we deal with user input we need to validate the incoming requests. Spring MVC offers standard predefined validators.
However, for some situations predefined validators may not be enough and we may want to create one ourselves.

# IP address validator
For our learning purposes let's create an IP address validator. Initially we want to keep things simple:
IP should be written using the dot-decimal notation, so it should have 3 dots ("."), no characters, numbers in between the dots, and numbers should be in a valid range.

```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = IpAddressValidator.class)
@Documented
public @interface IpAddress {
    String message() default "Not valid IP address";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

We need to provide all of the following parameters:
- `message`, pointing to a property key in ValidationMessages.properties, which is used to resolve a message in case of violation,
- `groups`, allowing to define under which circumstances this validation is to be triggered ,
- `payload`, allowing to define a payload to be passed with this validation (I don't know how to use it yet, just adding to conform to the Spring standards.),
- `@Constraint` annotation pointing to an implementation of the ConstraintValidator interface.

Our requirement can be met using regex expressions but however it's better to stick to proven and well tested libraries if available.
`Google Guava` is an open-source set of common libraries for Java and it includes `InetAddressess` class for our use case. 
Of course it may be overkill to include whole library for only one validation but, in fact, `Google Guava` library is very rich in features
that can come in handy during the projet development.

The validator implementation looks like this:
```java
public class IpAddressValidator implements ConstraintValidator<IpAddress, String> {
  @Override
  public boolean isValid(String ip, ConstraintValidatorContext ctx) {
     return InetAddresses.isInetAddress(ip);
  }
}
```

It's quite simple at this point. We can just annotate our `ip` field and it does the job. For example, we have a `Device` class:
```java
public Device {
  ...
    
  @IpAddress
  private String ip;
  ...
}
```
Now, let's try to make our implementation a little bit more advanced. We give the option to validate only a specific version of IP.
Let's define IP version in enum type:
```java
public enum IpVersion {
    V4,
    V6,
    ANY
}
```
And add IpVersion to `IpAddress`. Below is the updated version:
```java
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = IpAddressValidator.class)
@Documented
public @interface IpAddress {
    String message() default "Not valid IP address";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
    IpVersion version() default IpVersion.ANY;
}
```

Next, update implementation class:
```java
public class IpAddressValidator implements ConstraintValidator<IpAddress, String> {
   private IpVersion version;

    @Override
    public void initialize(PublicIpv4Address ipValidator) {
        version = ipValidator.version();
    }

    @Override
    public boolean isValid(String ip, ConstraintValidatorContext ctx) {
        if(!InetAddresses.isInetAddress(ip)) // handles the case for dot decimal notation since InetAddress doesn't check for it.
            return false;
      
        try {
            InetAddress address = InetAddress.getByName(ip);
            switch (version) {
                case V4:
                    return address instanceof Inet4Address;
                case V6:
                    return address instanceof Inet6Address;
                case ANY:
                    return true;
            }
        } catch (UnknownHostException e) {
            return false;
        }
        return true;
    }
}
```

Lastly, updating our `Device` class to allow only IPv4 adresses:
```java
public Device {
  ... 
    
  @IpAddress(version = IpVersion.V4)
  private String ip;
  ...
}
```

# Conclusion
We learned how to create a custom IP validator to verify a field.
