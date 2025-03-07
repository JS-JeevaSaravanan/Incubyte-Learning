

no async in abstract 


'async' modifier cannot be used with 'abstract' modifier.ts(1243)


Instead of adding `async`, simply define the return type as `Promise<any>`. This forces child classes to implement the method asynchronously.


