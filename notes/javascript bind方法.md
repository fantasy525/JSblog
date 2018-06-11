记录一下过程
```
Function.prototype.bind=function (context) {
  if(typeof this !=='function'){
    throw new Error(`${this.name} is not a function`)
  }
  const srcFun=this// 保存原始函数
  const arg=Array.prototype.slice.call(arguments,1)// 把arguments类数组转为真实数组
  let noop={}
  const fBound= function () {
   if(this instanceof srcFun){
     context=this
   }
    // 合并新旧参数
    return srcFun.apply(context,arg.concat(Array.prototype.slice.call(arguments,0)))
  }
  noop=Object.create(this.prototype);//克隆原始函数的prototype
  fBound.prototype=noop//新函数的prototype指向要和原函数一致
  return fBound
}
```
很简陋，没有严谨的判断。

需要注意的就是要判断当new 返回的函数的时候要做判断处理也就是
```shagn
//代码2
if(this instanceof srcFun){
     context=this
   }
```
当new 的时候
```
var obj={}
obj._proto_=构造函数的prototype
构造函数.call(obj)
```
所以在代码2中的this要是原始构造函数的实例,那就得更改bind后的函数，让bind后的函数的prototype指向原始函数的prototype即可
这里不能直接指向即：fBound.prototype=srcFun.prototype,因为指向一个对象会互相影响
要用个空对象当个中介

