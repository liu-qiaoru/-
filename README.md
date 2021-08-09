# 记录

1、map object的区别

   map删除一项,object删除一项\n
   map获取长度,object获取长度\n
:object只能用字符串、数字、symbol做key,map可以是任意类型
 填入map的数据,会保持原有的顺序，而Object无法做到
 map可以通过for...of迭代,object不可以
 
 map删除:Map.prototype.delete
 object删除:delete Object.key

 map长度:Map.prototype.size
 object长度:Object.keys().lenght

2、set是什么,有什么优势

:集合,每一项是唯一的,可迭代,相对于数组来说增加元素、删除元素、查找元素更快

3、for in  和  for of 

:for...in用来遍历对象的键名,for...of一般用来遍历array的键值
for...in语句以任意顺序迭代对象的[可枚举属性],for...of 语句遍历[可迭代对象]定义要迭代的数据。

4、js基本数据类型

:string number bool undefined null symbol bigint

5、undefined 和 null 的区别

:null表示“没有对象”,即该处不应该有值,undefined表示“缺少值”,即该处应该有值,但还没有定义

6、var 和 let   let和const    const定义一个对象,可以修改里面的项吗 const a = {b:1}; a.b = 2;

:var存在变量提升,let必须先声明再使用
 var是全局作用域,let有块级作用域概念
 
 let该有的特性，const都有;不同的是,第一const不允许“修改”变量，第二const必须初始化
 
 使用const声明的对象的属性是可以修改。因为Object类型是引用类型,用const声明常量保存的是对象的地址，不可变的是地址，
 而修改对象的属性，并不会改变对象的地址，因此用const声明对象的属性是可以修改的.  数组也是引用类型，同理


7、暂存性死区

:let声明不会被提升到当前执行上下文的顶部，从该块级作用域开始，到初始化位置，称作“暂存死区”，所以在对于a的暂存死区中使用a会报Reference错误


8、在浏览器定义一个var 和 在node中定义一个var 有什么区别

:在 HTML 中, 全局作用域是针对 window 对象，var关键字定义的全局作用域变量属于 window 对象，而let定义的不属于。注意这是在浏览器环境下，如果是Node则无window对象。
`
var a = 0;
console.log(window.a) // 0
let b = 1;
console.log(window.b) // undefined
`

9、scss样式如何局部应用,原理

添加scoped属性,会给每一个标签添加data属性

10、webpack配置如何解析scss

`
module.exports = {
  module: {
    rules: [
      {
        // 增加对 SCSS 文件的支持
        test: /\.scss$/,
        // SCSS 文件的处理顺序为先 sass-loader 再 css-loader 再 style-loader
        use: ['style-loader', 'css-loader', 'sass-loader'],
      },
    ]
  },
};
`
1.通过 sass-loader 把 SCSS 源码转换为 CSS 代码，再把 CSS 代码交给 css-loader 去处理。
2.css-loader 会找出 CSS 代码中的 @import 和 url() 这样的导入语句，告诉 Webpack 依赖这些资源。同时还支持 CSS Modules、压缩 CSS 等功能。处理完后再把结果交给 style-loader 去处理。
3.style-loader 会把 CSS 代码转换成字符串后，注入到 JavaScript 代码中去，通过 JavaScript 去给 DOM 增加样式。


11、babel是什么,原理

:babel是一个 JavaScript 编译器,主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中

1.解析
解析步骤接收代码并输出 AST。 这个步骤分为两个阶段：词法分析（Lexical Analysis） 和 语法分析（Syntactic Analysis）。
2.转换
转换步骤接收 AST 并对其进行遍历，在此过程中对节点进行添加、更新及移除等操作
Babel提供了@babel/traverse(遍历)方法维护这AST树的整体状态，并且可完成对其的替换，删除或者增加节点，这个方法的参数为原始AST和自定义的转换规则，返回结果为转换后的AST。
3.生成
代码生成步骤把最终（经过一系列转换之后）的 AST 转换成字符串形式的代码，同时还会创建源码映射（source maps）。
深度优先遍历整个 AST，然后构建可以表示转换后代码的字符串
Babel使用 @babel/generator 将修改后的 AST 转换成代码，生成过程可以对是否压缩以及是否删除注释等进行配置，并且支持 sourceMap


# 题

1、给定一个非空数组,求出现最高的数据,以数组形式返回
eg.  [1,2,2,3]  返回:[2]
     [1,‘2’,‘2’,1,3,null,null]  返回:[1,‘2’,null]

tip:使用map
    
2、给定一个整数无序数组和变量sum，如果存在数组中任意两项和等于 sum 的值，则返回true。否则,返回false。
eg. 数组[3,5,1,4] sum = 9， 返回true 因为4 + 5 = 9

tip: 使用set
