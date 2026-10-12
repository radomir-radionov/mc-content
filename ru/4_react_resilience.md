# React — Resilience

**Graceful failure** in React apps — error boundaries, retries, reset state, and user-facing recovery flows.

[← Back to hub](./4_react.md)

### Related docs

- [React — Patterns](./4_react_patterns.md) · [React — Algorithms](./4_react_algorithms.md) · [React — Concurrent UI](./4_react_concurrent.md) · [React — Performance](./4_react_perf.md) · [Error boundaries in Core](./4_react_core.md) · [Next.js `error.tsx`](./4_nextjs_routing.md) · [Testing](./7_special_testing.md)

### Main concepts

| Concept                          | Covers                         |
| -------------------------------- | ------------------------------ |
| **Error boundaries**             | Catch render errors in subtree |
| **What boundaries cannot catch** | Events, async, SSR limits      |
| **Retry UI pattern**             | Reset boundary + refetch       |
| **Reset keys**                   | Remount subtree after failure  |
| **Route-level errors**           | Framework error pages          |
| **Offline & fallback**           | Degraded mode messaging        |

### Sections

- [Overview](#overview)
- [Error boundaries](#error-boundaries)
- [Recovery patterns](#recovery-patterns)

---

## Overview

<details>
  <summary>Overview</summary>

### Overview

**Definition:** **Resilience** means users see a **recoverable** failure (message + retry) instead of a blank screen when a component throws during render.

**How it works:** Error boundaries for render/lifecycle errors; explicit `error` state for fetch; framework route errors for segment failures.

**When to use:** Any production feature with async data and third-party widgets.

**Resilience stack (layered):**

| Layer            | Tool                               | Catches                          |
| ---------------- | ---------------------------------- | -------------------------------- |
| Widget           | Error boundary                     | Render throw in embed/chart      |
| Data fetch       | Query `error` + retry UI           | Network / 5xx                    |
| Route segment    | `error.tsx` / error boundary route | Page-level RSC failure           |
| Global           | `window.onerror` / monitoring      | Uncaught non-React errors        |

**Thoughts:** Users need **action** (retry, go home, contact support) — not only “Something went wrong”.

**See also:** [Concurrent — Errors in transitions](./4_react_concurrent.md#errors-in-transitions)

**Sources:**

[react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

Error boundaries

</details>

### Error boundaries

<details>
  <summary>Error boundaries</summary>

### Error boundaries

**Definition:** A class component (or library wrapper) implementing **`static getDerivedStateFromError`** / **`componentDidCatch`** that renders **fallback UI** when a child throws during render.

**How it works:**

- Place around risky subtrees (charts, markdown, plugin slots)
- Log `error` + `info.componentStack` in `componentDidCatch`
- Does **not** catch event handlers, async code, or Server Component errors on server (use route `error.js`)

**When to use:** Isolate third-party SDK crashes from whole app shell.

**Where to place boundaries:**

| Layer              | Catches                        |
| ------------------ | ------------------------------ |
| Around chart/embed | SDK render throw               |
| Route segment      | Page-level failure (framework) |
| Root (sparingly)   | Last resort white-screen guard |

**Pluses and minuses:**

| +                     | −                               |
| --------------------- | ------------------------------- |
| Isolates blast radius | Class API or extra dependency   |
| Better UX than blank  | Does not fix event/async errors |

**Examples:**

```tsx
class ChartBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean }
> {
  state = { hasError: false };
  static getDerivedStateFromError() {
    return { hasError: true };
  }
  render() {
    if (this.state.hasError) return <p>Chart failed to load.</p>;
    return this.props.children;
  }
}
```

**See also:** [Profiler](./4_react_components.md#profiler)

**Sources:**

[react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

What boundaries cannot catch

</details>

<details>
  <summary>What boundaries cannot catch</summary>

### What boundaries cannot catch

**Definition:** Boundaries only catch errors in **render**, **lifecycle**, and **constructors** of descendants — not everywhere.

**How it works:**

| Not caught                       | Handle instead                          |
| -------------------------------- | --------------------------------------- |
| Event handler `onClick` throws   | `try/catch` in handler + local error UI |
| `setTimeout` / Promise rejection | `.catch` / `async` try/catch + state    |
| Server render throw (RSC)        | Framework error segment / `error.tsx`   |
| Errors in boundary itself        | Parent boundary or root handler         |

**When to use:** Debugging “boundary did not fire” — check if throw was in effect/event.

**Interview angle:** “Boundaries are like `try/catch` for **render** — not for events or async. For fetch errors use error state or Query.”

**Examples (event handler — boundary useless):**

```tsx
function BadButton() {
  return (
    <button
      type="button"
      onClick={() => {
        throw new Error("click"); // not caught by boundary
      }}
    >
      Save
    </button>
  );
}
// Fix: try/catch in handler → setLocalError(...)
```

**See also:** [Retry UI pattern](#retry-ui-pattern)

**Sources:**

[react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)

Recovery patterns

</details>

### Recovery patterns

<details>
  <summary>Retry UI pattern</summary>

### Retry UI pattern

**Definition:** Offer **Try again** that clears error state and **re-runs fetch** or **remounts** the failed subtree.

**How it works:**

1. Store `error` + `retryCount` in boundary state or query library
2. Button calls `resetErrorBoundary()` (library) or `setState({ hasError: false })` + invalidate query
3. Optionally exponential backoff for API retries (not for render retries)

**When to use:** Transient network failures; flaky embeds.

**Examples (react-error-boundary style):**

```tsx
function DataPanel() {
  const { data, error, refetch, isFetching } = useQuery({ queryKey: ["stats"], queryFn: fetchStats });
  if (error) {
    return (
      <div role="alert">
        <p>Could not load stats.</p>
        <button type="button" onClick={() => refetch()} disabled={isFetching}>
          Try again
        </button>
      </div>
    );
  }
  return <Chart data={data} />;
}
```

**Pluses and minuses:**

| +                    | −                              |
| -------------------- | ------------------------------ |
| Clear recovery path  | Retry storms without backoff   |
| Works with Query     | Render errors need boundary    |

**See also:** [Fetch with abort](./4_react_algorithms.md#fetch-with-abort) · [TanStack Query retry](./4_react_tanstack.md)

**Sources:**

[react.dev/learn/synchronizing-with-effects](https://react.dev/learn/synchronizing-with-effects)

Reset keys

</details>

<details>
  <summary>Reset keys</summary>

### Reset keys

**Definition:** Change React **`key`** on a subtree to **remount** clean state after error or “start over” — discards broken internal state.

**How it works:**

```tsx
<Wizard key={attemptId} />
```

Increment `attemptId` on “Reset form”. Same pattern as resetting uncontrolled inputs without full page reload.

**When to use:** Multi-step wizard corrupted state; failed file upload widget.

**How it works with boundaries:** Combine `key={resetKey}` on subtree with boundary `resetKeys={[resetKey]}` (library) so both internal state and error flag clear on “Start over”.

**Examples:**

```tsx
const [attempt, setAttempt] = useState(0);
return (
  <>
    <CheckoutWizard key={attempt} />
    <button type="button" onClick={() => setAttempt((a) => a + 1)}>
      Start over
    </button>
  </>
);
```

**See also:** [Wizard step algorithm](./4_react_algorithms.md#wizard-step-algorithm) · [Preserving state](https://react.dev/learn/preserving-and-resetting-state)

**Sources:**

[react.dev/learn/preserving-and-resetting-state](https://react.dev/learn/preserving-and-resetting-state)

Route-level errors

</details>

<details>
  <summary>Route-level errors</summary>

### Route-level errors

**Definition:** Framework **error routes** (`error.tsx` in Next.js App Router) catch segment errors and show UI without crashing parent layouts.

**How it works:** Place `error.tsx` next to `page.tsx` for a route segment; export client component with `reset()` prop from framework.

**When to use:** Server Components throw, data loader fails for one page — keep nav shell.

**Examples (App Router pattern):**

```tsx
"use client";

export default function Error({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong</h2>
      <button type="button" onClick={() => reset()}>Try again</button>
    </div>
  );
}
```

**Thoughts:** Log `error.digest` / monitoring in production; `reset()` re-renders the segment — pair with refetch if data was the cause.

**See also:** [Next.js — Routing](./4_nextjs_routing.md) · [Retry UI pattern](#retry-ui-pattern)

**Sources:**

[nextjs.org/docs/app/building-your-application/routing/error-handling](https://nextjs.org/docs/app/building-your-application/routing/error-handling)

Offline & fallback

</details>

<details>
  <summary>Offline & fallback</summary>

### Offline & fallback

**Definition:** Detect **`navigator.onLine`** or failed fetch and show **cached/degraded** UI instead of infinite spinners.

**How it works:** `useOnlineStatus` hook + Query `networkMode`; show banner “You’re offline — showing saved data”.

**When to use:** Mobile PWA-ish apps, field tools with spotty network.

**How it works:**

1. `useOnlineStatus()` + banner when `!online`
2. Query `networkMode: 'offlineFirst'` or show last cached page from persistence
3. Disable mutations that cannot sync; queue with background sync only when product requires it

**Pluses and minuses:**

| +                       | −                           |
| ----------------------- | --------------------------- |
| Honest UX vs spinner    | Stale data must be labeled  |
| Works with Query cache  | `online` events are imperfect |

**Examples:**

```tsx
function OfflineBanner() {
  const online = useOnlineStatus();
  if (online) return null;
  return <p role="status">You are offline — showing saved data.</p>;
}
```

**See also:** [Custom hooks pattern](./4_react_patterns.md#custom-hooks-pattern) · [Optimistic updates](./4_react_algorithms.md#optimistic-updates)

**Sources:**

[MDN — online event](https://developer.mozilla.org/en-US/docs/Web/API/Window/online_event)

</details>
