---
title: redirect
description: API Reference for the redirect function.
---

The `redirect` function allows you to redirect the user to another URL. `redirect` can be used in Server Components, Client Components, [Route Handlers](/docs/app/building-your-application/routing/route-handlers), and [Server Actions](/docs/app/building-your-application/data-fetching/forms-and-mutations).

If a resource doesn't exist, you can use the [`notFound` function](/docs/app/api-reference/functions/not-found) instead.

## Parameters

The `redirect` function accepts two arguments:

```js
redirect(path, type)
```

| Parameter | Type                                                          | Description                                                 |
| --------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| `path`    | `string`                                                      | The URL to redirect to. Can be a relative or absolute path. |
| `type`    | `'replace'` (default) or `'push'` (default in Server Actions) | The type of redirect to perform.                            |

By default, `redirect` will use `push` (adding a new entry to the browser history stack) in [Server Actions](/docs/app/building-your-application/data-fetching/forms-and-mutations) and `replace` (replacing the current URL in the browser history stack) everywhere else. You can override this behavior by specifying the `type` parameter.

The `type` parameter has no effect when used in Server Components.

## Returns

`redirect` does not return any value.

## Example

Invoking the `redirect()` function throws a `NEXT_REDIRECT` error and terminates rendering of the route segment in which it was thrown.

```jsx filename="app/team/[id]/page.js"
import { redirect } from 'next/navigation'

async function fetchTeam(id) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }) {
  const team = await fetchTeam(params.id)
  if (!team) {
    redirect('/login')
  }

  // ...
}
```

> **Good to know**: `redirect` does not require you to use `return redirect()` as it uses the TypeScript [`never`](https://www.typescriptlang.org/docs/handbook/2/functions.html#never) type.

| Version   | Changes                |
| --------- | ---------------------- |
| `v13.0.0` | `redirect` introduced. |
