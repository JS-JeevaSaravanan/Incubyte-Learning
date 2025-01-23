
To set more than one environment variable when running a command in your terminal (like `npm run start` or `node`), you can use the following syntax.

### For Linux/macOS (Bash Shell)

In a single command, you can set multiple environment variables like this:

```bash
ENV_VAR1=value1 ENV_VAR2=value2 ENV_VAR3=value3 npm run start
```

For example, if you want to set `PORT`, `DATABASE_URL`, and `NODE_ENV` variables when running your NestJS app, you can do:

```bash
PORT=4000 DATABASE_URL=postgres://localhost:5432/mydb NODE_ENV=production npm run start
```

This will set those environment variables **only for this command**.

### For Windows (Command Prompt)

In Windows Command Prompt, the syntax is slightly different. You can set environment variables like this:

```bash
set PORT=4000 && set DATABASE_URL=postgres://localhost:5432/mydb && set NODE_ENV=production && npm run start
```

### For Windows (PowerShell)

In PowerShell, you can set environment variables as follows:

```bash
$env:PORT="4000"; $env:DATABASE_URL="postgres://localhost:5432/mydb"; $env:NODE_ENV="production"; npm run start
```

### 2. **Accessing Environment Variables in NestJS**

Once you set the environment variables, you can access them in your NestJS application using `process.env`. For example:

```typescript
const port = process.env.PORT || 3000;
await app.listen(port);
```

---

### Alternative: Using `.env` Files (Recommended for Multiple Variables)

Instead of setting environment variables in the command line each time, you can use a `.env` file to store all your environment variables and use a library like **`dotenv`** to load them into your application.

1. **Install `dotenv`**:
    
    ```bash
    npm install dotenv
    ```
    
2. **Create a `.env` file** at the root of your project with the following content:
    
    ```ini
    PORT=4000
    DATABASE_URL=postgres://localhost:5432/mydb
    NODE_ENV=production
    ```
    
3. **Load the `.env` file** in your `main.ts` or any entry point of your application:
    
    ```typescript
    import * as dotenv from 'dotenv';
    dotenv.config();
    
    const port = process.env.PORT || 3000;
    await app.listen(port);
    ```
    

With this setup, you don't need to set environment variables manually every time you run the app, and you can manage multiple variables in a centralized `.env` file.

---

Let me know if you need further clarification!