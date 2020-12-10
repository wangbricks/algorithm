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

资源：
https://www.yuque.com/wanglei-enpro/kcpp9e/wzzhlr
