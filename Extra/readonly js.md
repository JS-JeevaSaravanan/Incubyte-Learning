
**1. Configuration Objects:**

- Imagine you have a configuration object that holds settings for your application. You want to ensure that these settings are not accidentally modified during runtime.

TypeScript

```
interface AppConfig {
  readonly port: number;
  readonly databaseUrl: string;
  readonly apiKey: string;
}

const config: AppConfig = {
  port: 3000,
  databaseUrl: 'mongodb://localhost:27017/myapp',
  apiKey: 'your_api_key',
};

// Prevents accidental modification:
// config.port = 8080; // Error: Cannot assign to 'port' because it is a read-only property.
```

**2. API Responses:**

- When fetching data from an API, you might want to treat the response object as read-only to prevent accidental modifications that could corrupt the data.

TypeScript

```
interface User {
  readonly id: number;
  readonly name: string;
  readonly email: string;
}

fetch('/api/users')
  .then(response => response.json() as User)
  .then(users => {
    users.forEach(user => {
      // user.email = 'new@example.com'; // Error: Cannot assign to 'email' because it is a read-only property.
      console.log(user.email);
    });
  });
```

**3. Class Properties:**

- In classes, use `readonly` for properties that should be initialized in the constructor and never changed afterward.

TypeScript

```
class User {
  readonly id: number;
  name: string;

  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }

  // Cannot change id after initialization:
  // updateId(newId: number) {
  //   this.id = newId; // Error: Cannot assign to 'id' because it is a read-only property.
  // }
}
```

**4. Function Parameters:**

- Use `readonly` for function parameters to prevent the function from modifying the passed object.

TypeScript

```
function displayUserInfo(user: readonly User) {
  // user.name = 'New Name'; // Error: Cannot assign to 'name' because it is a read-only property.
  console.log(user.name);
}
```

**5. Array and Tuple Types:**

- Use `readonly` with array and tuple types to prevent modifications to the elements.

TypeScript

```
const numbers: readonly number= [1, 2, 3];
// numbers.push(4); // Error: Property 'push' does not exist on type 'readonly number'.

const point: readonly [number, number] = [10, 20];
// point[0] = 5; // Error: Cannot assign to '0' because it is a read-only property.
```

**Benefits of Using Readonly:**

- **Immutability:** Enforces immutability, preventing accidental modifications and making your code more predictable.
- **Type Safety:** The compiler helps you catch potential errors related to modifying read-only properties.
- **Clarity:** Clearly communicates the intent that certain properties should not be changed.
- **Maintainability:** Makes your code easier to reason about and maintain, especially in large projects.

By using `readonly` effectively, you can improve the reliability and maintainability of your TypeScript code.



