一 let和const
1.let
(1)一个大括号就是一个块级作用域，let声明的变量只在自己作用域有效；
(2)es6强制开启严格模式，变量未声明不能引用，所以会报 Uncaught ReferenceError

```javascript
function test() {
  for (let i = 1; i < 3; i++) {
    console.log(i)
  }
  console.log(i);  // Uncaught ReferenceError: i is not defined
}
test();
```
(3)let不能重复声明

```javascript
function test() {

  let a = 1; 

  let a = 2;

}

test();
```

(4)let不存在变量提升(这个地方有问题)

```javascript
// var 的情况

console.log(a); // 输出undefined

var a = 2;

// let 的情况

console.log(b); // 报错ReferenceError

let b = 2;
```

2.const
(1)const声明之后必须赋值，否则会编译不通过；
(2)const声明的值不允许修改；

```javascript
const PI = 3.14;

// PI = 2;  

// const PI;

console.log(PI);
```

(3)const如果是对象的话，可以向对象中添加属性，也可以修改a的属性；json是指向内存地址的一个指针，指针的指向不变，但是那个被json指针所指向的内存地址所存储的内容是可以变化的；

```javascript
const json = {

  a: 2

}

json.a = 3;

json.b = 3;

console.log(json.a)   //3

console.log(json.b)   //3
```

二 解构赋值
1.基本用法
先上两个例子了解什么是解构赋值

```javascript
{

  let a, b, rest;

  [a, b, rest] = [1, 2];

  console.log(a, b, rest);   //1 2 undefined

}

{
  let a, b, rest;

  [a, b, ...rest] = [1, 2, 3, 4, 5, 6, 7];

  console.log(a, b, rest);   //1 2 [3, 4, 5, 6, 7]
}
```

2.对象的解构赋值

```javascript
{

  let a, b;

  ({ a, b } = { a: 1, b: 2 });  //a，b 顺序不影响其结构结果

  console.log(a, b); // 1 2

}
```


3.默认值

```javascript
{

  let a, b, rest;

  [a, b, rest = 3] = [1, 2];

console.log(a, b, rest); // 1 2 3

}
```
4.实际应用
变量的交换

```javascript


{

  let a = 1;

  let b = 2;

  [a, b] = [b, a];

  console.log(a, b);  //2 1

}
```

接收函数返回的值

```javascript
{

  function f() {

    return [12, 13];

  }

  let a, b;

  [a, b] = f();

  console.log(a, b); //12 13

}

{

  function f() {

    return [12, 13, 14, 15, 16];

  }

  let a, b;

  [a, , , b] = f();  //函数返回多个值，可以选择性的接收对应的值

  console.log(a, b); // 12 16

}

{

  function f() {

    return [12, 13, 14, 15, 16];

  }

  let a, b;

  [a, , ...b] = f();  //取出对应的值，其他的值可以直接赋值给数据

  console.log(a, b); // 12 [14, 15, 16]

}

```

5.对象的解构赋值的应用

```javascript
{

  let o = { p: 42, q: true };

  let { p, q } = o;

  console.log(p, q); //42 true

}

{

  let { a = 10, b = 11 } = { a: 3 }  // 对象的默认值更改

  console.log(a,b); // 3, 11

}
```

6.解构赋值的简单应用举例

```javascript
{

  let metaData = {

    title: 'abc',
    test: [{
      title: 'gao',
      desc: 'description'
    }]

  }

  let { title: esTitle, test: [{ title: cnTitle }] } = metaData;

  console.log(esTitle, cnTitle);

}
```

三 正则的扩展
1.构造函数来创建正则

```javascript
{

  let regex1 = new RegExp('xyz', 'i');

  let regex2 = new RegExp(/xyz/i);

  console.log(regex1.test('xyz123'), regex2.test('xyz123')); // true true

  let regex3 = new RegExp(/xyz/ig, 'i'); // 后面的修饰符会把前面的修饰符给覆盖掉

  console.log(regex3.flags);  // es6新增的，用来获取正则表达式的修饰符

}
```


2.g修饰符和y修饰符
y修饰符的作用与g修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个位置开始。不同之处在于，g修饰符只要剩余位置中存在匹配就可，而y修饰符确保匹配必须从剩余的第一个位置开始。

```javascript
{

  let s = 'bbb_bb_b';

  let a1 = /b+/g; // g只要匹配到都算

  let a2 = /b+/y; // y必须是下一个开始的字母开始匹配

  console.log('one', a1.exec(s), a2.exec(s)); // g修饰符匹配到都可以，y修饰符必须从第一个开始匹配，如果一第个不是b则会输出null

  console.log('two', a1.exec(s), a2.exec(s)); // 第二次匹配，g修饰符会只要匹配到都可以，y修饰符必须从紧邻的下一个字符开始匹配

  console.log(a1.sticky, a2.sticky); // 判断是否开启了y修饰符   false true

}

one和two的输出结果
```

3.u修饰符(unicode)
ES6 对正则表达式添加了u修饰符，含义为“Unicode模式”，用来正确处理大于uFFFF的 Unicode 字符。

```javascript
{

  console.log('u-1', /^\uD83D/.test('\uD83D\uDC2A')); // 不加u把后面的四个字节当成两个字符

  console.log('u-2', /^\uD83D/u.test('\uD83D\uDC2A')); // 加u把后面的4个字节当作一个字符

  console.log(/\u{61}/.test('a'));  // false 大括号括起来代表一个unicode字符，所以必须加u才能识别

  console.log(/\u{61}/u.test('a')); // true

  console.log(\u{20BB7});

  let s = '?';

  console.log('u-1', /^.$/.test(s));  //false 字符串大于两个字节，必须加u修饰符才能匹配到

  console.log('u-2', /^.$/u.test(s)); //true

  console.log('test-1', /?{2}/.test('??')); // false

  console.log('test-2', /?{2}/u.test('??')); // true

}
```

四 字符串扩展
1.unicode的表示方法

```javascript
{

  console.log('a', '\u0061'); // a a

  console.log('s', '\u20BB7'); // s ₻7  把前两个字节当作一个整体

  console.log('s', '\u{20BB7}'); // s ?  unicode编码用{}可以正常识别

}
```

2.codePointAt和charCodeAt的对比
对于4个字节的字符，JavaScript不能正确处理，字符串长度会误判为2，而且charAt方法无法读取整个字符，charCodeAt方法只能分别返回前两个字节和后两个字节的值。ES6提供了codePointAt方法，能够正确处理4个字节储存的字符，返回一个字符的码点。

```javascript
{

   let s = '?';

   console.log(s.length);  // 2

   console.log('0', s.charAt(0));  // 0 �   //es5未对多个字节的字符做处理

   console.log('1', s.charAt(1));  // 1 �

   console.log('at0', s.charCodeAt(0));  //at0 55362

   console.log('at1', s.charCodeAt(1));  //at1 57271

   let s1 = '?a';

   console.log('length', s1.length); // 3

   console.log('code0', s1.codePointAt(0)); // code0 134071

   console.log('code0', s1.codePointAt(0).toString(16));  // code0 es6会自动把多个字节的字符当作一个整体来处理 

   console.log('code1', s1.codePointAt(1)); // code1 57271

   console.log('code2', s1.codePointAt(2)); // code2 97

}
```

3.fromCharCode和fromCodePoint
ES5提供String.fromCharCode方法，用于从码点返回对应字符，但是这个方法不能识别Unicode编号大于0xFFFF。ES6提供了String.fromCodePoint方法，可以识别大于0xFFFF的字符，弥补了String.fromCharCode方法的不足。在作用上，正好与codePointAt方法相反。注意，fromCodePoint方法定义在String对象上，而codePointAt方法定义在字符串的实例对象上。

```javascript
{

  console.log(String.fromCharCode('0x20bb7'));  //ஷ

  console.log(String.fromCodePoint('0x20bb7'))  //?

}
```

4.字符串遍历器

```javascript
{

  // es5

  let str = '\u{20bb7}abc';

  for (let i = 0; i < str.length; i++) {

    console.log('es5', str[i]);

    //� � a b c   

  }

  //es6

  for (let code of str) {

    console.log('es6', code);

    // ? a b c

  }

}
```

5.一些常用的字符串api

```javascript
{

  let str = 'string';

  console.log('includes', str.includes('c'));  // 判断是否包含  false

  console.log('start', str.startsWith('s'));   // 以什么开头  true

  console.log('end', str.endsWith('ng'));   // 以什么结尾   true

  console.log('repeat', str.repeat(2));     // 字符串重复两次  stringstring

}
```

ES6 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。padStart()用于头部补全，padEnd()用于尾部补全。如果原字符串的长度，等于或大于指定的最小长度，则返回原字符串。如果用来补全的字符串与原字符串，两者的长度之和超过了指定的最小长度，则会截去超出位数的补全字符串。

```javascript
{

    console.log('1'.padStart(2,'0')); // 01
    console.log('1'.padEnd(2,'0')); // 10

}
```

6.模板字符串

```javascript
{

  let name = "List";

  let info = "hello world";

  let m = i am ${name} ${info};

  console.log(m);  //i am List hello world

}
```

7.标签模板

```javascript


{

  let user = {

    name:'list',
    info:'hello world'

  }

  function fn(s,v1,v2){

    console.log(s,v1,v2);
    return s+v1+v2;

  }

  console.log(fni am ${user.name} ${user.info})  // ``符号相当于一个函数的参数fn(i am {user.name} {user.info});

}

输出结果
```

8.String.row API
ES6还为原生的String对象，提供了一个raw方法。String.raw方法，往往用来充当模板字符串的处理函数，返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，对应于替换变量后的模板字符串。

```javascript
{

  console.log('raw '+String.rawhi\n${1+2})

  console.log('noRaw '+hi\n${1+2})

}
```

输出结果

五 数值扩展
1.二进制八进制表示法
从 ES5 开始，在严格模式之中，八进制就不再允许使用前缀0表示，ES6进一步明确，要使用前缀0o表示。如果要将0b和0o前缀的字符串数值转为十进制，要使用Number方法。

```javascript
{

  console.log('B',0b11010101010);  //二进制表示，b大小写都可以

  console.log('O',0O1237637236);  // 八进制表示法

}
```

2.Number.isFinite()和Number.isNaN()
Number.isFinite()用来判断数字是否有限（无尽小数），Number.isNaN()来判断一个数是不是小数

```javascript
{

  console.log('15',isFinite(15));    //true

  console.log('NaN',isFinite(NaN));  //false

  console.log('1/0',isFinite(1/0));  //false

  console.log('isNaN',Number.isNaN(15)); // false

  console.log('isNaN',Number.isNaN(NaN));  // true

}
```

3.Number.isInteger
Number.isInteger用来判断一个数是不是整数

```javascript
{

  console.log('13',Number.isInteger(13));      // true

  console.log('13.0',Number.isInteger(13.0));  // true 

  console.log('13.1',Number.isInteger(13.1));  //false

  console.log('13',Number.isInteger('13'));    // false

}
```

4.Number.MAX_SAFE_INTEGER,Number.MIN_SFAE_INTEGER和isSafeInterger
Number.MAX_SAFE_INTEGER,Number.MIN_SFAE_INTEGER表示js可以准确表示的值的范围，isSafeInterger用来判断这个值是否在安全范围内。

```javascript
{

  console.log(Number.MAX_SAFE_INTEGER,Number.MIN_SFAE_INTEGER);

  console.log('15',Number.isSafeInteger(15));

  console.log('9999999999999999999999',Number.isSafeInteger(9999999999999999999999));

}
```

5.Math.trunc和Math.sign
Math.trunc方法用于去除一个数的小数部分，返回整数部分。Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

```javascript
{

  console.log('4.1',Math.trunc(4.1));   //4

  console.log('4.9',Math.trunc(4.9));   //4

}

{

  console.log('-5',Math.sign(-5))    //-1

  console.log('5',Math.sign(5))      //+1

  console.log('0',Math.sign(0))      //0

  console.log('50',Math.sign(50))    //+1

  console.log('NaN',Math.sign(NaN))  //NaN

} 
```

6.cbrt
cbrt用来计算一个数的开方

```javascript
{

  console.log('-1',cbrt(-1));   //-1

  console.log('8',cbrt(8));     //2

}
```

六 数组扩展
1. Array.of
   Array.of方法用于将一组值，转换为数组,这个方法的主要目的，是弥补数组构造函数Array()的不足。因为参数个数的不同，会导致Array()的行为有差异。

   ```javascript
   {

     let arr = Array.of(1,2,3,4);

     console.log('arr=',arr);  // arr= [1, 2, 3, 4]

     let emptyArr = Array.of();

     console.log(emptyArr);  // []

     //与Array方法对比

     Array() // []

     Array(3) // [, , ,]

     Array(3, 11, 8) // [3, 11, 8]

   }
   ```

2.Array.from
Array.from方法用于将两类对象转为真正的数组：类似数组的对象和可遍历的对象（包括ES6新增的数据结构Set和Map）。

```javascript
<p>你好</p>

<p>我好</p>

<p>大家好</p>

{

  let p = document.querySelectorAll('p');

  let pArr = Array.from(p);

  pArr.forEach(function(item){

    console.log(item.textContent);  // 你好 我好 大家好

  })

  console.log(Array.from([1,3,5],function(item){return item*2})) // [2,6,10]

}
```

3.Array.fill
fill方法使用给定值，填充一个数组。

```javascript
{

  console.log('fill-7',[1,3,'undefined'].fill(7));   //[7,7,7]

  console.log('fill,pos',[1,2,3,4,5,7,8].fill(7,1,4)); //[1, 7, 7, 7, 5, 7, 8]  // 后两个参数表示索引的位置

}
```

4.entries()，keys() 和 values()
ES6 提供三个新的方法——entries()，keys()和values()——用于遍历数组。

```javascript
{

  for(let index of [1,2,3,4].keys()){

    console.log('index',index);

   // index 0

   // index 1

   // index 2

   // index 3

  }

  for(let value of [1,2,3,4].values()){

    console.log('value',value);

   // value 1

   // value 2

   // value 3

   // value 4

  }

  for(let [index,value] of [1,2,4,5,6].entries()){

    console.log(index,value);

   // 0 1

   // 1 2

   // 2 4

   // 3 5

   // 4 

  }

}
```

5.Array.copyWithin
截取一定长度的数字并且替换在相对应的索引的位置

```javascript
{

  console.log([1,4,9,6,7,2,3].copyWithin(1,3,5));  //  [1, 6, 7, 6, 7, 2, 3]  // 截取3-5的位置的数字，从索引1的位置开始替换

  console.log([1,4,9,6,7,2,3].copyWithin(1,3,6));  //  [1, 6, 7, 2, 7, 2, 3] 

}
```

6.findIndex和find
数组实例的find方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为true的成员，然后返回该成员。如果没有符合条件的成员，则返回undefined。数组实例的findIndex方法的用法与find方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

```javascript
{

  console.log([1,2,3,4,5,6].find(function(item){return item > 3}));   //4

  console.log([1,2,3,4,5,6].findIndex(function(item){return item > 3}));   // 3

}
```

7.includes
Array.prototype.includes方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的includes方法类似。ES2016 引入了该方法。

```javascript
{

  console.log([1,2,NaN].includes(1));  // true

  console.log([1,2,NaN].includes(NaN));  // true

}
```

8.扩展运算符
扩展运算符（spread）是三个点（...）。将一个数组转为用逗号分隔的参数序列。

```javascript
console.log(...[1, 2, 3])

// 1 2 3

console.log(1, ...[2, 3, 4], 5)

// 1 2 3 4 5

[...document.querySelectorAll('div')]

// [<div>, <div>, <div>]
```

七 函数扩展
1.默认值
ES6 之前，不能直接为函数的参数指定默认值;ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。

```javascript
{

    function fn(x,y='hello'){  // 默认值后面不能再出现形参
        console.log(x,y);
    }
    fn('word');  // word hello
    fn('word','nihao')  // word nihao

}
{

    let a = 'nihao';
    function test(a,b=a){  //1.
        //let a = 1; 参数变量是默认声明的，所以不能用let或const再次声明
        console.log(a,b);
    }
    test('word'); // word word  
    test();  //undefined undefined

}
{

    let a = 'nihao';
    function test(x,b=a){  //2.
        console.log(x,b)
    }
    test('hello');// hello nihao

}
```

3.rest参数
ES6 引入rest参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

```javascript
{

    function fn(...arg){
        for(let v of arg){
            console.log(v);
        }
    }
    fn(1,2,3,4);
    //1
    //2
    //3
    //4

}
{

    console.log(...[1,2,3,4]);  // 1，2，3，4
    console.log('a',...[1,2,3,4]); // a,1,2,3,4

}
```

4.箭头函数
ES6 允许使用“箭头”（=>）定义函数。

```javascript
{

    let arr = v => v*2;
    console.log(arr(2));
    
    var sum = (num1, num2) => { return num1 + num2; } //如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。

}
```

使用注意点
箭头函数有几个使用注意点。

（1）函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

（3）不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

5.绑定 this
函数绑定运算符是并排的两个冒号（::），双冒号左边是一个对象，右边是一个函数。该运算符会自动将左边的对象，作为上下文环境（即this对象），绑定到右边的函数上面。

```javascript
foo::bar;

// 等同于

bar.bind(foo);

foo::bar(...arguments);

// 等同于

bar.apply(foo, arguments);

const hasOwnProperty = Object.prototype.hasOwnProperty;

function hasOwn(obj, key) {

  return obj::hasOwnProperty(key);

}
```

尾调用（Tail Call）是函数式编程的一个重要概念，本身非常简单，一句话就能说清楚，就是指某个函数的最后一步是调用另一个函数。

```javascript
{

    function fn1(x){
        console.log('fn1',x);
    }
    function fn2(x){
        return fn1(x);  // 对fn1的调用必须在最后一步操作
    }
    fn2(2);

}
```

八 对象扩展
1.属性的简介表示法
ES6 允许直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```javascript
{

    let a = 5,b=6;
    let es5 = {
        a:a,
        b:b
    }
    let es6 = {
        a,
        b
    }
    console.log(es5,es6)  // {a: 5, b: 6}  {a: 5, b: 6}

    let es5_fn = {   // 
        fn:function(){
            console.log('hello')
        }
    }
    let es6_fn = {
        fn(){
            console.log('hello')
        }
    }
    console.log(es5_fn.fn,es6_fn.fn);

}
```

2.动态key值
es6允许属性的key值是动态的变量

```javascript
{

    let a = 'b';
    let es5_obj = {
        a:'c',
        b:'c'
    }
    let es6_obj = {
        [a]:'c'   // a是动态的变量，可以自由赋值
    }
    console.log(es5_obj, es6_obj);

}
```

3.Object.is
这个方法相当于es5 中的 ===，来判断属性是否相等

```javascript
{

    console.log('is',Object.is('a','a'));  // true
    console.log('is',Object.is([],[]));    // false   数组对象拥有不同的地址，

}
```

4.Object.assign
Object.assign方法用于对象的合并，将源对象的所有可枚举属性，复制到目标对象。

```javascript
{

    console.log('拷贝',Object.assign({a:1},{b:2}));  //浅拷贝

    let test = {a:2,b:3}
    for(let [key,value] of Object.entries(test)){   // 遍历
        console.log([key,value]); 
        //[a:2]
        //[b:3]
    }

}
```

九 Symbol
1.Symbol简单举例
ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。

```javascript
{

    let a1 = Symbol();
    let a2 = Symbol();
    console.log(a1===a2)   // false
    
    let a3 = Symbol.for('a3');
    let a4 = Symbol.for('a3');
    
    console.log(a3===a4);  //true

}
```

2.Symbol的一些API
Symbol.for可以用来命名具有相同的key值的对象。
Object.getOwnPropertySymbols方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。
Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。

```javascript
{

    let a1 = Symbol.for('abc');
    let obj = {
        [a1]:123,
        abc:234,
        c:345
    }
    console.log(obj); 
    // abc:234
    // c:345
    // Symbol(abc):123
    
    Object.getOwnPropertySymbols(obj).forEach(function(item){
        console.log('symbol',item,obj[item]); //symbol Symbol(abc) 123
    })
    Reflect.ownKeys(obj).forEach(function(item){
        console.log(item,obj[item]); 
        //abc 234
        //c 345
        //Symbol(abc) 123
    })

}
```

十 Map和Set数据结构
1.set的基本用法
ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。Set 本身是一个构造函数，用来生成 Set 数据结构。 Set 结构不会添加重复的值

```javascript
{

    let list = new Set();
    list.add(2);
    list.add(3);
    console.log(list.size);  //2
    
    let arr = [1,2,3,4,5];
    let list2 = new Set(arr);
    console.log(list2.size); //5
    
    console.log(list2) //{1, 2, 3, 4, 5}
    
    let arr2 = [1,2,3,4,2,1];   //这里可以当作数组去重
    let list3 = new Set(arr2);
    console.log(list3) //{1, 2, 3, 4}

}
```

add(value)：添加某个值，返回Set结构本身。
delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
has(value)：返回一个布尔值，表示该值是否为Set的成员。
clear()：清除所有成员，没有返回值。

```javascript
{

    let arr = ['add','delete','clear','has'];
    let list = new Set(arr);
    console.log(list); // {"add", "delete", "clear", "has"}
    
    list.delete('add');
    console.log(list); // {"delete", "clear", "has"}
    
    console.log(list.has('clear')); // true
    
    list.clear();  
    console.log(list); //{}
    //set遍历方法
    {
        let arr = ['add','delete','clear','has'];
        let list = new Set(arr);
    
        for(let key of list.keys()){
            console.log('keys',key)
            //keys add
            //keys delete
            //keys clear
            //keys has
        }
        for(let value of list.values()){
            console.log('values',value)
            //values add
            //values delete
            //values clear
            //values has
        }
        for(let [key,value] of list.entries()){
            console.log(key,value);
            //add add
            //delete delete
            //clear clear
            //has has
        }
        list.forEach(function(item){console.log(item)})
           // add
           // delete
           // clear
           // has
    }

}
```

2.WeakSet基本用法
WeakSet结构与Set类似，也是不重复的值的集合。但是，它与 Set有两个区别。首先，WeakSet 的成员只能是对象，而不能是其他类型的值。
WeakSet中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。
WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在

```javascript
{

    const ws = new WeakSet();
    ws.add(1)
    // TypeError: Invalid value used in weak set
    ws.add(Symbol())
    // TypeError: invalid value used in weak set
    
    let weakset = new WeakSet()  // 没有clear，set方法，不能遍历
    let obj = {}   
    weakset.add(obj)
    // weekset.add(2)  WeakSet必须添加的是对象，弱引用   
    console.log(weakset);

}
```

3.Map的基本用法
ES6 提供了 Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object结构提供了“字符串—值”的对应，Map结构提供了“值—值”的

```javascript
{

    const map = new Map([
      ['name', '张三'],
      ['title', 'Author']
    ]);
    
    map.size // 2
    map.has('name') // true
    map.get('name') // "张三"
    map.has('title') // true
    map.get('title') // "Author"

}

{

    let map = new Map();
    let arr = ['123'];
    map.set(arr,'456');
    console.log(map,map.get(arr)) // {["123"] => "456"} "456"

}

{

    let map = new Map([['a',123],['b',456]])
    console.log(map);                      //{"a" => 123, "b" => 456}
    console.log(map.size);              //2
    console.log('123'+map.delete('a')); //true
    console.log(map)                      // {"b" => 456}
    map.clear()
    console.log(map);                    //{}

}
```

4.WeakMap的一些API
WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
WeakMap的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
WeakMap 与 Map 在 API 上的区别主要是两个，一是没有遍历操作（即没有key()、values()和entries()方法），也没有size属性。因为没有办法列出所有键名，某个键名是否存在完全不可预测，跟垃圾回收机制是否运行相关。这一刻可以取到键名，下一刻垃圾回收机制突然运行了，这个键名就没了，为了防止出现不确定性，就统一规定不能取到键名。二是无法清空，即不支持clear方法。因此，WeakMap只有四个方法可用：get()、set()、has()、delete()。

```javascript
{

    let weakmap = new WeakMap() //没有clear，set方法，不能遍历
    let o = {}
    weakmap.set(o,123);
    console.log(weakmap.get(o));

}
```

十一 proxy和reflect
1.Proxy
Proxy用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```javascript
{

  let obj = {

    name:'gao',
    time:'2017-08-13',
    emp:'123',

  }

  let temp = new Proxy(obj,{

    get(target,key){
          return target[key].replace('2017','2018');
    },
    set(target,key,value){
      if(key === 'name'){
        return target[key] = value;
      }else{
        return target[key];
      }
    },
    has(target,key){
      if(key === 'name'){
        return target[key];
      }else{
        return false;
      }
    },
    deleteProperty(target,key){
      if(key.indexOf('i') > -1){
        delete target[key];
        return true;
      }else{
        return target[key];
      }
    },
    ownKeys(target){
      return Object.keys(target).filter(item=>item!='name');
    }

  })

  console.log('get',temp.time);  //get 2018-08-13

  temp.time = '2018';

  console.log('set',temp.name,temp); //set gao   {name: "gao", time: "2017-08-13", temp: "123"}

  temp.name = 'he';

  console.log('set',temp.name,temp); // set he  {name: "he", time: "2017-08-13", temp: "123"}

  console.log('has','name' in temp,'time' in temp);  //has true false

  delete temp.time;

  console.log('delete',temp);   //delete  {name: "he", temp: "123"}

  console.log('ownkeys',Object.keys(temp));  //["emp"]

}
```

2.Reflect
Reflect对象与Proxy对象一样，也是 ES6 为了操作对象而提供的新 API。Reflect对象的设计目的有这样几个。
（1） 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法。
（2） 修改某些Object方法的返回结果，让其变得更合理。比如，Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false。
（3） 让Object操作都变成函数行为。某些Object操作是命令式，比如name in obj和delete obj[name]，而Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)让它们变成了函数行为。
（4）Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，你总可以在Reflect上获取默认行为。

```javascript
{

  let obj = {

    name:'gao',
    time:'2017-08-13',
    emp:'123',

  }

  console.log('reflect get',Reflect.get(obj, 'name'));  // reflect get gao

  Reflect.set(obj,'name','hexaiofei');

  console.log(obj);  // {name: "hexaiofei", time: "2017-08-13", emp: "123"}

  console.log('reflect has', Reflect.has(obj,'name'));  //reflect has true

}
```

3.简单应用

```javascript
{

  function validator(target,validator) {

    return new Proxy(target,{
      _validator:validator,
      set(target,key,value,proxy){
        if(target.hasOwnProperty(key)){
          let va = this._validator[key];
          if(!!va(value)){
            return Reflect.set(target,key,value,proxy);
          }else{
            throw Error(`不能设置${key}到${value}`);
          }
        }else{
          throw Error(`${key}不存在`);
        }
      }
    })

  }

  const personValidators={

    name(value){
      return typeof value === 'string'
    },
    age(value){
      return typeof value === 'number' && value > 18;
    }

  }

  class Person{

    constructor(name,age) {
      this.name = name;
      this.age = age;
      return validator(this,personValidators)
    }

  }

  const person = new Person('lilei',30);

  console.log(person);

  person.name = 48;

}
```

十二 Class的基本语法
1.简介
ES6 提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类。基本上，ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 

  }

  let v_parent = new Parent();

  console.log(v_parent);  //{name: "gao"}

}
```

2.继承
Class可以通过extends关键字实现继承，这比ES5的通过修改原型链实现继承，要清晰和方便很多。

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 

  }

  class child extends Parent {

  }

  let v_child = new child();

  console.log(v_child);  //{name: "gao"}

}
```

3.constructor
constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。

4.super关键字
super这个关键字，既可以当作函数使用，也可以当作对象使用。在这两种情况下，它的用法完全不同。第一种情况，super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。第二种情况，super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。super()在子类constructor构造方法中是为了获取this上下文环境,所以如果在constructor中使用到this,必须在使用this之前调用super(),反之不在constructor中使用this则不必调用super()

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 

  }

  class child extends Parent {

    constructor(name='child'){
      super(name);
      this.type = 'child'
    }

  }

  let v_child = new child();

  console.log(v_child);  //{name: "child", type: "child"}

}
```

5.getter和setter
与 ES5 一样，在“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 
    get longName(){
      return 'mk' + this.name;
    }
    set longName(value){
      // console.log(value);
      this.name = value;
    }

  }

  let v_parent = new Parent();

  console.log('get',v_parent.longName);  //get mkgao

  v_parent.longName = 'hello';

  console.log('get',v_parent.longName);  //get mkhello

}
```

6.静态方法
类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 
    static tell(){
      console.log('tell');
    }

  }

  let v_parent = new Parent();

  console.log(v_parent);  //{name: "gao"}

  Parent.tell(); // tell

}
```

7.静态属性
静态属性指的是Class本身的属性，即Class.propName，而不是定义在实例对象（this）上的属性。

```javascript
{

  class Parent {

    constructor(name='gao') {
      this.name = name;
    } 

  }

  Parent.tell = 'nihao';

  let v_parent = new Parent();

  console.log(v_parent);  //{name: "gao"}

  console.log(Parent.tell);   // nihao

}
```

十三 Promise
Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。它由社区最早提出和实现，ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。所谓Promise，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
Promise对象有以下两个特点。
（1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Fulfilled（已成功）和Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从Pending变为Fulfiled和从Pending变为Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 Resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

注意，为了行文方便，本章后面的Resolved统一只指Fulfilled状态，不包含Rejected状态。

有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。

Promise也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

如果某些事件不断地反复发生，一般来说，使用 Stream 模式是比部署Promise更好的选择。

1.基本用法
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

Promise实例生成以后，可以用then方法分别指定Resolved状态和Rejected状态的回调函数。

```javascript
// ES5的回调函数

{

  let ajax = function(callback){

    console.log('nihao');
    setTimeout(function(){
      callback && callback.call()
    },1000)

  }

  ajax(function(){

    console.log('timeout1');

  })

}

// es6 Promise的用法

{

  let ajax = function(){

    console.log('wohao');
    return new Promise((resolve, reject) => {
      setTimeout(function(){
        resolve();
      },1000);
    });

  }

  ajax().then(function(){

    console.log('promise','timeout1');

  })

}

promise.then(function(value) {   // promise的用法

  // success

}, function(error) {

  // failure

});
```

2.Promise.prototype.then()
Promise实例具有then方法，也就是说，then方法是定义在原型对象Promise.prototype上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，then方法的第一个参数是Resolved状态的回调函数，第二个参数（可选）是Rejected状态的回调函数。
then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。

```javascript
{

  let ajax = function(){

    console.log('dajiahao');
    return new Promise((resolve, reject) => {
      setTimeout(function(){
        resolve();
      },1000);
    });

  };

  ajax().then(function(){

    return new Promise((resolve, reject) => {
      setTimeout(function(){
        resolve();
      },2000)
    });

  })

  .then(function(){

    console.log('timeout3');

  })

}
```

3.Promise.prototype.catch()
Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。

```javascript
{

  let ajax = function(num){

    console.log('dajiahao');
    return new Promise((resolve, reject) => {
      if(num>6){
        console.log('6');
      }else{
        throw new Error('出错了');
      }
    });

  };

  ajax(3).then(function(){

    console.log('3');

  })

  .catch(error=>{

    console.log(error)   //出错了

  })

}
```


4.Promise.all
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

var p = Promise.all([p1, p2, p3]);
上面代码中，Promise.all方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。（Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。）

p的状态由p1、p2、p3决定，分成两种情况。

（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

```javascript
{

  function loadImg(src){

    return new Promise((resolve, reject) => {
      let img = document.createElement('img');
      img.src=src;
      img.onload = function(){
        resolve(img);
      }
      img.onerror = function(error){
        reject(error);  
      }
    });

  }

  function showImgs(imgs){

    imgs.forEach(function(img){
      document.body.appendChild(img);
    })

  }

  Promise.all([

    loadImg(''),
    loadImg(''),
    loadImg(''),

  ]).then(showImgs)

}
```

4.Promise.race
Promise.race方法同样是将多个Promise实例，包装成一个新的Promise实例。

var p = Promise.race([p1, p2, p3]);
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
Promise.race方法的参数与Promise.all方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。
下面是一个例子，如果指定时间内没有获得结果，就将Promise的状态变为reject，否则变为resolve。

```javascript
{

  function loadImg(src){

    return new Promise((resolve, reject) => {
      let img = document.createElement('img');
      img.src=src;
      img.onload = function(){
        resolve(img);
      }
      img.onerror = function(error){
        reject(error);  
      }
    });

  }

  function showImg(img){

    let img = document.createElement('p');
    p.appendChild(img);
    document.body.appendChild(p);

  }

  Promise.race([

    loadImg(''),
    loadImg(''),
    loadImg(''),

  ]).then(showImgs)

}
```

十四 Iterator 和 for...of 循环
Iterator 接口的目的，就是为所有数据结构，提供了一种统一的访问机制，即for...of循环。当使用for...of循环遍历某种数据结构时，该循环会自动去寻找 Iterator 接口。一种数据结构只要部署了 Iterator 接口，我们就称这种数据结构是”可遍历的“（iterable）。
ES6 规定，默认的 Iterator 接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）。Symbol.iterator属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名Symbol.iterator，它是一个表达式，返回Symbol对象的iterator属性，这是一个预定义好的、类型为 Symbol的特殊值，所以要放在方括号内。

1.数组的Symbol.iterator属性
变量arr是一个数组，原生就具有遍历器接口，部署在arr的Symbol.iterator属性上面。所以，调用这个属性，就得到遍历器对象。

```javascript
{

  let arr = ['hellow','world'];

  let map = arrSymbol.iterator;

  console.log(map.next());  //{value: "hellow", done: false}

  console.log(map.next());  //{value: "world", done: false}

  console.log(map.next());  //{value: "undefined", done: false}

}
```

2.自定义的Iterator接口

```javascript
{

  let obj = {

    start:[1,3,2],
    end:[7,8,9],
    [Symbol.iterator](){
      let self = this;
      let index = 0;
      let arr = self.start.concat(self.end);
      let len = arr.length;
      return {
        next(){
          if(index<len){
            return {
              value:arr[index++],
              done:false
            }
          }else{
            return {
              value:arr[index++],
              done:true
            }
          }
        }
      }
    }

  }

  for(let key of obj){

    console.log(key); //1 3 2 7 8 9

  }

}
```

十五 Genertor
1.基本概念
Generator 函数有多种理解角度。从语法上，首先可以把它理解成，Generator函数是一个状态机，封装了多个内部状态。执行 Generator 函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。

```javascript
{

  let tell = function* (){

    yield 'a';
    yield 'b';
    return 'c';

  }

  let k = tell();

  console.log(k.next()); //{value: "a", done: false}

  console.log(k.next()); //{value: "b", done: false}

  console.log(k.next()); //{value: "c", done: true}

  console.log(k.next()); //{value: undefined, done: true}

}
```

2.与 Iterator 接口的关系
由于 Generator 函数就是遍历器生成函数，因此可以把Generator赋值给对象的Symbol.iterator属性，从而使得该对象具有 Iterator 接口。

```javascript
{

  let obj = {};

  obj[Symbol.iterator] = function* (){

    yield '1';
    yield '2';
    yield '3';

  }

  for(let value of obj){

    console.log(value); // 1 2 3

  }

}
```

3.next方法

```javascript
{

  let state = function* (){

      yield 'a';
      yield 'b';
      yield 'c';

  }

  let status = state();

  console.log(status.next());  //a

  console.log(status.next());  //b

  console.log(status.next());  //c

  console.log(status.next());  //a

  console.log(status.next());  //b

  console.log(status.next());  //c

  console.log(status.next());  //a

}
```

4.Genertor的简单应用

```javascript
//简单的抽奖

{

  let draw = function(count){

    console.info(`剩余${count}次`);

  }

  let chou = function *(count){

    while (count>0) {
      count--;
      yield draw(count);
    }

  }

  let start = chou(5);

  let btn = document.createElement('button');

  btn.id = 'start';

  btn.textContent = '抽奖';

  document.body.appendChild(btn);

  document.getElementById('start').addEventListener('click',function(){

    start.next();

  },false);

}

// 长轮询

{

  let ajax = function* (){

    yield new Promise((resolve, reject) => {
      setTimeout(function(){
        resolve({code:1})
      },200)
    });

  }

  let pull = function(){

    let generator = ajax();
    let step = generator.next();
    step.value.then(function(d){
      if(d.code != 0){
        setTimeout(function(){
          console.log('wait');   //隔一秒输出 wait
          pull();
        },1000)
      }else{
        console.log(d);
      }
    })

  }

  pull();

}
```

十六修饰器
1.方法的修饰
修饰器函数一共可以接受三个参数，第一个参数是所要修饰的目标对象，即类的实例（这不同于类的修饰，那种情况时target参数指的是类本身）；第二个参数是所要修饰的属性名，第三个参数是该属性的描述对象。

```javascript
{

  let readonly = function(target,name,descriptor){

    descriptor.writable = false;
    return descriptor;

  };

  class test{

    @readonly
    time(){
      return '2017-08-27'
    }

  }

  let tests = new test();

  console.log(tests.time());  // 2017-08-27

  // let testss = new test();

  // // tests.time = function(){

  // //   console.log('2017-08-28');

  // // }

  // console.log(tests.time());  //Cannot assign to read only property 'time' of object

}
```

2.类的修饰
修饰器是一个对类进行处理的函数。修饰器函数的第一个参数，就是所要修饰的目标类。

```javascript
{

  let typename = function(target,name,descriptor){

    target.myname = 'hello';

  };

  @typename

  class test{

  }

  console.log(test.myname) // hello

}
```

十七模块化
ES6 模块不是对象，而是通过export命令显式指定输出的代码，再通过import命令输入。

```javascript
{

  export let A = 123;

  export function text(){

    console.log('123');

  }

  export class hello{

    text(){
      console.log('345');
    }

  }

}

{

  let A = 123;

  function text(){

    console.log('123');

  }

  class hello{

    text(){
      console.log('345');
    }

  }

  export default {

    A,
    text,
    hello

  }

}
```

借鉴了阮一峰ECMAScript 6 入门的内容