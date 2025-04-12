

### ✅ Example

```ts
it.each([
  { case: 'adds 1 + 1 correctly', a: 1, b: 1, expected: 2 },
  { case: 'adds 2 + 3 correctly', a: 2, b: 3, expected: 5 },
])('$case', ({ a, b, expected }) => {
  expect(a + b).toBe(expected);
});
```

---

### 🧠 Explanation

- `it.each([...])`: Runs the test for **each object** in the array.
    
- `$case`: This is replaced by the `case` value in the test name.
    
- `({ a, b, expected })`: **Destructures** the values from the test case so you can use them directly.
    
- `expect(a + b).toBe(expected)`: The actual test logic.
    

---

### 🔍 Output when running tests

```
✓ adds 1 + 1 correctly
✓ adds 2 + 3 correctly
```

Super readable, less code, and easy to scale!

Let me know if you want one for a real-world function too.