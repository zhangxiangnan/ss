#### 切面示例
##### 定义注解
```sql
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface DistributedLockAnnotation {

    String lockUniqueKey();

    int lockTime();

    TimeUnit lockTimeUnit();
}
```
##### 定义切面
```sql
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.aspectj.lang.reflect.MethodSignature;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;

// 切面定义
@Aspect
@Component
public class DistributedLockAspect {

    @Autowired
    private DistributedLock distributedLock;

    // 切入点
    @Pointcut("@annotation(xx.xx.DistributedLockAnnotation)")
    public void logPointCut() {
    }

    // 环绕通知
    @Around("logPointCut()")
    public Object around(ProceedingJoinPoint point) throws Throwable {
        Object result;
        try {
            boolean locked = tryGetDistributeLock(point);
            if (!locked) {
                throw new RunTimeException("xx");
            }
            result = point.proceed();
        } catch (Throwable e) {
            throw e;
        }
        return result;
    }

    private boolean tryGetDistributeLock(ProceedingJoinPoint point) {
        try {
            MethodSignature signature = (MethodSignature) point.getSignature();
            Method method = signature.getMethod();
            DistributedLock annotation = method.getAnnotation(DistributedLockAnnotation.class);
            String lockKey = SPELUtils.evalSPEl(point.getArgs(), method, annotation.uniqueKey());
            if (StringUtils.isBlank(lockKey)) {
                log.warn("xx, lockKey = " + lockKey);
                return false;
            }
            boolean locked = distributedLock.tryLock(lockKey, annotation.unit(), annotation.leaseTime());
            if (!locked) {
                log.warn("xx,, lockKey = " + lockKey + ", locked = " + locked);
            }
            return locked;
        } catch (Exception e) {
            log.error("", e);
        }
        return true;
    }
}
```