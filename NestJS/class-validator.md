# Comprehensive Guide: class-validator in NestJS

## Table of Contents
1. Introduction
2. Installation
3. Basic Usage
4. Common Decorators
5. Custom Validation
6. Validation Groups
7. Conditional Validation
8. Validation Pipe
9. Error Messages and Internationalization
10. Best Practices
11. Advanced Topics

## 1. Introduction

class-validator is a powerful library used in NestJS for validating class instances and their properties. It uses decorators to define validation rules directly in your class definitions, making your code more readable and maintainable.

## 2. Installation

To get started, install the required packages:

```bash
npm install class-validator class-transformer
```

## 3. Basic Usage

Here's a basic example of how to use class-validator in a NestJS DTO (Data Transfer Object):

```typescript
import { IsString, IsInt, Min, Max } from 'class-validator';

export class CreateUserDto {
  @IsString()
  name: string;

  @IsInt()
  @Min(18)
  @Max(100)
  age: number;
}
```

## 4. Common Decorators

class-validator provides many built-in decorators for common validation scenarios:

- `@IsString()`, `@IsNumber()`, `@IsBoolean()`: Type validation
- `@Min()`, `@Max()`: Number range validation
- `@IsEmail()`: Email format validation
- `@IsOptional()`: Marks property as optional
- `@IsNotEmpty()`: Ensures property is not empty
- `@MinLength()`, `@MaxLength()`: String length validation
- `@IsDate()`: Date validation
- `@IsArray()`: Array validation
- `@IsEnum()`: Enum validation

## 5. Custom Validation

You can create custom validators for complex validation logic:

```typescript
import { registerDecorator, ValidationOptions, ValidatorConstraint, ValidatorConstraintInterface, ValidationArguments } from 'class-validator';

@ValidatorConstraint({ name: 'isPasswordStrong', async: false })
export class IsPasswordStrongConstraint implements ValidatorConstraintInterface {
  validate(password: string, args: ValidationArguments) {
    // Implement your password strength logic here
    const regex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)[a-zA-Z\d]{8,}$/;
    return regex.test(password);
  }

  defaultMessage(args: ValidationArguments) {
    return 'Password is not strong enough';
  }
}

export function IsPasswordStrong(validationOptions?: ValidationOptions) {
  return function (object: Object, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName: propertyName,
      options: validationOptions,
      constraints: [],
      validator: IsPasswordStrongConstraint,
    });
  };
}

// Usage
export class UserDto {
  @IsPasswordStrong()
  password: string;
}
```

## 6. Validation Groups

Validation groups allow you to specify which validators should run in different scenarios:

```typescript
import { IsString, IsNumber, IsOptional, Min, ValidateIf } from 'class-validator';

export class UserDto {
  @IsString({ groups: ['create', 'update'] })
  name: string;

  @IsNumber({}, { groups: ['create'] })
  @IsOptional({ groups: ['update'] })
  @Min(18, { groups: ['create', 'update'] })
  age: number;
}

// In your controller
@Post()
@UsePipes(new ValidationPipe({ groups: ['create'] }))
create(@Body() createUserDto: UserDto) {
  // ...
}

@Patch(':id')
@UsePipes(new ValidationPipe({ groups: ['update'] }))
update(@Param('id') id: string, @Body() updateUserDto: UserDto) {
  // ...
}
```

## 7. Conditional Validation

You can use `@ValidateIf` for conditional validation:

```typescript
export class UserDto {
  @IsString()
  userType: 'employee' | 'contractor';

  @ValidateIf(o => o.userType === 'employee')
  @IsString()
  employeeId: string;

  @ValidateIf(o => o.userType === 'contractor')
  @IsString()
  contractorId: string;
}
```

## 8. Validation Pipe

NestJS provides a built-in ValidationPipe that uses class-validator under the hood. You can use it globally or per-route:

```typescript
// Global usage (in main.ts)
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();

// Per-route usage
@Post()
@UsePipes(new ValidationPipe({ transform: true, whitelist: true }))
create(@Body() createUserDto: CreateUserDto) {
  // ...
}
```

## 9. Error Messages and Internationalization

You can customize error messages and even internationalize them:

```typescript
import { IsString, MinLength, ValidateIf } from 'class-validator';

export class UserDto {
  @IsString({ message: 'Name must be a string' })
  @MinLength(2, { message: 'Name must be at least 2 characters long' })
  name: string;

  // For internationalization
  @MinLength(2, {
    message: (args: ValidationArguments) => {
      return i18n.__('validation.minLength', { property: args.property, min: args.constraints[0] });
    }
  })
  internationalizedName: string;
}
```

## 10. Best Practices

1. Always validate input data, especially from external sources.
2. Use DTOs to define the shape and validation rules of your data.
3. Leverage built-in decorators when possible for common validations.
4. Create custom validators for complex business logic.
5. Use validation groups to reuse DTOs in different contexts.
6. Implement conditional validation when needed.
7. Customize error messages for better user experience.
8. Consider internationalization for applications supporting multiple languages.

## 11. Advanced Topics

### 11.1 Async Validators

For validations that require asynchronous operations (e.g., database queries):

```typescript
import { registerDecorator, ValidationOptions, ValidatorConstraint, ValidatorConstraintInterface, ValidationArguments } from 'class-validator';
import { Injectable } from '@nestjs/common';
import { UserService } from './user.service';

@Injectable()
@ValidatorConstraint({ name: 'isUserAlreadyExist', async: true })
export class IsUserAlreadyExistConstraint implements ValidatorConstraintInterface {
  constructor(private userService: UserService) {}

  async validate(email: string, args: ValidationArguments) {
    const user = await this.userService.findByEmail(email);
    return !user;
  }
}

export function IsUserAlreadyExist(validationOptions?: ValidationOptions) {
  return function (object: Object, propertyName: string) {
    registerDecorator({
      target: object.constructor,
      propertyName: propertyName,
      options: validationOptions,
      constraints: [],
      validator: IsUserAlreadyExistConstraint,
    });
  };
}

// Usage
export class CreateUserDto {
  @IsEmail()
  @IsUserAlreadyExist({ message: 'User already exists' })
  email: string;
}
```

### 11.2 Custom Validation Decorators

You can create your own decorators that combine multiple validations:

```typescript
import { applyDecorators } from '@nestjs/common';
import { IsString, MinLength, MaxLength } from 'class-validator';

export function IsName(validationOptions?: ValidationOptions) {
  return applyDecorators(
    IsString(validationOptions),
    MinLength(2, validationOptions),
    MaxLength(50, validationOptions),
  );
}

// Usage
export class UserDto {
  @IsName({ message: 'Invalid name' })
  name: string;
}
```

### 11.3 Validation with Transformations

You can use class-transformer along with class-validator to transform data before validation:

```typescript
import { IsDate, IsString } from 'class-validator';
import { Type } from 'class-transformer';

export class EventDto {
  @IsString()
  title: string;

  @Type(() => Date)
  @IsDate()
  date: Date;
}

// In your main.ts or controller
app.useGlobalPipes(new ValidationPipe({ transform: true }));
```

This guide covers the essential aspects of using class-validator in NestJS. Remember to refer to the official documentation for the most up-to-date information and additional features.
