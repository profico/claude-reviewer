# NestJS Framework Review Rules

## Architecture & Structure
- Ensure proper module organization (feature modules)
- Check for circular dependencies between modules
- Verify proper use of `@Module()` decorators
- Flag improper dependency injection patterns

## Dependency Injection
- Check for missing `@Injectable()` decorators on services
- Verify providers are properly registered in modules
- Flag constructor injection anti-patterns
- Check for improper scope usage (DEFAULT, REQUEST, TRANSIENT)

## Controllers & Routes
- Verify proper HTTP method decorators (`@Get()`, `@Post()`, etc.)
- Check for missing DTOs and validation
- Flag routes without proper guards or interceptors
- Verify proper status code usage

## Guards & Middleware
- Check for missing authentication guards on protected routes
- Verify guard order (authentication before authorization)
- Flag improper exception handling in guards
- Check for missing role-based access control

## Pipes & Validation
- Check for missing `class-validator` decorators in DTOs
- Verify `ValidationPipe` is enabled globally or per-route
- Flag improper transformation logic in pipes
- Check for missing custom validation decorators

## Exception Handling
- Verify proper use of built-in HTTP exceptions
- Check for missing global exception filters
- Flag uncaught promise rejections
- Verify proper error message formatting

## Database & TypeORM/Prisma
- Check for missing transactions on multi-step operations
- Flag N+1 query problems
- Verify proper entity relationships
- Check for missing indexes on frequently queried fields
- Flag raw queries without proper sanitization

## Services & Business Logic
- Check for controllers with business logic (should be in services)
- Verify proper separation of concerns
- Flag services with database-specific code
- Check for missing error handling

## Configuration
- Verify proper use of `ConfigModule` for environment variables
- Check for hardcoded credentials or secrets
- Flag missing validation for configuration values
- Verify proper type safety for config values

## Testing
- Check for missing unit tests for services
- Verify e2e tests for critical endpoints
- Flag missing mocks for external dependencies

## Performance
- Flag synchronous operations that should be async
- Check for missing caching strategies
- Verify proper use of `@nestjs/cache-manager`
- Flag blocking operations in request handlers

## Common Mistakes
- Not using DTOs for request/response
- Missing proper error handling in async operations
- Improper module imports (importing entire modules when only service needed)
- Not using proper TypeScript types
- Missing validation on user input
