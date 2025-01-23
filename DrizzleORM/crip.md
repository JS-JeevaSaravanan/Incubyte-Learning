### **1. Install Dependencies**

Install Drizzle ORM and your database (e.g., SQLite):

```bash
npm install drizzle-orm sqlite3
```

---

### **2. Set Up Database**

Create your SQLite database file (`database.sqlite`), then create a `users` table:

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  age INTEGER
);
```

---

### **3. Configure Drizzle ORM**

In your TypeScript (`index.ts`), configure Drizzle with SQLite:

```typescript
import { drizzle } from 'drizzle-orm';
import sqlite3 from 'sqlite3';

// Setup SQLite connection
const db = new sqlite3.Database('database.sqlite');

// Define the `users` table structure
const { defineTable, integer, text } = require('drizzle-orm/sqlite-core');
const usersTable = defineTable('users', {
  id: integer().primary(),
  name: text(),
  age: integer(),
});

// Initialize Drizzle ORM with the SQLite connection
const drizzleDb = drizzle(db);
```

---

### **4. Write Queries**

#### **Insert Data**

```typescript
async function insertUser(name: string, age: number) {
  await drizzleDb.insert(usersTable).values({ name, age });
}
```

#### **Get All Users**

```typescript
async function getUsers() {
  const users = await drizzleDb.select().from(usersTable);
  console.log(users);
}
```

#### **Update User**

```typescript
async function updateUser(id: number, name: string) {
  await drizzleDb.update(usersTable).set({ name }).where(usersTable.id.equals(id));
}
```

#### **Delete User**

```typescript
async function deleteUser(id: number) {
  await drizzleDb.delete().from(usersTable).where(usersTable.id.equals(id));
}
```

---

### **5. Run Your Code**

Make sure to run your TypeScript file (`index.ts`):

```json
{
  "scripts": {
    "start": "ts-node index.ts"
  }
}
```

Run:

```bash
npm run start
```

---

### **Key Points to Remember**

- **Define your tables** with `defineTable`.
- Use **chainable methods** like `.select()`, `.insert()`, `.update()`, `.delete()` for CRUD operations.
- Drizzle ORM is **type-safe** for SQL queries.
- You can use **SQLite** or other supported databases.

---

**That's it!** This is a quick and crisp overview of Drizzle ORM.

