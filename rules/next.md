# Next.js Framework Review Rules

## Architecture & Structure
- Ensure proper use of App Router vs Pages Router
- Check for correct file structure (`app/` or `pages/` directory)
- Verify proper usage of server vs client components
- Check for `"use client"` directive placement

## Performance
- Flag missing `loading.tsx` or `error.tsx` files for route segments
- Check for improper data fetching patterns (avoid waterfall requests)
- Verify proper use of `generateStaticParams` for dynamic routes
- Check for missing image optimization (use `next/image`)
- Flag blocking operations in Server Components

## Data Fetching
- Ensure fetch requests use proper caching strategies
- Check for `fetch()` with `cache: 'no-store'` when dynamic data is needed
- Verify `revalidate` options are set appropriately
- Flag client-side data fetching that should be server-side

## Routing
- Check for proper dynamic route naming (`[slug]`, `[...slug]`)
- Verify route groups are used correctly (`(group)`)
- Check for parallel routes setup (`@slot`)
- Flag incorrect usage of redirects and not-found pages

## Metadata & SEO
- Check for missing `metadata` exports in pages
- Verify proper OpenGraph and Twitter card setup
- Flag missing alt text on images

## Server Components (RSC)
- Flag async operations in Client Components
- Check for serialization issues (passing non-serializable props)
- Verify hooks are only used in Client Components
- Flag improper use of browser APIs in Server Components

## Client Components
- Check for unnecessary `"use client"` directives
- Verify event handlers are in Client Components
- Flag state management in wrong component type

## API Routes & Server Actions
- Check for proper error handling in route handlers
- Verify server actions have proper form validation
- Flag exposed sensitive data in responses
- Check for missing authentication/authorization

## Common Mistakes
- Importing Client Component features in Server Components
- Missing error boundaries
- Improper use of `useRouter` vs `redirect()`
- Not using streaming for slow operations
- Mixing Pages Router and App Router patterns
