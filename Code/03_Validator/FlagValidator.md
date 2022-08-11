>  **基于hibernate-validator框架的一个校验注解**，主要解决限定传某几个数值的问题。

* 使用：`@FlagValidator({"0","1","2"})`

```java
/**
 * 用户验证状态是否在指定范围内的注解
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD,ElementType.PARAMETER})
@Constraint(validatedBy = FlagValidatorClass.class)
public @interface FlagValidator {
    String[] value() default {};

    String message() default "flag is not found";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
```

