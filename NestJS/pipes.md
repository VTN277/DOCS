# Comprehensive Guide to NestJS Pipes

## Introduction

NestJS pipes are a powerful feature of the NestJS framework that allow you to transform or validate input data before it reaches your route handlers. They play a crucial role in ensuring data integrity and can significantly simplify your application logic.

## What are Pipes?

Pipes in NestJS are classes annotated with the `@Injectable()` decorator. They implement the `PipeTransform` interface, which requires a `transform()` method. This method takes the input value and optional metadata and returns the transformed value.

## Built-in Pipes

NestJS provides several built-in pipes:

1. `ValidationPipe`
2. `ParseIntPipe`
3. `ParseFloatPipe`
4. `ParseBoolPipe`
5. `ParseArrayPipe`
6. `ParseUUIDPipe`
7. `ParseEnumPipe`
8. `DefaultValuePipe`

## Creating Custom Pipes

To create a custom pipe, you need to:

1. Create a class that implements the `PipeTransform` interface
2. Implement the `transform()` method
3. Apply the `@Injectable()` decorator

Example:

```typescript
import { Injectable, PipeTransform, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class CustomValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    if (!value) {
      throw new BadRequestException('Value is required');
    }
    return value;
  }
}
```

## Using Pipes

Pipes can be used at different levels:

1. Handler-level pipes
2. Parameter-level pipes
3. Global pipes

### Handler-level Pipes

```typescript
@Post()
@UsePipes(new ValidationPipe())
create(@Body() createUserDto: CreateUserDto) {
  // ...
}
```

### Parameter-level Pipes

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  // ...
}
```

### Global Pipes

In your `main.ts` file:

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

## Validation Pipe

The `ValidationPipe` is one of the most commonly used pipes. It leverages the `class-validator` and `class-transformer` libraries to perform validation based on decorators in your DTO classes.

Example DTO:

```typescript
import { IsString, IsInt, Min, Max } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsInt()
  @Min(0)
  @Max(120)
  age: number;
}
```

## Transformation Pipes

Pipes can also transform input data. For example, the `ParseIntPipe` transforms string input into integers:

```typescript
@Get(':id')
findOne(@Param('id', ParseIntPipe) id: number) {
  // id is guaranteed to be a number
}
```

## Best Practices

1. Use pipes for input validation and transformation
2. Implement custom pipes for specific business logic
3. Use global pipes for application-wide concerns
4. Combine pipes with DTOs for robust input validation
5. Handle exceptions thrown by pipes appropriately

## Conclusion

NestJS pipes are a powerful tool for ensuring data integrity and simplifying your application logic. By leveraging both built-in and custom pipes, you can create more robust and maintainable applications.

