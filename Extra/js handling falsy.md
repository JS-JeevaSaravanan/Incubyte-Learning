
```js
console.log(null ?? 1) //=> 1
console.log(undefined ?? 1)//=> 1
console.log(0 ?? 1)//=> 0

const func1 = (h=1) => {
  console.log(h)
}

func1(null)//=> null
func1(undefined)//=> 1
func1(0)//=> 0

const val1 = { val:null }
const val2 = { val:undefined }
const val3 = { val:0 }
const val4 = {}

const { val:v1= 1 } = val1;
const { val:v2 = 1 } = val2;
const { val:v3 = 1 } = val3;
const {val:v4 = 1 } = val4;

console.log(v1)
console.log(v2)
console.log(v3)
console.log(v4)

/*
summary : 
?? => overrides both undefined and null
default arg (destructure & func param )=> override only undefined
*/
```



