## JS语法 ##
1. 闭包的含义:闭包是指在js中内部函数总是可以访问外部函数中声明的参数和变量,即使在外部函数被返回了之后
    ```js 
    function Counter(start) {
        var count = start;
        return {
            increment: function () {
                count++;
                },
                get: function() {
                    return count;
                    }
        }
    } 

    var foo = new Counter(4);
    foo.increment;
    foo.get(); //5
    ```
    这里,Couter函数返回两个闭包,函数increment和get.这两个函数都维持着对外部作用域Counter的引用,因此总可以访问此作用域内定义的变量  

    - 循环中的闭包  
    一个常见的错误出现在循环中使用闭包
    ```js
        for(var i = 0; i < 10; i ++) {
            setTimeout(function () {
                console.log(i)
            }, 1000);
        }
    ```
    上面的代码不会输出0-9，而是输出数字10十次.因为当console.log被调用时，依然保持这对i的引用  
    为获得正确的序列号,需要在每次循环中创建变量i的拷贝  
    ```js
        for(var i = 0; i < 10; i ++) {
            (function (e){
                setTimeout(function() {
                    console.log(e);
                }, 1000);
            })(i)
        }
    ```  
    外部匿名函数会立即执行，并把i作为它的参数,此时函数内e变量就拥有了i的拷贝  
2. 使用闭包的好处  
- 希望一个变量长期驻在内存
- 避免全局变量的污染
- 私有成员