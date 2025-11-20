# Generic Code Review Rules

## Code Quality
- Check for proper error handling
- Verify input validation
- Flag potential null/undefined reference errors
- Check for proper type safety
- Verify proper logging practices

## Security
- Flag hardcoded credentials or API keys
- Check for SQL injection vulnerabilities
- Verify proper input sanitization
- Flag XSS vulnerabilities
- Check for insecure dependencies

## Performance
- Flag inefficient algorithms (O(n²) where O(n) possible)
- Check for memory leaks
- Verify proper resource cleanup
- Flag blocking operations
- Check for unnecessary computations

## Best Practices
- Verify proper naming conventions
- Check for code duplication (DRY principle)
- Flag overly complex functions
- Verify proper separation of concerns
- Check for magic numbers/strings

## Testing
- Flag missing error case handling
- Check for untested edge cases
- Verify proper test coverage for critical paths

## Common Issues
- Race conditions
- Off-by-one errors
- Division by zero
- Buffer overflows
- Improper error handling
- Resource leaks
