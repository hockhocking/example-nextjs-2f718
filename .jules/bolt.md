## 2026-08-22 - Direct theme derivation vs useEffect state synchronization

**Learning:** Syncing theme-dependent visual props into `useState` via `useEffect` causes an extra state update and cascading re-render cycle on component mount or theme changes.
**Action:** Derive theme visual properties directly during render from `useTheme().resolvedTheme`.

## 2026-08-22 - Pre-instantiating Arcjet rule clients vs per-request instantiation

**Learning:** Calling `arcjet.withRule(...)` inside dynamic route handlers creates new Arcjet rule instances and client clones on every incoming HTTP request.
**Action:** Pre-instantiate static Arcjet rule clients at module scope outside handler functions and select the pre-instantiated client at request time.

## 2026-08-22 - Pre-instantiating Zod form resolvers vs per-render instantiation

**Learning:** Instantiating `zodResolver(schema)` inside React client form components creates new resolver closures on every render pass, causing unnecessary allocations and unstable resolver references in `useForm`.
**Action:** Pre-instantiate static `zodResolver(schema)` instances at module scope outside component render bodies.

## 2026-09-04 - Static className props and memoization for icon components vs inline array props

**Learning:** Passing inline array literals (`classes={["..."]}`) to unmemoized SVG icon components causes array allocations per render, string joining overhead (`.join(" ")`), and breaks referential equality, forcing icons to re-render whenever parent layouts update.
**Action:** Support string `className` props on SVG icon components, wrap them with `React.memo`, and pass static string class names from parent components.

## 2026-09-05 - Static regular expressions and test() vs inline match() in server components

**Learning:** Calling `hostname?.match(/.../)` inside Next.js Server Components / route handlers re-compiles regular expressions and allocates match result arrays on every incoming HTTP request.
**Action:** Pre-instantiate static RegExp instances at module scope outside component render bodies or route handlers, and use `RegExp.prototype.test()` instead of `match()` for boolean checks.

## 2026-09-06 - Inverted isDevelopment checks in IP resolution vs local fallback

**Learning:** Using `!isDevelopment(process.env)` instead of `isDevelopment(process.env)` in route handlers forces unnecessary `ip(req)` header parsing on local requests during development while hardcoding static local IPs in production.
**Action:** Always check `isDevelopment(process.env) ? "127.0.0.1" : ip(req)` to bypass IP parsing in dev and accurately identify client IPs in production.
