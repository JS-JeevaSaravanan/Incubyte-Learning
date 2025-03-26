


batch call test cases:  
  
Siya Agarwal  
  
- Call Counts:  
- 0 (call once)  
- 1000 (call once)  
- max +1 (call twice)  
  
- Business Creation:  
1. Create 1000+ businesses:  
a. Hard code  
b. Write logic (test fixture, test):  
i. createFakeBusinesses(x: number)  
2. In other tests, pass 1000 or 1001  
3. When testing createFakeBusinesses, pass 2  
4. Make max limit an env variable:  
a. prod -> 1000  
b. test -> small number  
5. Method to get max limit:  
a. getMaxLimit  
i. prod -> 1000  
ii. Mock in tests to return small number





but this code can affect test parellism : 

```js
// service.ts
import { Injectable } from '@nestjs/common';
import { getMaxLimit } from './utils'; // Assuming your utility is in utils.ts

@Injectable()
export class MyService {
  async processData(): Promise<number> {
    const max = getMaxLimit();
    // ... some logic using max ...
    return max;
  }
}

// utils.ts
export function getMaxLimit(): number {
  // Complex logic to determine the max limit
  return 100;
}

it('should use a mocked max limit', async () => {
    const mockMaxLimit = 50;
    const getMaxLimitSpy = vi.spyOn(utils, 'getMaxLimit').mockReturnValue(mockMaxLimit); // Mock the function

    const result = await service.processData();

    expect(result).toBe(mockMaxLimit);
    expect(getMaxLimitSpy).toHaveBeenCalled(); // Verify the mock was called
    getMaxLimitSpy.mockRestore(); // Restore the original function after the test
  });

```
