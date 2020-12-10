### 解题1、
思路：
+ 两次遍历匹配 val1+val2 = target；
+ 这里使用的Array.forEach方法

缺点：
+ 定义多余的变量 result；
+ 匹配到两数之和等于目标值时，无法跳出循环；共遍历：Array.length * Array.length
+ 第二次索引值，获取繁琐 （y1+y2+1）;

耗时：
执行用时：124 ms, 在所有 JavaScript 提交中击败了47.42%的用户
内存消耗：43.6 MB, 在所有 JavaScript 提交中击败了15.89%的用户

```
var twoSum = function(nums, target) {
   const result = [];
   nums.forEach( (x1,y1) =>{**
        nums.slice(y1+1).forEach( (x2,y2) => {
            if( x1 + x2 === target ){ 
                result.push(y1,y1+y2+1)
            }
        } )
   } )
   return result
};
```

### 改进、
思路：
+ 发现Array.forEach的劣势，对比for循环（巩固map、foreach、for循环区别）；
+ 第二次遍历的开始索引值只需要在第一次遍历的索引值之后的即可；遍历次数递减

耗时：
执行用时：76 ms, 在所有 JavaScript 提交中击败了96.52%的用户
内存消耗：38.1 MB, 在所有 JavaScript 提交中击败了74.35%的用户

```
var twoSum = function (nums, target) {
  for(let a = 0;a<nums.length;a++){
      for(let b = a+1;b<nums.length;b++ ){
          if( nums[a] + nums[b]  === target){
              return [a,b]
          }
      }
  }
};
```

### 再次改进、
思路：
+ 如何避免两次循环，具体如下；
遍历数组，记录数组元素构成工程式 x+y=target的需要值;
比如：
一、参数 const arr = [1,2,3,4,5],target = 9;
二、需要一能存储value和索引的数据类型(map)，const map = new Map();
   1+y=9;y=8;
   因为函数返回值是数组的索引，所以map.set(8,0);
   2+y=9;y=7;
   map.set(7,1)
   3+y=9;y=6;
   map.set(6,2)
   ...
3、总结公式 const arr= [x1,x2,x3,x4,...]
   map.set(target-x,index)
4、遍历数组,数组元素存在map的值时，说明满足x+y=target
   具体看代码

耗时：
执行用时：64 ms, 在所有 JavaScript 提交中击败了99.96%的用户
内存消耗：38.2 MB, 在所有 JavaScript 提交中击败了72.54%的用户

```
var twoSum = function(nums, target){
   const map = new Map();
   for(let i=0; i<nums.length; i++){
        if (map.has(nums[i])){
            return [map.get(nums[i]),i];
        }else{
            map.set(target-nums[i],i);
        }
   }
}
```

### 总结for循环、forEach、map

for、forEach、map区别；
#### for循环和forEach区别
+ for 可以使用break；跳出循环，优化性能；forEach报错；
+ for 函数内的for可以使用return；forEach无效；
+ for循环过程中支持修改索引（修改 i），但forEach做不到（底层控制index自增，我们无法左右它）
+ forEach 参数 arr.forEach(function(self,index,arr){ //this 执行obj },obj)

#### map
+ arr.map( function(val,key,arr){},this )
+ 循环内部有能力改变原数组
+ map和forEach处理数组元素的范围是在callback第一次调用就已经确认了；
+ 比如数组的长度--数组新增加的数据是不会被遍历访问；
+ 但是map遍历数组的元素，是在map调用后，访问元素前才生效；如果在此期间删除元素，访问不到
```
var arr = [{name:1},{name:2}]
var bee=arr.map( item=>{
    arr.splice(0,arr.length);
    return item
} )
// index=1的元素访问不到了；
bee [{…}, empty]
//但是扩展运算符可以解决这个问题
var arr = [{name:1},{name:2}]
// [...arr]生成一个新的数组；
var bee=[...arr].map( item=>{
    arr.splice(0,arr.length);
    return item
} )
```