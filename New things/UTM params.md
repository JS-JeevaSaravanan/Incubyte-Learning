


https://blog.hubspot.com/customers/understanding-basics-utm-parameters


https://ortto.com/learn/what-are-utm-parameters/


https://agencyanalytics.com/blog/utm-tracking




```ts
export function addUtmParameters(url: string, utmParams: Record<string, string>): string {
  if (!url) {
    return url;
  }

  try {
    const urlObject = new URL(url);

    Object.keys(utmParams).forEach(key => {
      urlObject.searchParams.append(key, utmParams[key]);
    });

    return urlObject.toString();
  } catch (e) {
    console.error(`Error adding UTM parameters to URL: ${url}`, e);
    return url; // Consider returning the original URL on error
  }
}

```

is there better alternative then appending ? 

Yes, a better alternative to `append()` is using `set()`, which ensures that if a UTM parameter already exists, it gets replaced instead of adding a duplicate entry.

### **Why use `set()` instead of `append()`?**

- `append(key, value)`: Adds multiple values for the same key if it already exists.
- `set(key, value)`: Ensures only one value exists per key (overwrites existing value if present).

### **Refactored Code Using `set()`**

```ts
export function addUtmParameters(url: string, utmParams: Record<string, string>): string {
  if (!url) {
    return url;
  }

  try {
    const urlObject = new URL(url);

    Object.entries(utmParams).forEach(([key, value]) => {
      urlObject.searchParams.set(key, value);
    });

    return urlObject.toString();
  } catch (e) {
    console.error(`Error adding UTM parameters to URL: ${url}`, e);
    return url; // Return original URL in case of an error
  }
}
```

### **Key Benefits of `set()` Over `append()`**

✅ Ensures only one value per UTM parameter (no duplicates).  
✅ Cleans up URLs that might already contain UTM parameters.  
✅ More predictable behavior, especially when modifying existing query parameters.

Would you like further improvements, such as handling edge cases like invalid URLs more gracefully? 🚀

----

When dealing with **UTM parameters** in URLs, you generally have two ways to add them:

1. **Appending UTM parameters** (e.g., keeping existing query parameters and adding UTMs).
2. **Setting (replacing) UTM parameters** (e.g., overwriting existing ones or removing others before adding UTMs).

### ✅ **Appending UTM Parameters**

**Pros:**

- Preserves existing query parameters (useful if the URL already has tracking, session IDs, or filters).
- Avoids breaking functionality if other query params are required for page behavior.
- Works well when UTMs are being added dynamically via scripts (e.g., in JavaScript-based tracking).

**Cons:**

- Can lead to **long, messy URLs** with redundant parameters.
- Possible **conflicts** if a UTM param already exists (e.g., multiple `utm_source` values).
- Might introduce **duplicate tracking issues** if a user visits different versions of the same URL with different UTMs.

---

### 🔄 **Setting (Replacing) UTM Parameters**

**Pros:**

- Ensures a **clean and consistent tracking URL** without unnecessary or conflicting parameters.
- Avoids **duplicate or outdated UTMs** that could skew analytics.
- Useful when you need **strict attribution control** (e.g., force a specific campaign source).

**Cons:**

- **Removes existing parameters**, which might break functionality (e.g., search filters, session data, custom query strings).
- Can cause **loss of referrer data** if previous tracking parameters are important.
- Requires **careful implementation** to avoid unintentionally dropping necessary params.

---

### **Best Practice Recommendation**

- **If the URL has necessary query params**, append UTMs carefully to avoid breaking anything.
- **If strict tracking control is required**, replace UTMs but ensure critical parameters remain.
- **Consider hybrid approaches**, like appending only when UTMs don’t exist or using referrer-based logic.

Would you like help implementing a JavaScript function for handling UTM updates? 🚀


---

can we append same utm param key with different values ?

Yes, technically, you **can** append the same UTM parameter key multiple times with different values, but the behavior depends on how it's handled by browsers, analytics tools, and servers.

### **What Happens When You Append the Same UTM Key Multiple Times?**

Example:

```plaintext
https://example.com?utm_source=google&utm_source=facebook
```

#### **1️⃣ Browser Behavior**

- Most browsers **do not block duplicate query parameters**.
- When accessing `window.location.search` in JavaScript, you’ll see all occurrences:
    
    ```js
    console.log(new URLSearchParams(window.location.search).getAll("utm_source"));
    // Output: ["google", "facebook"]
    ```
    
- However, if you use `.get("utm_source")`, it only returns the **first** value.

#### **2️⃣ Google Analytics & Tracking Tools**

- **Google Analytics (GA4/UA)**:
    - It **only captures the first occurrence** of a UTM parameter.
    - In our example, GA will register `utm_source=google` and **ignore** the second `utm_source=facebook`.
- **Other tracking tools**:
    - Some tools might log multiple values, but most follow GA’s approach and take the **first occurrence**.

#### **3️⃣ Server-Side Behavior**

- On a backend that processes query parameters, behavior varies:
    - **Express.js (`req.query`)**:
        
        ```js
        app.get("/", (req, res) => {
          console.log(req.query.utm_source);
          // If multiple, Express returns the last value: "facebook"
        });
        ```
        
    - **PHP (`$_GET`)**: Only captures the **last occurrence**.
    - **Django (Python request.GET)**: Keeps the **last value** unless you specifically use `.getlist()`.

---

### **✅ Best Practices**

- **Avoid duplicate UTM keys** unless you know how your analytics tool will handle them.
- If multiple values are needed, consider using **comma-separated values**:
    
    ```plaintext
    https://example.com?utm_source=google,facebook
    ```
    
    - This way, GA and other tools will capture `utm_source="google,facebook"`.
- If you **must track multiple sources**, consider using a **custom query parameter**:
    
    ```plaintext
    https://example.com?utm_source=google&utm_source_alt=facebook
    ```
    
    - Then, handle `utm_source_alt` in your analytics reports.

Would you like a script to clean up duplicate UTM params dynamically? 🚀

---

Yes! In addition to `.set()`, the `URLSearchParams` API provides several methods to manipulate query parameters:

### **1. `.append()`** (Use when allowing duplicate keys)

- Unlike `.set()`, which **overwrites** existing values, `.append()` **adds** a new value to the same key.
- Useful if you need multiple values for the same key (e.g., `?id=1&id=2&id=3`).

```ts
params.append("pageSize", String(normalizedPageSize));
params.append("pageToken", pageToken);
```

### **2. `.get()`** (Retrieve a value)

```ts
const pageSize = params.get("pageSize"); // Returns a string or null
```

### **3. `.getAll()`** (Retrieve all values for a key)

```ts
const allIds = params.getAll("id"); // Returns an array of values
```

### **4. `.has()`** (Check if a parameter exists)

```ts
if (params.has("pageToken")) {
  console.log("pageToken is present");
}
```

### **5. `.delete()`** (Remove a parameter)

```ts
params.delete("pageToken");
```

### **6. `.toString()`** (Convert to a query string)

```ts
const queryString = params.toString(); // "pageSize=10&pageToken=abc123"
```

### **When to Use `.append()` vs `.set()`?**

- ✅ **Use `.set()`** if you only want **one value per key** (default case).
- ✅ **Use `.append()`** if you want **multiple values for the same key** (e.g., filtering with multiple categories).

Let me know if you need a specific use case! 🚀


---
