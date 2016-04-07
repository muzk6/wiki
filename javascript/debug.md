# debug
> 调试

#### 监控变量变化，并下断点
```
// original object
var obj = {someProp: 10};
// save in another property 
JPY = obj.someProp;
// overwrite with accessor 
Object.defineProperty(obj, 'someProp',
        {
            get: function() {
                return JPY;
            },
            set: function(value) {
                debugger;
            }
        }
);
```

#### 变量自动声明
```
if (0) {
    var da = 10;
}
```
*虽然if为假没执行，但是变量da却被声明了，为undefined，只有没被声明才被报错并且为null，因此后面调用da这变量时不会报错！！* 