<h1 align="center">
<br><img src="https://dwglogo.com/wp-content/uploads/2017/12/Spring_Framework_logo_01.png" alt="MkDocs icon" width="170">
<br>Logging with AOP
</h1>

## Description

<p>
In order to be able to trace the code execution, I used slf4j (log4j implementation).
</p>

<!-- https://shields.io/ -->

## How it works

There are several ways to log the code
<dl>
<li>First, we can use standard tools such as LoggerFactory in order to obtain Logger object and invoke its methods.
</li>
E.g:

```java
Logger logger = LoggerFactory.getLogger(CustomClass.class);
```
<li>The next option available in my project is AOP-Logging. I have several pointcuts and aspects as follows:</li>

```java
@Around("@annotation(LogAnnotation)")
public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {

    long start = System.currentTimeMillis();
    Object proceed = joinPoint.proceed();
    long executionTime = System.currentTimeMillis() - start;
    
    // some business logic
        
    return proceed;
}
```

As one might have noticed, the pointcut is LogAnnotation. 
So, to make method produce logs we just need to annotate it with this annotation.
</dl>