# An In-Depth Guide to NestJS Interceptors

## Introduction

NestJS interceptors are a powerful feature in the NestJS framework that allow you to add extra logic before and after the execution of the main handler. They are inspired by the Aspect-Oriented Programming (AOP) technique and can be used for various purposes such as:

- Binding extra logic before/after method execution
- Transforming the result returned from a function
- Transforming the exception thrown from a function
- Extending the basic function behavior
- Completely overriding a function depending on specific conditions

## How Interceptors Work

Interceptors have access to the `ExecutionContext` instance, which provides additional details about the current execution process. The interceptor's logic is implemented in the `intercept()` method, which takes two arguments:

1. `ExecutionContext`: Inherits from `ArgumentsHost` and provides additional details about the current execution process.
2. `CallHandler`: An interface with the `handle()` method, which you can use to invoke the route handler method at some point in your interceptor.

## Creating an Interceptor

To create an interceptor, you need to:

1. Create a class that implements the `NestInterceptor` interface
2. Implement the `intercept()` method
3. Use the `@Injectable()` decorator to make it injectable

Here's a basic example:

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    const now = Date.now();
    return next
      .handle()
      .pipe(
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}
```

## Using Interceptors

You can use interceptors at different levels:

1. Controller-level: Apply to all routes in a controller
2. Method-level: Apply to a specific route handler
3. Global-level: Apply to every registered route

### Controller-level

```typescript
@UseInterceptors(LoggingInterceptor)
export class CatsController {}
```

### Method-level

```typescript
@UseInterceptors(LoggingInterceptor)
@Get()
findAll() {
  return this.catsService.findAll();
}
```

### Global-level

```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalInterceptors(new LoggingInterceptor());
```

## Advanced Interceptor Techniques

### Response Mapping

You can use interceptors to transform the response from your route handlers:

```typescript
@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data, timestamp: new Date().toISOString() })));
  }
}
```

### Exception Mapping

Interceptors can also catch and handle exceptions:

```typescript
@Injectable()
export class ErrorsInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(
        catchError(err => throwError(() => new BadGatewayException())),
      );
  }
}
```

### Stream Overriding

For scenarios where you need to completely override the response stream:

```typescript
@Injectable()
export class CacheInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const isCached = true;
    if (isCached) {
      return of([{ cachedData: 'from cache' }]);
    }
    return next.handle();
  }
}
```

## Best Practices

1. Keep interceptors focused on a single responsibility
2. Use dependency injection to make interceptors more flexible and testable
3. Consider performance implications, especially for global interceptors
4. Use appropriate scope (request, transient, singleton) based on your use case
5. Combine with other NestJS features like guards and pipes for comprehensive request processing

## Conclusion

NestJS interceptors provide a powerful way to add cross-cutting concerns to your application. They can be used for logging, error handling, response transformation, and much more. By understanding and effectively using interceptors, you can create more maintainable and feature-rich NestJS applications.
