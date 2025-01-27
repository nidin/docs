---
description: A randomly accessible sequence of values of a generic type.
---

# Array

## API

The Array API is very similar to JavaScript's \([MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)\), with the notable difference that any operation on an Array of non-nullable reference type values \(like `string`\), that would result in a holey array \(containing `null` values not matching the element type anymore\), results in an error. Example:

```typescript
// The Array constructor implicitly sets `.length = 10`, leading to an array of
// ten times `null` not matching the value type `string`. So, this will error:
var arr = new Array<string>(10);
// arr.length == 10 -> ERROR

// To account for this, the .create method has been introduced that initializes
// the backing capacity normally but leaves `.length = 0`. So, this will work:
var arr = Array.create<string>(10);
// arr.length == 0 -> OK

// When pushing to the latter array or subsequently inserting elements into it,
// .length will automatically grow just like one would expect, with the backing
// buffer already properly sized (no resize will occur). So, this is fine:
for (let i = 0; i < 10; ++i) arr[i] = "notnull";
// arr.length == 10 -> OK
```

### Static

* Array.**isArray**&lt;`U`&gt;\(value: `U`\): `bool` Tests if a value is an array.
* Array.**create**&lt;`T`&gt;\(capacity?: `i32`\): `Array<T>` Creates a new array with the specified pre-allocated backing capacity, but leaves length at zero. Unlike `new Array`, this is safe for non-nullable reference type values because it does not create a holey array.

### Constructor

* new **Array**&lt;`T`&gt;\(capacity?: `i32`\) Constructs a new array. Traps If `T` is a non-nullable reference type and `capacity`is greater than zero because the operation would create a holey array \(means: with `null` values not matching the non-nullable value type anymore, see `Array.create` instead\).

### Instance

#### Fields

* Array\#**length**: `i32` The length of this array. Setting the length to a value larger than internal capacity will automatically grow the array. Traps if `T` is of a non-nullable reference type and setting length would result in a holey array.

#### Methods

* Array\#**concat**\(other: `Array<T>`\): `Array<T>` Concatenates the values of this and the other array to a new array, in this order.
* Array\#**copyWithin**\(target: `i32`, start: `i32`, end?: `i32`\): `this` Copies a region of an array's values over the respective values starting at the target location.
* Array\#**every**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `bool` Calls the specified function with every value of the array until it finds the first value for which the function returns `false`. Returns `true` if all functions returned `true` or the array is empty, otherwise `false`.
* Array\#**fill**\(value: `T`, start?: `i32`, end?: `i32`\): `this` Replaces the values of the array from `start` inclusive to `end` exclusive in place with the specified value, returning the array.
* Array\#**filter**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `Array<T>` Calls the specified function with every value of the array, returning a new array with all values for which the function returned `true`.
* Array\#**findIndex**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `i32` Calls the specified function with every value of the array until it finds the first value for which the function returns `true`, returning its index. Returns `-1` if that's never the case.
* Array\#**forEach**\(fn: `(value: T, index: i32, array: Array<T>) => void`\): `void` Calls the specified function with every value of the array.
* Array\#**includes**\(value: `T`, fromIndex?: `i32`\): `bool` Tests if the array includes the specified value, optionally providing a starting index.
* Array\#**indexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the first index where the specified value can be found in the array. Returns `-1` if not found.
* Array\#**join**\(separator?: `string`\): `string` Concatenates all values of the array to a string, separated by the specified separator \(default: `,`\).
* Array\#**lastIndexOf**\(value: `T`, fromIndex?: `i32`\): `i32` Gets the last index where the specified value can be found in the array. Returns `-1` if not found.
* Array\#**map**&lt;`U`&gt;\(fn: `(value: T, index: i32, array: Array<T>) => U`\): `Array<U>` Calls the specified function with every value of the array, returning a new array of the function's return values.
* Array\#**pop**\(\): `T` Removes and returns the last value of the array. Modifies `Array#length`.
* Array\#**push**\(value: `T`\): `i32` Adds one more value to the end of the array and returns the Array's new length. Modifies `Array#length`.
* Array\#**reduce**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` Calls the specified reducer function with each value of the array, resulting in a single return value. Reducer signature: `(acc: U, cur: T, idx: i32, src: Array) => U`.  The respective previous reducer function's return value is remembered in `acc`, starting with `initialValue`, becoming the final return value in the process.
* Array\#**reduceRight**&lt;`U`&gt;\(fn: `Reducer`, initialValue: `U`\): `U` Calls the specified reducer function with each value of the array, from right to left, resulting in a single return value. See `Array#reduce` for the reducer function's signature.
* Array\#**reverse**\(\): `this` Reverses an array's values in place, modifying the array before returning it.
* Array\#**shift**\(\): `T` Removes and returns the first value of the array. Modifies `Array#length`.
* Array\#**slice**\(start?: `i32`, end?: `i32`\): `Array<T>` Returns a shallow copy of the array's values from `begin` inclusive to `end` exclusive, as a new array. If omitted, `end` defaults to the end of the array.
* Array\#**some**\(fn: `(value: T, index: i32, array: Array<T>) => bool`\): `bool` Calls the specified function with every value of the array until it finds the first value for which the function returns `true`, returning `true`. Returns `false` otherwise or if the array is empty.
* Array\#**sort**\(fn?: `(a: T, b: T) => i32`\): `this` Sorts the values of the array in place, using the specified comparator function, modifying the array before returning it. The comparator returning a negative value means `a < b`, a positive value means `a > b` and `0` means that both are equal. Unlike in JavaScript, where an implicit conversion to strings is performed, the comparator defaults to compare two values of type `T`.
* Array\#**splice**\(start: `i32`, deleteCount?: `i32`\): `Array<T>` Removes `deleteCount` \(defaults to all remaining\) values from the array, starting at index `start`, modifying the array in place, returning the removed values.
* Array\#**toString**\(\): `string` Returns the result of `Array#join()`.
* Array\#**unshift**\(value: `T`\): `i32` Adds one more value to the start of the array and returns the Array's new length. Modifies `Array#length`.

