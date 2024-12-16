[合集 \- JavaScript(1\)](https://github.com)1\.JavaScript是按顺序执行的吗？聊聊JavaScript中的变量提升12\-15收起
作为一位前端开发者，我们经常会听到这么一句话：**“JavaScript的执行是按照顺序自上而下依次执行的。”**这句话说的并没有错。但是它似乎又好像不完全对。我们先来看以下这段代码。你觉得结果会输出什么？




```
1 showName()
2 console.log(myName)
3 
4 var myName = '修谦'
5 function showName() {
6     console.log('我的名字叫修谦')
7 }
```


若是按照之前说的自上而下依次执行的逻辑话，那么应该输出的结果应该是：



> 1、因为函数showName执行时，其并未定义，因此会报错
> 
> 
> 2、同样的，因为变量myName也并未定义，因此也是会报错


然而当我们在浏览器控制台执行的时候，其实际的结果却如下图所示。


![image.png](https://icenterapi.zte.com.cn/group3/M04/0E/A9/CimMFmdWWMCABBZTAABjd1kiugM733.png)


代码竟然没有报错！第一行输出了“我的名字叫修谦”，第 2 行则输出了“undefined”，这时候你是否会有疑问：“这怎么和前面想象中的顺序执行有点不一样啊！怎么结果会是这样的呢？”


到这里，我想你应该想到了一点什么。那就是：“函数和变量是可以在定义之前使用的”**。**但是我们如果执行未定义的函数和变量的话，又会是一个什么样的结果呢？


我们尝试着将之前的第三行代码删掉，然后执行。




```
1 showName()
2 console.log(myName)
3 
4 function showName() {
5     console.log('我的名字叫修谦')
6 }
```


运行代码后，如下图所示，这一次我们看到的结果是函数已经执行了，但是console函数输出的已经报错了，输出了“myName is not defined”。


![image.png](https://icenterapi.zte.com.cn/group3/M04/0E/E9/CimMFmdWW1OALCRmAABjEPqJd5g780.png)


到这里，对于以上的两个结果，你是否又能得到了一些新的启示呢？事实上，通过上面的两次代码执行，我们至少可以得到以下几个结论：



> 1、JavaScript在执行的过程中，如果使用了未定义的变量，则会报错
> 
> 
> 2、在一个变量定义之前使用，不会报错，只是其值是undefined。
> 
> 
> 3、在一个函数定义前使用它，并不会报错，而是会正确执行


第一个结论我们很容易理解，因为变量未被定义，所以在使用的时候肯定是找不到，因此必然会报错。但是对于第二个和第三个结论，确实让人费解的：变量和函数为什么能在其定义之前使用？这似乎表明JS代码并不是按之前说的自上而下依次执行的。


另外一点，就是同样的方式，变量和函数的处理结果为什么不一样？如上面的执行结果，提前使用的showName函数能打印出来完整结果，但是提前使用的myName变量值却是undefined，而不是我们定义时使用的“修谦”的这个值。要解释这个，就不得不说到JavaScript中的一个很重要的概念：**变量提升**


### 1、什么是JavaScript的变量提升（Hoisting）


在说JavaScript的变量提升之前，我们得要先说一下JavaScript中的声明和赋值操作，对于如下的这行代码




```
1 var myName = '修谦'
```


实际上，这句代码你可以把它分为两部分来看，即**声明**和**赋值**




```
1 var myName //  变量声明
2 myName = '修谦' // 变量赋值
```


以上的这个是JavaScript中变量的声明和赋值，我们再来看一下JavaScript中的函数声明和赋值操作是什么样的，我们还是看以下这段代码




```
1 function showName() {
2     console.log('我的名字叫修谦')
3 }
4 
5 var showName = function() {
6     console.log('我的名字叫修谦')
7 }
```


我们可以看出第一个函数showName是一个完整的函数声明，它没有涉及到赋值操作；第二个函数是先声明变量showName，再把function(){console.log('我的名字叫修谦')}赋值给了showName。到这里你应该知道了JavaScript中的变量声明和赋值是怎么回事了。


说完了JavaScript中的变量声明和赋值是怎么回事后，我们再来说JavaScript中的变量提升。


 在JavaScript中，所谓的变量提升：是指在 JavaScript 代码执行过程中，JavaScript 引擎把变量的声明部分和函数的声明部分提升到代码开头的一种“行为”。当变量被提升后，会给变量设置默认值，而其所设置的默认值就是我们最为熟悉的undefined。从这个概念的字面意义上来看，“变量提升”意味着变量和函数的声明会在物理层面移动到代码的最前面。


但其实这样说也并不准确。因为实际上，在JavaScript中，变量和函数的声明在代码里的位置是不会改变的。为什么呢？因为在JavaScript中，一段代码的执行是需要先经过JavaScript引擎先编译的，当代码编译完后，才会进入到代码的执行阶段（下图所示）。说变量和函数的声明在代码里的位置是不会改变的原因，是因为代码在编译阶段便已经被JavaScript引擎放入到了内存中（既然放到了内存当中，那么其位置当然就已经固定）。


![image.png](https://icenterapi.zte.com.cn/group3/M04/10/FE/CimMFmdWi6KAXXLbAADWsan4XOQ361.png)


那既然在编译阶段就在内存中固定了位置，为什么又会出现提升呢？编译阶段和变量提升存在什么关系呢？这里我们就不得不说到另外一个概念：**执行上下文（Execution context）**


### 2、执行上下文**（Execution context）**


所谓**执行上下文**，我们可以简单的理解为就是 **JavaScript 执行一段代码时的运行环境**，比如当我们在JavaScript文件中调用一个函数，那么就会进入这个函数的执行上下文，就会确定该函数在执行期间用到的诸如 this、变量、对象以及函数等。并且在执行上下文中还存在一个**变量环境的对象（Viriable Environment）**，这是非常重要的。因为该对象中保存了变量提升的内容，比如上面代码中的变量myName和函数showName，都会保存在该对象中（我们先用下面的这段代码模拟一下，后面在详细讲解）。




```
1 ViriableEnvironment（变量环境）
2     myName -> undefined
3     showName -> function: {console.log(myName)}
```


在JavaScript中，执行上下文一般分为以下三种：


**1、全局执行上下文：**当 JavaScript 执行全局代码的时候，会编译全局代码并创建全局执行上下文，而且在整个页面的生存周期内，全局执行上下文只有一份。


**2、函数执行上下文：**当调用一个函数的时候，函数体内的代码会被编译，并创建函数执行上下文，在一般情况下，函数执行结束之后，创建的函数执行上下文会被销毁。


**3、eval ：**当使用 eval 函数的时候，eval 的代码也会被编译，并创建执行上下文。


但是我们现在常接触或者说的一般都是指前面两者。了解完执行上下文的概念和分类后，我们再来了解一下另外的两个知识点：**函数执行（调用）**和**栈**


### 3、函数执行（调用）


函数调用概念很简单，简单一点来说就是运行一个函数，具体使用方式是使用函数名称跟着一对小括号。我们举个例子来说一下




```
1 var myName = '修谦'
2 function showName() {
3     console.log('我的名字叫修谦')
4 }
5 
6 showName() // 执行
```


这段代码很简单。首先我们创建了一个名叫myName的变量，接着又创建了一个showName的函数。完后紧接着在最后面调用执行了该方法。下面我们就以这段简单的代码来说一下函数调用的过程。


当执行到函数showName()之前，JavaScript 引擎会为上面这段代码创建全局执行上下文，包含声明的函数和变量，如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/16/2B/CimMFmdWyTyAJxNkAAChT-8hybo039.png)          


从图中可以看出，上面那段代码中全局变量和函数都保存在全局上下文的变量环境中。当执行上下文准备好之后，JavaScript引擎便开始执行全局代码，当执行到showName函数时，JavaScript判断出这是一个函数调用，于是便开始了以下操作：




> 1、首先，从全局执行上下文中，取出showName函数代码。
> 2、其次，对showName函数的这段代码进行编译，并创建该函数的执行上下文和可执行代码。
> 3、最后，执行代码，输出结果。


我们可以用一张相对完整的图来描述 



![](https://icenterapi.zte.com.cn/group3/M04/14/B8/CimMFmdWsC-AHXcMAADnCUdInCQ933.png)    


当执行到showName函数的时候，我们就有了两个执行上下文了——**全局执行上下文**和 **showName 函数本身的执行上下文（函数执行上下文）**。这也就是说在执行JavaScript 时，会存在多个执行上下文。那当有多个上下文的时候，JavaScript引擎是如何管理的呢？这就是我们下面要说到的一种数据结构——**栈**
### 4、栈（Stack）


**栈（Stack）**是一种线性数据结构，遵循**后进先出**（Last In, First Out, LIFO）的原则。这意味着最后进入栈的元素会最先被移除。如下图所示，最先进入的是A，但是最先出的却是E。而avaScript引擎正是利用栈的这种结构来管理执行上下文的。在执行上下文创建好后，JavaScript引擎会将执行上下文压入栈中，然后进行执行，而通常把这种用来管理执行上下文的栈称为**执行上下文栈**，或者叫**JavaScript调用栈**。


![](https://icenterapi.zte.com.cn/group3/M04/16/4C/CimMFmdWy2mACkF_AABtpa9spHU318.png)    


### 4、执行上下文栈（**JavaScript调用栈**）


下面我们就来具体的用代码和图来模拟JS执行上下文栈是如何执行代码的，如下面一段代码（以ES5来演示）




```
 1 var a = 2
 2 function add(b,c){
 3   return b+c
 4 }
 5 function addAll(b,c){
 6   var d = 2
 7   result = add(b,c)
 8   return a+result+d
 9 }
10 addAll(3,3)
```


**第一步：创建全局上下，并将其压入栈底（如图所示）**此时变量a、函数add 以及 addAll 都保存到了全局上下文的变量环境对象中。 


**![](https://icenterapi.zte.com.cn/group3/M04/24/1F/CimMFmdYH9qAaIRuAAByQxgAVfs904.png)**


当全局执行上下文压入到调用栈后，紧接着，JavaScript引擎便开始执行全局代码了。首先会执行a\=2的赋值操作，赋值完后，此前a的值就从undefined变成了2。因为此时的add函数和addAll函数都还没有执行，因此状态还是之前的。这一步完成后，我们再来看全局上下文的状态，如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/24/50/CimMFmdYIseAH4MmAABInu3P0Ic090.png)    


**第二步：执行addAll函数，**此时JavaScript引擎会编译该函数，并为其创建一个执行上下文，然后将其执行上下文压入栈中，如图所示：


![](https://icenterapi.zte.com.cn/group3/M04/25/33/CimMFmdYL0aAKcclAADELBQIO3s207.png)    


同样的，当addAll函数的执行上下文创建好之后，就会进入了函数代码的执行阶段了，因为函数中有一个变量d，因此还是先执行赋值操作，即将d的值从之前的d\=undefined设置成d\=10 。然后接着往下执行。


**第三步：执行add函数，**当执行到add函数调用语句时，JavaScript引擎同样又会为其创建执行上下文，并将其压入调用栈，此时的调用栈的状态如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/24/DF/CimMFmdYKryAA7njAAEHmyS_S8Y895.png)            


然后add函数执行，将返回结果赋值给变量result，此时的result的值便从之前的undefined变成了6。随后该函数的执行上下文便从从栈顶弹出。此时的调用栈如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/24/E9/CimMFmdYKzWAW0ycAADFPbs_GUo477.png)    


紧接着addAll执行最后一个相加操作后并返回，完成之后，addAll的执行上下文也会从栈顶部弹出，此时调用栈中就只剩下全局上下文了。最终如下图所示。 至此，整个JavaScript的执行便完成了。


![](https://icenterapi.zte.com.cn/group3/M04/24/FA/CimMFmdYLFiAOIvwAABwicRA2lw441.png)    


通过以上的分析，我们可以知道，正是由于JavaScript存在变量提升这种特性，从而导致了我们在日常的学习或者工作中，总是能看到很多与直觉不符或者说与我们思习惯不一样的代码，而这也是JavaScript的一个重要设计缺陷。为此，**ECMAScript6**引入**块级作用域**的概念并配合 **let、const**关键字，来避开了这种设计缺陷（这个我们接下来就会说）。但是在说之前，我们还要继续说变量提升剩余的两个问题：**为什么JS中会出现变量提升？变量提升有什么缺点？**


### 5、JS中变量提升的原因


我们都知道在ES6 之前，JavaScript是不支持块级作用域的。因为当初设计这门语言的时候，只是按照最简单的方式来设计的。即只设计了**全局作用域**和**函数作用域。**以此来简化JavaScript代码的解析和执行过程。可没有想到的是 JavaScript后面会这么火，最后其没有块级作用域的缺陷便慢慢暴露了出来。


既然问题已经暴露出来了的话，那就解决问题。但是你不可能贸然的立马增加块级作用域吧！毕竟已经用JavaScript这门语言开发了那么多应用。于是就采取了一个不是特别激进的方法——**把作用域内部的变量统一提升**。这也是彼时最快速，也是最简单的方式。


当然了任何事物都有两面性。这一做法的一个很大的缺点就是直接导致了函数中的变量无论是在哪里声明的，在编译阶段都会被提取到执行上下文的变量环境中，所以这些变量在整个函数体内部的任何地方都是能被访问的，而这也就是我们通常说的JS 中的变量提升。


### 6、JS中变量提升的问题


**1、变量在不知不觉中就被覆盖**


我们先来看下面的一段代码，你认为会输出什么结果？是修谦？是吴门山人？




```
 1 var myName = "修谦"
 2 
 3 function showName(){
 4   console.log(myName);
 5   if(0){
 6    var myName = "吴门山人"
 7   }
 8   console.log(myName);
 9 }
10 
11 showName()
```


其实你把代码执行的话，会发现其输出的结果两者都不是。而是输出了undefined。为什么会这样呢？你可以参照前面举的那个JS执行的例子来自己试着画一下过程图。这里我们就直接贴最后的执行栈图。


![](https://icenterapi.zte.com.cn/group3/M04/51/EE/CimMFmdc9-iABQIjAADIV05cT2k130.png)    


当showName函数的执行上下文创建后，JavaScript引擎便开始执行其内部的代码。首先执行的是console.log(myName)。而执行这段代码需要使用变量myName，而从图上我们可以看到，这里有两个myName变量：一个是在全局执行上下文中，其值是“修谦”；另外一个则是在showName函数的执行上下文中，其值是undefined。这个时候JS到底要使用哪一个输出呢？作为一个前端开发人员，我想绝大部分人都会说出正确的答案：“肯定是先使用showName函数执行上下文里面的变量啦！”


的确是这样，因为函数执行过程中，JavaScript会优先从当前所在的执行上下文中查找变量，但是因为变量提升的原因，当前的执行上下文中就包含了变量myName，而其值是undefined，所以获取到的myName的值就是undefined。而不是如其它语言一样，会输出“修谦”。


**2\. 本应销毁的变量没有被销毁**


那既然在JavaScript中，变量提升会带来上面说到的那些个问题？最后的解决方案又是什么呢？答案就是在2015年的时候发布了新的JS标准——**ECMAScript6（简称ES6）。**在 该标准中，正式引入了块级作用域的概念。并且还引入了**let**和**const**关键字来声明块级作用域，至此，JavaScript也能像其他语言一样拥有了块级作用域。


### **7、ES6中的let和const**


关于let和const。我们还是先来看如下的代码




```
1 let myName = '修谦'
2 const myAag = 35
3 myName = '山人'
4 console.log(myName)
5 
6 myAag = 18
7 console.log(myAag)
```


这段代码输出的结果，我觉得只要是写过JavaScript的人都应该知道结果是啥。第一个输出的是“山人”；而第二个则输出一个错误。从这里我们可以看出，虽然两者都是用来声明块级作用域的，但是两者之间还是有区别的，使用 **let** 关键字声明的变量是可以被改变的，而使用 **const** 声明的变量其值是不可以被改变的。说到这里我们也顺带说一下面试中常被问到的一个问题：**在JavaScript中，什么是暂时性死区?**


还是先看代码




```
1 function example() {
2   console.log(x); 
3   let x = 10;
4 }
5 
6 example();
```


当我们把这段代码复制到到浏览器控制台的时候会报这样一个错误： “ReferenceError: Cannot access 'x' before initialization”。这个错误翻译过来是：引用错误：初始化之前无法访问“x”（翻译的可能不准，但是意思差不多）。从这个错误我们知道了在ES6中，当我们用**let**和**const** 声明的变量在声明之前是处于一种“未初始化”状态，而这种状态被称为暂时性死区（官方的定义是：在 JavaScript 中，"暂时性死区"（Temporal Dead Zone, TDZ）是指在块级作用域（如 let 和 const 声明的变量所在的代码块）中，在变量声明之前访问该变量会导致引用错误（ReferenceError））。


 说完**let**和**const**，我们再来看以下的这两行简单的代码




```
1 var myName = '修谦'
2 let myAag = 35
```


这两行代码其实并没有什么特别的，我用其来就只是为了引出一个问题，即：JavaScript是怎么样在支持变量提升特性的同时又支持块级作用域的呢？因为我们在项目中，有时候你会发现有的人在代码中即会用**var**关键字来声明变量，同时又用**let**和**const**来声明变量。虽然这种方式不推荐，但是总归是不可避免的。前面我们已经谈到了变量提升特性。所以接下来我们重点谈的就是JavaScript是如何支持块级作用域的。


### 8、JavaScript 是如何支持块级作用域的？


前面我们说到，在JavaScript引擎中是通过变量环境实现函数级作用域的，那么在 ES6 中，又是如何在其基础之上，实现对块级作用域的支持呢？我们还是先来看下面的一段代码




```
 1 function showName(){
 2     var myName = '修谦'
 3     let myAag = 35
 4     {
 5       let myAag = 18
 6       var heName = '华仔'
 7       let heAge = 63
 8       console.log(myName)
 9       console.log(myAag)
10     }
11 }   
12 showName()
```


当执行上面这段代码的时候，JavaScript引擎会先对其进行编译并创建执行上下文，然后再按照顺序执行代码，之前我们的例子是没有使用ES6中的关键字**let**。但是现在引入了**let**关键字，它会创建块级作用域，那么它是如何影响执行上下文的呢？这里我们就不得不提到一个名词——**词法环境**。你应该还记得之前的例子中，右边一直有一块空着的，名叫词法环境的块。而JavaScript之所以支持块级作用域，就是与它有关。


下面我们还是按照之前的方式来梳理一下这段代码的执行。


**第一步：编译并创建全局执行上下文**


![](https://icenterapi.zte.com.cn/group3/M04/55/DA/CimMFmddOQ-AA_22AAB0ydNY8JM612.png)    


**第二步：执行showName函数，为其创建函数执行上下文**


![](https://icenterapi.zte.com.cn/group3/M04/55/EC/CimMFmddOn6AX9iyAADdevX6DVU540.png)    


到showName函数执行这一步。我么可以从调用栈中看出：




> 1、函数内部通过 var 声明的变量，在编译阶段全都被存放到变量环境里面了（这个和之前的一样）
> 2、通过 let 声明的变量，在编译阶段则会被存放到词法环境（Lexical Environment）中
> 3、在函数的作用域块内部，通过 let 声明的变量并没有被存放到词法环境中



**第三步：继续往下执行代码。**当执行到代码块里面时，变量环境中myName的值已经被设置成了"修谦"，而词法环境中myAag的值则被设置成了35，此时的函数的执行上下文如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/56/13/CimMFmddPU-ABowNAADxnWJfnLQ220.png)    


从第三步的图中我们可以看出，当进入函数内部的作用域块时，作用域块中通过 **let** 声明的变量（myAag和heAag），会被存放在词法环境的一个单独的区域中，且不影响作用域块外面的变量（之前的myAag)。因此它们都是独立的存在。另外我们从中也可以看出，其实**在词法环境内部，也是维护了一个小型栈结构，栈底是函数最外层的变量**（即内部作用域块外边的变量，这里就是myAag），当进入某一个作用域块后，就会把该作用域块内部的变量压到栈顶（myAag和heAag）；当作用域执行完成之后，该作用域的信息就会从栈顶弹出，而这就是词法环境的结构（前提就是必须用**let**或者**const**关键字定义）。


**第四步：继续往下执行代码。**将作用块中的myAag和heAag分别赋值为16，63，同时也将环境变量中的heName的值赋值为“华仔”。如图所示


![](https://icenterapi.zte.com.cn/group3/M04/56/5A/CimMFmddQp6ALpeOAADv2tqzIa8909.png)    


**第四步：继续往下执行代码。**当执行到作用域块中的console.log(myName)这行代码时，此时就需要在**词法环境**和**变量环境**中查找变量myName的值了，而具体查找方式是：**沿着当前词法环境的栈顶向下查询，如果在词法环境中的某个块中查找到了，就直接返回给JavaScript引擎，如果没有查找到，那么继续在变量环境中查找（同样的，作用域块中的console.log(myAag)也是这样的规则）**。此时如下图所示：因为在词法环境中没有找到myName的这个变量，因此就会去变量环境中去找，最终在变量环境中找到了myName（黄色箭头所指），因此输出“修谦”**。**同样console.log(myAag)因为在词法环境中找到了myAag（深蓝色箭头所指），因此输出18。而将上面的代码在浏览器里执行，也是这样的结果


![](https://icenterapi.zte.com.cn/group3/M04/56/94/CimMFmddR7CAefjeAAE34uI9jWg290.png)    


![image.png](https://icenterapi.zte.com.cn/group3/M04/56/A1/CimMFmddSKSAQHgiAAArKlu99qs746.png)


当函数内部作用域块执行结束之后，其内部定义的变量就会从词法环境的栈顶弹出，最终的执行上下文如下图所示：


![](https://icenterapi.zte.com.cn/group3/M04/56/AA/CimMFmddSTWARlyDAADf8KSKRoA065.png)    


通过上面的分析，我们基本已经理解了词法环境的结构和工作机制：**ES6中的块级作用域就是通过词法环境的栈结构来实现的，而之前的变量提升是通过变量环境来实现，通过这两者的结合，JS 引擎也就同时支持了变量提升和块级作用域了。**至此，我想关于变量提升，你应该有一个比较深刻的印象了。当然了，上面写的可能并不完全正确。也欢迎大家指正批评。


 


 本博客参考[楚门加速器](https://shexiangshi.org)。转载请注明出处！
