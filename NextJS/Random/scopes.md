

### **NestJS Scope Explanation with Real-World Example**

In **NestJS**, **scope** determines how a provider (service, repository, etc.) is instantiated and shared within the application. There are three types:

#### **1. Default (Singleton) Scope**

- A single instance is created and shared across the entire application.
    
- Best for stateless services like logging, database connections, or business logic.
    

📌 **Example:** A **LoggerService** used across multiple modules.

```ts
@Injectable()
export class LoggerService {
  log(message: string) {
    console.log(`[LOG]: ${message}`);
  }
}
```

✅ **All modules that inject `LoggerService` get the same instance.**

---

#### **2. Request Scope (`Scope.REQUEST`)**

- A new instance is created for every incoming request.
    
- Best for handling request-specific data, like per-user authentication.
    

📌 **Example:** **UserSessionService** that stores data per request.

```ts
@Injectable({ scope: Scope.REQUEST })
export class UserSessionService {
  constructor(@Inject(REQUEST) private request: Request) {}

  getUser() {
    return this.request.user; // Assume user is attached in middleware
  }
}
```

✅ **Each request gets a fresh `UserSessionService` instance.**

---

#### **3. Transient Scope (`Scope.TRANSIENT`)**

- A new instance is created each time it is injected.
    
- Best for lightweight services that shouldn't be shared.
    

📌 **Example:** A **FormatterService** that provides utility methods.

```ts
@Injectable({ scope: Scope.TRANSIENT })
export class FormatterService {
  format(value: string) {
    return value.toUpperCase();
  }
}
```

✅ **Every injection of `FormatterService` creates a new instance.**

### **When to Use What?**

|Scope|When to Use|
|---|---|
|**Singleton (default)**|Shared services (e.g., logging, DB connections)|
|**Request**|Per-request state management (e.g., authentication)|
|**Transient**|Independent, short-lived services (e.g., utilities)|

Would you like a more detailed example on any scope? 🚀




https://docs.nestjs.com/fundamentals/injection-scopes


### **Understanding NestJS Scope and Dependency Injection Bubbling**

In **NestJS**, when a provider (service) is **request-scoped** or **transient**, it **bubbles up** through the dependency tree. This means that:

- A `Scope.REQUEST` service will force all its dependencies to also become request-scoped.
    
- A `Scope.TRANSIENT` service will create new instances of itself and its dependencies wherever it is injected.
    

---

## **1️⃣ Request Scope Bubbling (Scope.REQUEST)**

### **How It Works?**

- If a `REQUEST`-scoped service depends on another service, that dependency also becomes `REQUEST`-scoped.
    
- This ensures that services remain tied to a single request lifecycle.
    

---

### **Example: User Request Tracking**

Imagine we need to track the authenticated user for each request.

#### **Step 1: Create `UserSessionService` (Request-Scoped)**

```ts
import { Injectable, Scope, Inject } from '@nestjs/common';
import { REQUEST } from '@nestjs/core';
import { Request } from 'express';

@Injectable({ scope: Scope.REQUEST }) // Mark as request-scoped
export class UserSessionService {
  constructor(@Inject(REQUEST) private request: Request) {}

  getUser() {
    return this.request.user; // Assuming user is attached in middleware
  }
}
```

---

#### **Step 2: Create `OrderService` (Depends on UserSessionService)**

```ts
import { Injectable } from '@nestjs/common';
import { UserSessionService } from './user-session.service';

@Injectable()
export class OrderService {
  constructor(private readonly userSessionService: UserSessionService) {}

  getOrdersForUser() {
    const user = this.userSessionService.getUser();
    return `Fetching orders for ${user?.name || 'Guest'}`;
  }
}
```

✅ **Now, `OrderService` also becomes request-scoped automatically due to bubbling up!**

---

#### **Step 3: Use in a Controller**

```ts
import { Controller, Get } from '@nestjs/common';
import { OrderService } from './order.service';

@Controller('orders')
export class OrderController {
  constructor(private readonly orderService: OrderService) {}

  @Get()
  getOrders() {
    return this.orderService.getOrdersForUser();
  }
}
```

📌 Since `OrderService` depends on `UserSessionService`, it automatically becomes **request-scoped**, ensuring **each request gets a fresh instance**.

---

## **2️⃣ Transient Scope Bubbling (Scope.TRANSIENT)**

### **How It Works?**

- A `TRANSIENT`-scoped service creates **a new instance every time it is injected**.
    
- Any service depending on a `TRANSIENT`-scoped service does **not** automatically become transient.
    

---

### **Example: Formatting Service (New Instance Every Injection)**

#### **Step 1: Create `FormatterService` (Transient Scoped)**

```ts
import { Injectable, Scope } from '@nestjs/common';

@Injectable({ scope: Scope.TRANSIENT }) // Mark as transient
export class FormatterService {
  format(value: string) {
    return value.toUpperCase();
  }
}
```

---

#### **Step 2: Create `ReportService` (Uses FormatterService Multiple Times)**

```ts
import { Injectable } from '@nestjs/common';
import { FormatterService } from './formatter.service';

@Injectable()
export class ReportService {
  constructor(private readonly formatterService: FormatterService) {}

  generateReport(data: string) {
    return this.formatterService.format(data);
  }
}
```

✅ **Each injection of `FormatterService` creates a new instance, but `ReportService` remains singleton!**

---

#### **Step 3: Use in a Controller**

```ts
import { Controller, Get } from '@nestjs/common';
import { ReportService } from './report.service';

@Controller('report')
export class ReportController {
  constructor(private readonly reportService: ReportService) {}

  @Get()
  getReport() {
    return this.reportService.generateReport('hello world');
  }
}
```

📌 Every time `FormatterService` is injected, it gets a **new instance**, even within the same request.

---

## **🚀 Key Takeaways**

|Scope|Bubbling Behavior|
|---|---|
|**Singleton** (default)|No bubbling, same instance everywhere|
|**Request Scoped**|Forces dependencies to also be request-scoped|
|**Transient Scoped**|Does NOT force dependencies to be transient|

Would you like me to extend this with more complex scenarios? 🚀


https://kelvinbz.medium.com/explore-injection-scopes-in-nestjs-03a5e708102a

https://medium.com/better-programming/a-deep-dive-into-nestjs-injection-scope-d45e87fd918d

