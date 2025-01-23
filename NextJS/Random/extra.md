
Nest app creation:

```
nest new movies-app
```


```
nest g module $route_name
nest g controller $route_name
nest g service $route_name
```




port : main.ts
default port : 3000

```typescript
// src/main.ts
const port = process.env.PORT || 3000;  // Defaults to 3000 if no PORT is set
await app.listen(port);
```

You can then set the `PORT` environment variable before starting the app:

```bash
PORT=4000 npm run start
```

This will start the application on **port 4000** instead of the default port 3000.











































