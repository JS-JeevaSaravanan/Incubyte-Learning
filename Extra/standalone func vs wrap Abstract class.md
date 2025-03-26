

Using a **utility class** like `AppleListingErrorsUtil` versus using **standalone functions** depends on the use case and project structure. Here's a breakdown of when to use each approach and what is considered **good practice**.

---

### **🔹 Utility Class Approach (`AppleListingErrorsUtil`)**

```ts
export class AppleListingErrorsUtil {
  static failedDueToNoChanges(errorResponseData: any): boolean {
    return errorResponseData?.[0]?.code === "RESOURCE_HAS_NO_NEW_CHANGES";
  }

  static failedDueToValidation(errorResponseData: any): boolean {
    if (!Array.isArray(errorResponseData)) return false;

    const errorCodes = errorResponseData.map(({ code }) => code);
    const allErrorsAreValidationErrors = errorCodes.every(code => code.startsWith("VALIDATION__"));
    const hasLocationIdMismatchError = errorCodes.includes("VALIDATION__PartnersLocationIdNotMatched");

    return allErrorsAreValidationErrors && !hasLocationIdMismatchError;
  }
}
```

#### **✅ When is this a Good Practice?**

✔ When grouping multiple related functions into a single **cohesive utility class**  
✔ When you need **a clear namespace** (e.g., `AppleListingErrorsUtil.failedDueToValidation()`)  
✔ When you want to **extend or mock the class** in tests  
✔ When you're working in **OOP-heavy codebases**

#### **❌ When to Avoid?**

✘ When the class has **only static methods** and no instance state (can be overkill)  
✘ When you just need **pure functions** that don’t rely on any internal state

---

### **🔹 Standalone Function Approach (Better for Functional Style)**

```ts
export const failedDueToNoChanges = (errorResponseData: any): boolean => {
  return errorResponseData?.[0]?.code === "RESOURCE_HAS_NO_NEW_CHANGES";
};

export const failedDueToValidation = (errorResponseData: any): boolean => {
  if (!Array.isArray(errorResponseData)) return false;

  const errorCodes = errorResponseData.map(({ code }) => code);
  const allErrorsAreValidationErrors = errorCodes.every(code => code.startsWith("VALIDATION__"));
  const hasLocationIdMismatchError = errorCodes.includes("VALIDATION__PartnersLocationIdNotMatched");

  return allErrorsAreValidationErrors && !hasLocationIdMismatchError;
};
```

#### **✅ When is this a Good Practice?**

✔ When you **don't need OOP** and just need a few reusable functions  
✔ When you **favor functional programming** (easier to test, tree-shakable in JS bundlers)  
✔ When functions are **pure and independent** (no shared state)  
✔ When you want to **avoid unnecessary class boilerplate**

#### **❌ When to Avoid?**

✘ If you have many related functions and **want better organization**  
✘ If your project follows **OOP principles strictly**

---

### **🏆 Best Practice Recommendation**

✅ **Use standalone functions** if your project favors functional programming and you just need simple reusable functions.  
✅ **Use a utility class** if you want better organization and a clear grouping of related error-handling functions.

For your case, since these functions are **pure and do not need shared state**, the **standalone function approach is the best choice**! 🚀

Would you like a hybrid approach or more suggestions based on your project's structure? 😊


