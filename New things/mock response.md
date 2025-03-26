
locationClient.getLocationByIdV3.mockResolvedValueOnce({ data: { etag: 'etag' } }); locationClient.updateLocationV3.mockRejectedValueOnce({ response: { data: 'failed' } }); is see resolved don't have response.. but rejected have response wrapper.. is it vitest specific ?

No, this behavior is not specific to Vitest. It is a common convention in many HTTP client libraries like Axios. Here’s why:

### ✅ **Resolved Values (Success)**

When a request succeeds, most HTTP client libraries (e.g., Axios) return only the response **data** by default. This is why you typically mock resolved values like this:

```javascript
locationClient.getLocationByIdV3.mockResolvedValueOnce({ data: { etag: 'etag' } });
```

This means that when the function is awaited, it returns `{ data: { etag: 'etag' } }` directly.

### ❌ **Rejected Values (Error)**

When a request fails, the error object usually follows a **structured format**, often including metadata such as HTTP status, headers, and response body. For example, in Axios, errors contain a `response` object, like so:

```javascript
locationClient.updateLocationV3.mockRejectedValueOnce({ 
  response: { data: 'failed' } 
});
```

This mimics the behavior of an actual HTTP client error, where the rejected promise contains additional information inside a `response` wrapper.

### ✅ **Axios Example**

This mirrors how Axios works in real API calls:

```javascript
axios.get('/some-url')
  .then(response => console.log(response.data)) // ✅ Success: response.data
  .catch(error => console.log(error.response.data)); // ❌ Error: error.response.data
```

### 🔥 **Vitest vs. Jest?**

This behavior is **not** Vitest-specific—Jest, Vitest, and other test runners simply mock functions as instructed. The `response` wrapper is used **only because** it matches how HTTP clients (like Axios) structure their error objects. If you were using `fetch`, for example, you might mock errors differently.

Would you like me to adjust this to match a different library's behavior, such as Fetch API? 🚀

