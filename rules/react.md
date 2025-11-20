# React Framework Review Rules

## Component Architecture
- Check for proper functional component usage
- Verify proper component composition
- Flag components with too many responsibilities
- Check for missing component memoization where needed

## Hooks
- Verify hooks are only called at top level
- Check for missing dependency arrays in `useEffect`
- Flag incorrect dependency arrays
- Verify proper cleanup in `useEffect` returns
- Check for missing `useCallback` on event handlers passed as props
- Flag unnecessary `useMemo` or `useCallback` usage

## State Management
- Check for prop drilling (6+ levels)
- Verify proper use of Context API
- Flag state that should be lifted up or pushed down
- Check for derived state that should be computed
- Verify proper state initialization

## Performance
- Flag missing `React.memo` for expensive components
- Check for inline function definitions in JSX
- Verify proper key props in lists
- Flag re-renders caused by object/array recreation
- Check for missing code splitting for large components

## Event Handlers
- Verify event handlers don't cause infinite loops
- Check for missing event.preventDefault() when needed
- Flag async operations without proper error handling
- Verify proper event delegation patterns

## Forms
- Check for controlled vs uncontrolled component consistency
- Verify form validation logic
- Flag missing form submission error handling
- Check for accessibility attributes

## Accessibility
- Check for missing ARIA labels
- Verify semantic HTML usage
- Flag keyboard navigation issues
- Check for missing alt text on images
- Verify proper heading hierarchy

## TypeScript (if applicable)
- Check for `any` types that should be properly typed
- Verify proper prop type definitions
- Flag missing generic types for reusable components
- Check for unsafe type assertions

## Side Effects
- Verify proper data fetching patterns
- Check for race conditions in async effects
- Flag missing loading and error states
- Verify proper cleanup of subscriptions

## Common Mistakes
- Modifying state directly instead of using setState
- Not handling async errors in effects
- Missing keys in list rendering
- Improper conditional rendering causing bugs
- Using index as key in dynamic lists
- Not cleaning up side effects (timers, subscriptions)
- Putting too much logic in components vs custom hooks

## Custom Hooks
- Verify hooks follow naming convention (`use` prefix)
- Check for proper hook composition
- Flag hooks with side effects that should be documented
- Verify proper return values from hooks
