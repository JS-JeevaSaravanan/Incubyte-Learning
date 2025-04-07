
Here's a crisp breakdown of **Vitest timeout configs**:

---

### 🔧 Set Global Timeout

Set default timeout for all tests:

**`vitest.config.ts`**

```ts
export default defineConfig({
  test: {
    timeout: 5000, // 5 seconds
  },
});
```

---

### ⏱ Set Timeout Per Test

Use `test.timeout(ms)` or pass as third arg:

```ts
test('long operation', async () => {
  // your test
}, 10000); // 10 seconds
```

Or:

```ts
test.timeout(10000);
```

---

### 🧪 Hook Timeout

You can also set timeouts for hooks like `beforeEach`, `afterEach`, etc.:

```ts
beforeEach(async () => {
  // setup code
}, 3000);
```

---

### 💡 Tips

- Default timeout is **5 seconds (5000ms)**.
    
- Useful for **slow API calls**, **file I/O**, or **integration tests**.
    

---

Let me know if you want a reusable utility or template for applying timeouts smartly across your project.

---

Ah, got it — you're asking about **DB timeouts** (in the context of Vitest tests). Here's a crisp overview, assuming you're testing a database (e.g., PostgreSQL, MongoDB, etc.):

---

### 🔧 1. **Vitest Timeout (Test-Level)**

You might need to **increase test timeouts** if DB queries take time:

```ts
test('fetch from DB', async () => {
  // DB logic
}, 10000); // 10s timeout
```

Or globally in `vitest.config.ts`:

```ts
test: {
  timeout: 10000
}
```

---

### 🛠 2. **Database Client Timeout**

Depends on the DB client you're using.

#### 📦 PostgreSQL (with `pg`)

```ts
const client = new Client({
  connectionString: process.env.DATABASE_URL,
  statement_timeout: 5000, // 5s timeout per query
});
```

#### 📦 Prisma

```ts
// schema.prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  timeout  = 5000 // milliseconds (for some DBs)
}
```

Or use it programmatically:

```ts
await prisma.$queryRaw`SET statement_timeout = 5000`;
```

#### 📦 Mongoose (MongoDB)

```ts
mongoose.connect(MONGO_URI, {
  serverSelectionTimeoutMS: 5000, // timeout to connect
  socketTimeoutMS: 10000, // timeout for queries
});
```

---

### 🧪 3. **Don't Forget: Cleanup**

In `afterEach`, make sure to close DB connections to avoid hanging tests:

```ts
afterEach(async () => {
  await db.disconnect();
});
```

---

Want a snippet tailored to your DB stack? Just tell me what you're using (Prisma, Mongoose, etc.).


---

In **Vitest**, `advanceTimersByTimeAsync` is used with **fake timers** to simulate the passage of time in asynchronous code—especially when you have code that uses things like `setTimeout`, `setInterval`, or other timer-based APIs.

---

### 🔧 What is `advanceTimersByTimeAsync`?

```ts
vi.advanceTimersByTimeAsync(ms: number): Promise<void>
```

It **advances the virtual timer by `ms` milliseconds**, and **waits for any resulting asynchronous callbacks** (like `Promise`s that might resolve within that timer).

---

### ✅ When to Use It

You use it when:

- You've mocked timers using `vi.useFakeTimers()`.
    
- You have async logic that depends on timers (`setTimeout`, `setInterval`, etc.).
    
- You want to simulate the passage of time **accurately**, including allowing any `Promise`-based logic to resolve.
    

---

### 🧠 Difference Between `advanceTimersByTime` and `advanceTimersByTimeAsync`

|Method|Description|
|---|---|
|`advanceTimersByTime(ms)`|Advances timers synchronously. Ignores pending promises.|
|`advanceTimersByTimeAsync(ms)`|Advances timers **and waits** for any resulting asynchronous tasks to finish.|

---

### 📦 Example

```ts
import { describe, it, expect, vi } from 'vitest'

function waitWithTimeout(callback: () => void) {
  setTimeout(() => {
    callback()
  }, 1000)
}

describe('advanceTimersByTimeAsync', () => {
  it('should call the callback after 1 second', async () => {
    vi.useFakeTimers()

    const cb = vi.fn()
    waitWithTimeout(cb)

    // Without advancing the timer, cb is not called yet
    expect(cb).not.toHaveBeenCalled()

    // Advance the time by 1000ms
    await vi.advanceTimersByTimeAsync(1000)

    expect(cb).toHaveBeenCalled()
  })
})
```

---

Let me know if you want an example with `setInterval`, debounce, or something like `async/await` + `setTimeout` combo.


---

Absolutely — here’s a **crisp real-world example** using `vi.advanceTimersByTimeAsync` in the context of a **debounce function**, like you'd use in a search input:

---

### 🧠 Real-World Example: Debounced Search

Say you have a search bar that only calls the API **1 second after** the user stops typing.

```ts
function debouncedSearch(callback: () => void) {
  let timeout: ReturnType<typeof setTimeout>
  return () => {
    clearTimeout(timeout)
    timeout = setTimeout(callback, 1000) // delay by 1 second
  }
}
```

---

### ✅ Test with `vi.advanceTimersByTimeAsync`

```ts
import { describe, it, expect, vi } from 'vitest'

describe('debouncedSearch', () => {
  it('calls the API after 1 second of inactivity', async () => {
    vi.useFakeTimers()
    const apiCall = vi.fn()

    const search = debouncedSearch(apiCall)

    search() // user types
    search() // user types again quickly (resets timer)

    await vi.advanceTimersByTimeAsync(1000)

    expect(apiCall).toHaveBeenCalledTimes(1)
  })
})
```

---

### 🔍 What’s Happening

- `vi.useFakeTimers()` lets you control time.
    
- User types quickly twice → debounce timer resets.
    
- `advanceTimersByTimeAsync(1000)` simulates the **1-second wait**.
    
- Ensures `apiCall()` is only made once and **after** that delay.
    

---

Let me know if you want one for `setInterval`, animations, or retries 👌


Great question! Yes — **it's a good practice** to call `vi.useRealTimers()` at the end of the test (or in `afterEach`) when you've used `vi.useFakeTimers()`.

---

### ✅ Why?

Because fake timers **override global timer functions**, like `setTimeout`, `setInterval`, `clearTimeout`, etc.  
If you don’t restore them, it can cause:

- Other tests to behave incorrectly (especially ones relying on real timers),
    
- Confusing bugs that are hard to trace.
    

---

### 🔧 Where to Put It?

You can add it in your test directly:

```ts
afterEach(() => {
  vi.useRealTimers()
})
```

Or inline if you only use fake timers in a single test:

```ts
it('does something with timers', async () => {
  vi.useFakeTimers()
  // ... test logic
  vi.useRealTimers()
})
```

---

### 🧠 TL;DR

- ✅ **Yes**, always reset to `useRealTimers()` when done.
    
- 🛡️ It protects other tests from leaking mocked timer behavior.
    
- 🧼 Use `afterEach` for clean, repeatable tests.
    

Let me know if you want a Vitest setup file or `beforeEach/afterEach` template!


---


Absolutely! Let's break down `vi.runOnlyPendingTimersAsync()` with a real-world example — crisp and clear.

---

### 🧠 What is `runOnlyPendingTimersAsync()`?

```ts
await vi.runOnlyPendingTimersAsync()
```

It **runs only the currently scheduled timers**, without advancing time like `advanceTimersByTimeAsync` does.

This is useful when you **want to immediately flush any queued timeouts/intervals** without affecting future ones or simulating the passage of time.

---

### 🔧 Real-World Example: Retry with Delay

Suppose you have a function that retries an API request after a 2-second delay:

```ts
function fetchWithRetry(fetchFn: () => Promise<any>, retries = 1): Promise<any> {
  return fetchFn().catch(err => {
    if (retries <= 0) throw err
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve(fetchWithRetry(fetchFn, retries - 1))
      }, 2000)
    })
  })
}
```

---

### ✅ Test with `runOnlyPendingTimersAsync`

```ts
import { describe, it, expect, vi } from 'vitest'

describe('fetchWithRetry', () => {
  it('retries once after failure', async () => {
    vi.useFakeTimers()

    const fetchMock = vi.fn()
      .mockRejectedValueOnce(new Error('fail'))
      .mockResolvedValueOnce('success')

    const promise = fetchWithRetry(fetchMock)

    // Fast-forward to trigger the pending retry timer (but not all time)
    await vi.runOnlyPendingTimersAsync()

    const result = await promise

    expect(result).toBe('success')
    expect(fetchMock).toHaveBeenCalledTimes(2)

    vi.useRealTimers()
  })
})
```

---

### 🔍 What’s Happening?

- `fetchMock` fails once, then succeeds.
    
- Retry is **scheduled with `setTimeout(..., 2000)`**.
    
- Instead of waiting or advancing time, we **instantly flush** that retry with `runOnlyPendingTimersAsync()`.
    
- No unnecessary time travel — just immediate execution of **pending timers**.
    

---

### 🧠 TL;DR

|Method|Behavior|
|---|---|
|`advanceTimersByTimeAsync(ms)`|Simulates passage of time by `ms`, executes timers due in that time|
|`runOnlyPendingTimersAsync()`|Executes currently pending timers **without** simulating time|

---

Let me know if you want to compare it with `runAllTimersAsync` too — which runs **all** timers including intervals!



---

Great question! Let’s break down **Bun’s test runner** (`bun test`) vs **Vitest**, so you can choose the best fit for your workflow.

---

## ⚔️ **Bun Test vs Vitest**

|Feature|`bun test`|`Vitest`|
|---|---|---|
|✅ **Speed**|Extremely fast (native binary)|Very fast (Vite-powered, still JS-based)|
|🧪 **Test Syntax**|Simple, minimal|Rich, Jest-like (describe/it/expect)|
|🧰 **Mocking & Spying**|Basic (limited)|Powerful (`vi.fn()`, `vi.spyOn()`, mocks)|
|🔬 **Fake Timers**|Experimental|Fully supported (`vi.useFakeTimers()`, etc)|
|🧱 **Ecosystem & Plugins**|Minimal|Extensive (coverage, reporters, UI, etc.)|
|💻 **Dev Experience**|Fast, but barebones|Excellent DX with Vite integration|
|📦 **Framework Integration**|Barely any (yet)|Deep support (especially with Vue, React)|
|🧪 **Test UI / Watch Mode**|Basic CLI|Great watch mode, UI viewer (with plugins)|
|🧪 **TypeScript Support**|Native|Native (with Vite)|

---

### 🧠 TL;DR - Which One to Use?

#### 🚀 Choose **`bun test`** if:

- You want **blazing-fast test runs**.
    
- Your needs are **simple**: unit tests, assertions, no mocks/fake timers.
    
- You're already using Bun for your runtime.
    
- You want minimal config and fast feedback.
    

#### 🧪 Choose **`Vitest`** if:

- You need **advanced mocking**, **fake timers**, **snapshot testing**, or **custom matchers**.
    
- You're testing in a **frontend framework** (like React, Vue).
    
- You already use **Vite** (e.g., with Next.js, Vue, etc.).
    
- You need a **Jest-like** experience in modern tooling.
    

---

### 👀 Personal Take?

If you're working on a modern web app, **Vitest** is currently more **battle-tested and feature-complete**.  
But if Bun keeps evolving, `bun test` could become a super compelling minimal alternative — especially for fast CLI-driven testing.

---

Let me know your stack — I can recommend the best one for your exact setup!

