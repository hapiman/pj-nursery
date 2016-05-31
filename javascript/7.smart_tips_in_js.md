### 1.使用+将字符串转化为数字
备注：不过其只适合用于字符串数据，否则将返回NaN
```javascript
function toNumber(strNumber) {
    return +strNumber;
}
console.log(toNumber("1234")); // 1234
console.log(toNumber("ACB")); // NaN

console.log(+new Date()) // 1461288164385
```

### 2.合并数组
方法一：
```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.concat(array2)); // [1,2,3,4,5,6];
```
`concat`并不适合合并两个大型数组，因为这将消耗大量的内存来创建新的数组，可以使用`Array.push().apply(arr1,arr2)`来替代创建一个新的数组，因为这只是将两个数组合并在一起，同时减少内存的使用。
```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.push.apply(array1, array2)); // [1,2,3,4,5,6];
```
### 3.将NodeList转化为数组
如果你运行`document.querySelectorAll(“p”)`函数时，它可能返回DOM元素的数组，也就是NodeList对象。但这个对象不具有数组的函数功能，比如`sort()`、`reduce()`、`map()`、`ilter()`等。为了让这些原生的数组函数功能也能用于其上面，需要将节点列表转换成数组。可以使用`[].slice.call(elements)`来实现：
```javascript
var elements = document.querySelectorAll("p"); // NodeList
var arrayElements = [].slice.call(elements); // Now the NodeList is an array
var arrayElements = Array.from(elements); // This is another way of converting NodeList to Array
```
### 4.对数组元素的洗牌
```javascript
var list = [1,2,3];
console.log(list.sort(function() { Math.random() - 0.5 })); // [2,1,3]
```
