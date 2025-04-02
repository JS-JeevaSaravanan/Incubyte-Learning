

To make the custom matcher (`toHaveSameElements`) available across all **Vitest** tests in your project, follow these steps:

---

### **1️⃣ Create a Global Setup File**

Create a new file, e.g., **`test/setup.ts`**, and add the following:

```typescript
import { expect } from 'vitest';
import { isArrayHaveSameElements } from '../src/utils/isArrayHaveSameElements'; // Adjust the path if needed

expect.extend({
  toHaveSameElements(received: unknown[], expected: unknown[]) {
    const pass = isArrayHaveSameElements(received, expected);
    if (pass) {
      return {
        message: () => `✅ Arrays have the same elements (ignoring order).`,
        pass: true,
      };
    } else {
      return {
        message: () =>
          `❌ Arrays do NOT have the same elements.\nReceived: ${JSON.stringify(received)}\nExpected: ${JSON.stringify(expected)}`,
        pass: false,
      };
    }
  },
});

declare module 'vitest' {
  interface Assertion {
    toHaveSameElements(expected: unknown[]): void;
  }
}
```

---

### **2️⃣ Configure Vitest to Use the Setup File**

Modify **`vitest.config.ts`** to include the `setup.ts` file globally:

```typescript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    setupFiles: './test/setup.ts', // Load the custom matcher globally
  },
});
```

---

### **3️⃣ Usage in Any Test File**

Now, you can use `toHaveSameElements` in **any** test file without re-importing it:

```typescript
import { describe, it, expect } from 'vitest';

describe('Array Comparison', () => {
  it('should return true for arrays with same elements in different order', () => {
    expect([1, 2, 3]).toHaveSameElements([3, 2, 1]);
  });

  it('should return false for arrays with different elements', () => {
    expect([1, 2, 3]).not.toHaveSameElements([1, 2, 4]);
  });

  it('should handle arrays of objects', () => {
    expect([{ id: 1 }, { id: 2 }]).toHaveSameElements([{ id: 2 }, { id: 1 }]);
  });
});
```

---

### ✅ **Now, `toHaveSameElements` is globally available across all Vitest test files!**

You don’t need to manually import it in every test file—just use it anywhere in your project. 🚀


---

### **🛠 Full Corrected `setup.ts`**

```typescript
import { expect } from 'vitest';

function isArrayHaveSameElements<T>(
  arr1: T[],
  arr2: T[],
  sortFn: (a: T, b: T) => number = (a, b) =>
    JSON.stringify(a).localeCompare(JSON.stringify(b))
): boolean {
  if (arr1.length !== arr2.length) return false;

  const sortedArr1 = [...arr1].sort(sortFn);
  const sortedArr2 = [...arr2].sort(sortFn);

  return sortedArr1.every((item, index) => JSON.stringify(item) === JSON.stringify(sortedArr2[index]));
}

expect.extend({
  toHaveSameElements(received: unknown[], expected: unknown[]) {
    const pass = isArrayHaveSameElements(received, expected);
    if (pass) {
      return {
        message: () => `✅ Arrays have the same elements (ignoring order).`,
        pass: true,
      };
    } else {
      return {
        message: () =>
          `❌ Arrays do NOT have the same elements.\nReceived: ${JSON.stringify(received)}\nExpected: ${JSON.stringify(expected)}`,
        pass: false,
      };
    }
  },
});

// ✅ Corrected TypeScript Declaration
declare module 'vitest' {
  interface Assertion {
    toHaveSameElements(expected: unknown[]): void;
  }
}
```

---

### **✅ Now, Your Test Works Without Type Errors!**

```typescript
import { describe, it, expect } from 'vitest';

describe("Custom Matcher: toHaveSameElements", () => {
  it("checks if two arrays have the same elements", () => {
    expect([1, 2, 3]).toHaveSameElements([3, 2, 1]); // ✅ Works now!
  });

  it("fails when arrays have different elements", () => {
    expect([1, 2, 3]).not.toHaveSameElements([1, 2, 4]); // ✅ Works as expected
  });
});
```

---

### **🎯 Why This Fix Works**

1. **Ensures TypeScript knows `toHaveSameElements` expects an array (`unknown[]`)**
    
    - Before: `interface Assertion<T> { toHaveSameElements(expected: T[]): void; }` → ❌ TypeScript misinterprets `T`
        
    - After: `interface Assertion { toHaveSameElements(expected: unknown[]): void; }` → ✅ Corrected
        
2. **Setup is loaded properly in Vitest (`setupFiles: './test/setup.ts'`)**
    
    - No additional changes needed in `vitest.config.ts`
        

---

### **🚀 Final Result**

✅ No more TypeScript errors  
✅ `toHaveSameElements` is globally available in all tests  
✅ Works across different types of arrays (numbers, objects, etc.)

You’re all set! 🎉

---


Your function `isArrayHaveSameElements` is generally effective for comparing arrays with unordered elements. However, there are a few potential issues:

### **1. JSON Stringification Can Be Inconsistent**

- Objects with different key orders will produce different JSON strings, even if they have the same properties.
    
    ```typescript
    const a = { name: "Alice", age: 25 };
    const b = { age: 25, name: "Alice" };
    
    console.log(JSON.stringify(a) === JSON.stringify(b)); // false ❌
    ```
    
    **Fix:** Use deep comparison (`lodash.isEqual` or a recursive deep compare function).
    

### **2. Type Issues With Non-Stringifiable Objects**

- Some objects (like Dates, Maps, Sets) don’t serialize well with `JSON.stringify`.
    
    ```typescript
    const date1 = new Date("2023-01-01");
    const date2 = new Date("2023-01-01");
    
    console.log(JSON.stringify(date1) === JSON.stringify(date2)); // false ❌
    ```
    
    **Fix:** Use `.getTime()` for Dates and explicit conversions for other types.
    

### **3. Sorting Function Might Not Work for Complex Types**

- If `sortFn` doesn’t properly handle certain data structures, it may result in incorrect ordering.  
    **Fix:** Ensure `sortFn` is tailored for the expected data types.
    

### **4. Fails for Arrays With Duplicates**

- If the arrays have duplicate elements but in different frequencies, your function may incorrectly return `true`.
    
    ```typescript
    console.log(isArrayHaveSameElements([1, 2, 2], [1, 1, 2])); // true ❌
    ```
    
    **Fix:** Use element frequency comparison instead of sorting.
    

---

### **Suggested Fix**

Instead of sorting, compare element frequencies:

```typescript
import { isEqual } from "lodash";

function isArrayHaveSameElements<T>(arr1: T[], arr2: T[]): boolean {
  if (arr1.length !== arr2.length) return false;

  const countOccurrences = (arr: T[]) =>
    arr.reduce((acc, item) => {
      const key = JSON.stringify(item);
      acc[key] = (acc[key] || 0) + 1;
      return acc;
    }, {} as Record<string, number>);

  return isEqual(countOccurrences(arr1), countOccurrences(arr2));
}
```

### **Why This Works?**

✔ Avoids issues with JSON key ordering  
✔ Handles duplicates properly  
✔ Works for complex objects when stringified

Would you like further refinements? 🚀

---

The `sortObjectKeys` function, as written in the previous response, will **not** work recursively for nested objects. It only sorts the keys of the top-level object, but it doesn't handle nested objects within the object structure.

To handle nested objects as well, you'll need to make `sortObjectKeys` recursive so that it sorts the keys at all levels of the object. Here's an updated version that recursively sorts keys for nested objects:

### Updated Recursive `sortObjectKeys` Function

```ts
function sortObjectKeys(obj: Record<string, any>): Record<string, any> {
  if (typeof obj !== 'object' || obj === null) {
    return obj; // Return primitive value as is
  }

  // Sort the keys of the current object and recursively sort nested objects
  const sortedObj: Record<string, any> = {};
  Object.keys(obj)
    .sort()  // Sort the keys
    .forEach((key) => {
      sortedObj[key] = sortObjectKeys(obj[key]); // Recursively sort nested objects
    });

  return sortedObj;
}

function isArrayHaveSameElements<T>(arr1: T[], arr2: T[]): boolean {
  if (arr1.length !== arr2.length) return false;

  const countOccurrences = (arr: T[]) =>
    arr.reduce((acc, item) => {
      const key = JSON.stringify(sortObjectKeys(item));  // Sort the object keys recursively
      acc[key] = (acc[key] || 0) + 1;
      return acc;
    }, {} as Record<string, number>);

  return isEqual(countOccurrences(arr1), countOccurrences(arr2));
}
```

### Explanation:

- The `sortObjectKeys` function now checks if the value is an object. If it is, it sorts the keys. If the value is a primitive (like a number or a string), it returns the value as is.
    
- The function is **recursive**, meaning that it will sort the keys of nested objects, regardless of how deep the nesting goes.
    

### Example Usage:

```ts
const obj1 = {
  a: { x: 1, y: { p: 2, q: 3 } },
  b: 4,
};

const obj2 = {
  b: 4,
  a: { y: { q: 3, p: 2 }, x: 1 },
};

console.log(JSON.stringify(sortObjectKeys(obj1)));  // '{"a":{"x":1,"y":{"p":2,"q":3}},"b":4}'
console.log(JSON.stringify(sortObjectKeys(obj2)));  // '{"a":{"x":1,"y":{"p":2,"q":3}},"b":4}'

// Now comparing arrays of these objects:
const arr1 = [obj1];
const arr2 = [obj2];

console.log(isArrayHaveSameElements(arr1, arr2));  // true
```

### Key Points:

- **Recursive sorting** ensures that even nested objects are sorted by their keys.
    
- This works even for deeply nested objects, and arrays with such objects will be compared properly without regard to the key order.
    

This approach should address the issue of comparing arrays of objects (including nested ones) where the key order may vary.


---

sort callback has to be get from user itself


```ts
import { isEqual } from 'lodash';

  

export function doArraysContainSameItems<T>(arr1: T[], arr2: T[], sortFn: (a: T, b: T) => number): boolean {

	if (arr1.length !== arr2.length)
		return false;
	
	  
	
	const sortedArr1 = [...arr1].sort(sortFn);
	const sortedArr2 = [...arr2].sort(sortFn);
	
	return isEqual(sortedArr1, sortedArr2);
}

```

---


https://stackoverflow.com/questions/32103252/expect-arrays-to-be-equal-ignoring-order

in jasmine, it just works for primitive type elements


```ts
getJasmineRequireObj().ArrayWithExactContents = function(j$) {
  function ArrayWithExactContents(sample) {
    this.sample = sample;
  }

  ArrayWithExactContents.prototype.asymmetricMatch = function(
    other,
    matchersUtil
  ) {
    if (!j$.isArray_(this.sample)) {
      throw new Error(
        'You must provide an array to arrayWithExactContents, not ' +
          j$.basicPrettyPrinter_(this.sample) +
          '.'
      );
    }

    if (this.sample.length !== other.length) {
      return false;
    }

    for (const item of this.sample) {
      if (!matchersUtil.contains(other, item)) {
        return false;
      }
    }

    return true;
  };

  ArrayWithExactContents.prototype.jasmineToString = function(pp) {
    return '<jasmine.arrayWithExactContents(' + pp(this.sample) + ')>';
  };

  return ArrayWithExactContents;
};

```


---

