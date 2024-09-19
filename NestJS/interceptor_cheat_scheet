# NestJS Interceptor Cheat Sheet

## What are Interceptors?

Interceptors are a powerful feature in NestJS that allow you to:
- Bind extra logic before/after method execution
- Transform the result returned from a function
- Transform the exception thrown from a function
- Extend the basic function behavior
- Completely override a function depending on specific conditions

## Basic Structure

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

## Key Concepts

1. **ExecutionContext**: Provides details about the current execution process.
2. **CallHandler**: Represents the next interceptor in the chain, or the route handler if it's the last interceptor.

## Common Use Cases

### 1. Logging

```typescript
@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');
    return next
      .handle()
      .pipe(
        tap(() => console.log('After...')),
      );
  }
}
```

### 2. Transforming Response

```typescript
@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data, timestamp: new Date() })));
  }
}
```

### 3. Exception Mapping

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

### 4. Cache Interceptor

```typescript
@Injectable()
export class CacheInterceptor implements NestInterceptor {
  constructor(private cacheManager: Cache) {}

  async intercept(context: ExecutionContext, next: CallHandler): Promise<Observable<any>> {
    const key = this.getKey(context);
    const value = await this.cacheManager.get(key);
    
    if (value) {
      return of(value);
    }
    
    return next.handle().pipe(
      tap(response => this.cacheManager.set(key, response, { ttl: 60 }))
    );
  }

  private getKey(context: ExecutionContext): string {
    const request = context.switchToHttp().getRequest();
    return `${request.url}`;
  }
}
```

## How to Use

### Global Interceptor

```typescript
import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';
import { LoggingInterceptor } from './logging.interceptor';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: LoggingInterceptor,
    },
  ],
})
export class AppModule {}
```

### Controller-level Interceptor

```typescript
@UseInterceptors(LoggingInterceptor)
@Controller('cats')
export class CatsController {}
```

### Method-level Interceptor

```typescript
@UseInterceptors(LoggingInterceptor)
@Get()
findAll() {
  return this.catsService.findAll();
}
```

### Passing Arguments

```typescript
@UseInterceptors(new TransformInterceptor(new ValidationPipe()))
@Get()
findAll() {
  return this.catsService.findAll();
}
```

Remember, interceptors can be incredibly powerful when used correctly, but they can also add complexity to your application. Use them judiciously and always consider the impact on performance and maintainability.
