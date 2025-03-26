
```ts
const UTM_PARAMS = {
  SOURCE: {
    GOOGLE_BUSINESS_PROFILE: "google_business_profile",
    BING_PLACES_FOR_BUSINESS: "bing_places_for_business",
  },
  MEDIUM: {
    APPOINTMENT: "appointment",
    WEBSITE: "website",
  },
} as const;

// ✅ Dynamic Type
type UtmParams = {
  [K in keyof typeof UTM_PARAMS as `utm_${Lowercase<K>}`]: 
    (typeof UTM_PARAMS)[K][keyof (typeof UTM_PARAMS)[K]];
};

// ✅ Works with strong typing
const validUtmParams: UtmParams = {
  utm_source: UTM_PARAMS.SOURCE.GOOGLE_BUSINESS_PROFILE, // ✅ Valid
  utm_medium: UTM_PARAMS.MEDIUM.WEBSITE, // ✅ Valid
};

// ❌ TypeScript Error: Invalid key "utm_campaign"
const invalidUtmParams: UtmParams = {
  utm_campaign: "summer_sale",
};

```


Let's break it down step by step:

---

```ts
const UTM_KEY_PREFIX = "utm_";

  

const UTM_KEYS = {

SOURCE: `${UTM_KEY_PREFIX}source`,

MEDIUM: `${UTM_KEY_PREFIX}medium`,

} as const;

  

const UTM_VALUES = {

SOURCE: {

GOOGLE_BUSINESS_PROFILE: "google_business_profile",

BING_PLACES_FOR_BUSINESS: "bing_places_for_business",

},

MEDIUM: {

APPOINTMENT: "appointment",

WEBSITE: "website",

},

} as const;

  
  

type UtmParams = {

[K in keyof typeof UTM_KEYS]:

(typeof UTM_VALUES)[K][keyof (typeof UTM_VALUES)[K]];

};

  
  
  
  

export { UTM_KEYS, UTM_VALUES, UTM_KEY_PREFIX, type UtmParams };
```


### **Understanding the Type Definition**

```ts
type UtmParams = {
  [K in keyof typeof UTM_KEYS]: 
    (typeof UTM_VALUES)[K][keyof (typeof UTM_VALUES)[K]];
};
```

---

### **Step-by-Step Breakdown**

1️⃣ **`keyof typeof UTM_KEYS`** → Gets all keys of `UTM_KEYS`.

- `UTM_KEYS` is:
    
    ```ts
    const UTM_KEYS = {
      SOURCE: "utm_source",
      MEDIUM: "utm_medium",
    } as const;
    ```
    
- `keyof typeof UTM_KEYS` results in:
    
    ```ts
    "SOURCE" | "MEDIUM"
    ```
    

2️⃣ **`[K in keyof typeof UTM_KEYS]`** → Iterates over `"SOURCE"` and `"MEDIUM"` to define dynamic keys in `UtmParams`.

3️⃣ **Mapping the Keys (`UTM_KEYS[K]`)**

- `K` represents `"SOURCE"` or `"MEDIUM"`.
- `UTM_KEYS[K]` resolves to `"utm_source"` or `"utm_medium"`.
- This means `UtmParams` will have keys:
    
    ```ts
    {
      "utm_source": ...;
      "utm_medium": ...;
    }
    ```
    

4️⃣ **Mapping the Values (`(typeof UTM_VALUES)[K][keyof (typeof UTM_VALUES)[K]]`)**

- `typeof UTM_VALUES` is:
    
    ```ts
    const UTM_VALUES = {
      SOURCE: {
        GOOGLE_BUSINESS_PROFILE: "google_business_profile",
        BING_PLACES_FOR_BUSINESS: "bing_places_for_business",
      },
      MEDIUM: {
        APPOINTMENT: "appointment",
        WEBSITE: "website",
      },
    } as const;
    ```
    
- `(typeof UTM_VALUES)[K]` gets the **object** corresponding to `K`.
    - If `K` is `"SOURCE"`, it gets:
        
        ```ts
        {
          GOOGLE_BUSINESS_PROFILE: "google_business_profile",
          BING_PLACES_FOR_BUSINESS: "bing_places_for_business",
        }
        ```
        
- `[keyof (typeof UTM_VALUES)[K]]` gets **all possible values** from that object.
    - For `"SOURCE"`, possible values are:
        
        ```ts
        "google_business_profile" | "bing_places_for_business"
        ```
        
    - For `"MEDIUM"`, possible values are:
        
        ```ts
        "appointment" | "website"
        ```
        

---

### **Final Output (`UtmParams` Type)**

```ts
type UtmParams = {
  "utm_source": "google_business_profile" | "bing_places_for_business";
  "utm_medium": "appointment" | "website";
};
```

---

### **Summary (Crisp & Clear)**

- **Keys (`utm_source`, `utm_medium`)** → Derived dynamically from `UTM_KEYS`.
- **Values (possible strings for each key)** → Extracted from `UTM_VALUES`.
- **Ensures type safety** → Only valid key-value pairs are allowed.

This type ensures that when calling `addUtmParameters()`, only **correct UTM key-value pairs** can be passed! 🚀


```ts
type UtmParams = {

[K in keyof typeof UTM_KEYS as (typeof UTM_KEYS)[K]]:

(typeof UTM_VALUES)[K][keyof (typeof UTM_VALUES)[K]];

};
```


