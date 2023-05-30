#### spel-eval
```sql
import org.springframework.core.DefaultParameterNameDiscoverer;
import org.springframework.expression.EvaluationContext;
import org.springframework.expression.Expression;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.expression.spel.support.StandardEvaluationContext;

import java.lang.reflect.Method;

public static String evalSPEl(Object[] args, Method method, String spel) {
        DefaultParameterNameDiscoverer discoverer = new DefaultParameterNameDiscoverer();
        String[] parameterNames = discoverer.getParameterNames(method);
        EvaluationContext context = new StandardEvaluationContext();
        for (int i = 0; i < parameterNames.length; i++) {
            context.setVariable(parameterNames[i], args[i]);
        }
        SpelExpressionParser parser = new SpelExpressionParser();
        Expression expression = parser.parseExpression(spel);
        Object result = expression.getValue(context);
        if (result == null) {
            return null;
        } else {
            return result.toString();
        }
    }
```

```sql
@DistributedLock(lockUniqueKey = "'" + Constants.XX + "_" + "' + "
            + "#xx.xx1 + '_' + #xx.xx2.xx3",
            lockTimeUnit = TimeUnit.MILLISECONDS, lockTime = 1000)
```