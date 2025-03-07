
https://dev.to/srishtikprasad/builder-design-pattern-3a7j


to check {

https://dev.to/srishtikprasad/series/29009

}

---
the builder design pattern in js object creation, makes to code to put default values for object and we have to reset to the values that we required for.. if we miss anything, we don't know, the value of the key is default..

ans:

Yes, that's a common issue with the **Builder pattern** in JavaScript. When using it for object creation, default values are often set internally, and if we forget to explicitly set a property, we might unknowingly use the default value.

### **Ways to Handle This Issue**

1. **Validation in the `build()` Method**
    
    - Before returning the final object, validate that all required fields are explicitly set.
    - If any required field is missing, throw an error.
    
    ```js
    class UserBuilder {
        constructor() {
            this.name = "Default Name";
            this.age = 25;
            this.email = null;
        }
    
        setName(name) {
            this.name = name;
            return this;
        }
    
        setAge(age) {
            this.age = age;
            return this;
        }
    
        setEmail(email) {
            this.email = email;
            return this;
        }
    
        build() {
            if (!this.email) {
                throw new Error("Email is required but not set!");
            }
            return { name: this.name, age: this.age, email: this.email };
        }
    }
    
    const user = new UserBuilder()
        .setName("Alice")
        .setAge(30)
        .build(); // Throws an error because email is missing
    ```
    
2. **Logging Warnings for Default Values**
    
    - If a property still has a default value, log a warning.
    
    ```js
    build() {
        if (this.email === null) {
            console.warn("Warning: Email is not set, using default value:", this.email);
        }
        return { name: this.name, age: this.age, email: this.email };
    }
    ```
    
3. **Using TypeScript for Better Type Safety**
    
    - In TypeScript, you can enforce required fields using types.
    
    ```ts
    class UserBuilder {
        private name: string = "Default Name";
        private age: number = 25;
        private email?: string;
    
        setName(name: string) { this.name = name; return this; }
        setAge(age: number) { this.age = age; return this; }
        setEmail(email: string) { this.email = email; return this; }
    
        build(): { name: string; age: number; email: string } {
            if (!this.email) throw new Error("Email is required!");
            return { name: this.name, age: this.age, email: this.email };
        }
    }
    ```
    

Would you like a more refined implementation based on your use case? 🚀

