

unix time
* no time zone


It's UTC + [ISO-8601](https://www.iso.org/iso-8601-date-and-time-format.html) standard


### **1️⃣ UTC (Coordinated Universal Time)**

- UTC is the **global time standard** and does not change with daylight saving time.
- It is not specific to India (IST) or the US (EST/PST), making it a neutral reference.

### **2️⃣ ISO-8601 Standard Format**

- This is an **internationally accepted format for date and time representation**.
- It looks like this:
    - **Basic format:** `YYYY-MM-DDTHH:mm:ssZ` (e.g., `2025-03-19T12:45:30Z`)
    - **Extended format with offset:** `YYYY-MM-DDTHH:mm:ss±HH:mm` (e.g., `2025-03-19T12:45:30+05:30`)

**Breaking it down:**

- `T` separates the date and time.
- `Z` at the end means the time is in **UTC** (if there's no offset).
- If an offset is present (`+05:30`), it indicates the time zone difference from UTC.

### **What This Means for You**

- The report's timestamp will always be in **UTC**, ensuring consistency across different locations.
- Instead of Unix timestamps (which are hard to read), it now uses an easily understandable **ISO-8601 format**.
