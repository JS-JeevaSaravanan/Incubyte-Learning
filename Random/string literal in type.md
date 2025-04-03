

```ts
type CustomerCode = `${string}`;
type MarketableLocationId = `${number}`;
type ProviderId = `${number}`;
type LocationId = `${number}`;

// Strictly enforce the structure of BingStoreId
type BingStoreId =
  | `${CustomerCode}_${MarketableLocationId}`          // Format: "ABC_123"
  | `${CustomerCode}_${ProviderId}-${LocationId}`;    // Format: "ABC_45-678"

// Branded type to prevent accidental misuse
declare const brand: unique symbol;
export type BrandedBingStoreId = BingStoreId & { [brand]: "BingStoreId" };

// Function that ensures only valid BingStoreId can be created
export function createBingStoreId(value: string): BrandedBingStoreId {
  if (!/^.+_\d+$/.test(value) && !/^.+_\d+-\d+$/.test(value)) {
    throw new Error("Invalid BingStoreId format");
  }
  return value as BrandedBingStoreId;
}

```


