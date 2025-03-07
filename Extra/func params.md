

Question:
Passing less params to function vs passing non primitive types on param of function.. could you give best practice for it.. we need to reduce param count for better readability and maintainability we need to pass non primitive datatypes to make sure that param is not updated in function at any cost.. The programming language we use is TypeScript

Answer:

In TypeScript, balancing the number of function parameters and ensuring immutability can be achieved through a few best practices:

---

### **1. Reduce Parameter Count with Object Destructuring**

Instead of passing multiple parameters, wrap them inside an object. This improves readability and makes it easier to extend functionality without modifying function signatures.

✅ **Preferred Approach (Using an Object)**

```typescript
interface UserConfig {
  name: string;
  age: number;
  isAdmin?: boolean;
}

function createUser({ name, age, isAdmin = false }: UserConfig): void {
  console.log(`User: ${name}, Age: ${age}, Admin: ${isAdmin}`);
}

createUser({ name: "Alice", age: 30 });
```

❌ **Avoid This (Too Many Parameters)**

```typescript
function createUser(name: string, age: number, isAdmin: boolean = false): void {
  console.log(`User: ${name}, Age: ${age}, Admin: ${isAdmin}`);
}

createUser("Alice", 30);
```

**Benefits**:

- Improved readability.
- Easier to extend (e.g., adding new properties in `UserConfig`).
- Named parameters prevent ordering mistakes.

---

### **2. Pass Non-Primitive Types to Ensure No Mutation**

If you want to **prevent accidental modification** of an object inside a function, use **Readonly** or immutable types.

✅ **Use `Readonly<T>` for Function Parameters**

```typescript
interface User {
  name: string;
  age: number;
}

function printUser(user: Readonly<User>): void {
  // user.age = 25; ❌ Error: Cannot assign to 'age' because it is a read-only property.
  console.log(`User: ${user.name}, Age: ${user.age}`);
}

const user: User = { name: "Bob", age: 25 };
printUser(user);
```

✅ **Use `readonly` in Arrays to Prevent Mutation**

```typescript
function printItems(items: readonly string[]): void {
  // items.push("newItem"); ❌ Error: Property 'push' does not exist on type 'readonly string[]'.
  console.log(items.join(", "));
}

const names: string[] = ["Alice", "Bob"];
printItems(names);
```

**Benefits**:

- Ensures that the function **does not modify the object/array**.
- Encourages immutability and functional programming best practices.

---

### **3. Use Class or Types for Grouping Related Params**

When multiple parameters are always used together, create a class or type.

```typescript
class User {
  constructor(public readonly name: string, public readonly age: number) {}

  describe(): string {
    return `${this.name} is ${this.age} years old.`;
  }
}

function printUser(user: User): void {
  console.log(user.describe());
}

const user = new User("Charlie", 35);
printUser(user);
```

---

### **Key Takeaways**

✅ **Reduce function parameters** by wrapping them in objects (`{}` or a class).  
✅ **Use `Readonly<T>`** to enforce immutability and avoid unintended modifications.  
✅ **Prefer named parameters** (object destructuring) over long parameter lists for maintainability.  
✅ **For complex related parameters, use types or classes** to encapsulate logic.






