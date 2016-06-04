* 递归查看某个对象中是否有某个属性
```js
//方式一
function find(obj,key){
  if(! typeof obj === 'object') return false;
  if(key in obj) return true;
  for(var k in  obj){
    if(find(obj[k],key))
      return true;
  }
  return false;
}
//方式二
function containsKey(target,key){
  if(!(typeof target ==='object')|| !(typeof key === 'string'))
    return false;

  return Object.keys(target).some(function(k){return k===key} || containsKey(target[k], key));
}
```
