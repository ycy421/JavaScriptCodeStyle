# JavaScript(ECMAScript5) Style Guide

test
## 快速纠错
  - **jquery ajax**
  
    - 使用jquery ajax进行请求时，记得要指定 dataType:"json"，如果不指定，success函数里的'data'不会自动解析成JSON成JS对象；

    - 假设由于种种原因需要手动把JSON文本转换成JS对象，请使用JSON.parse()，避免使用eval()，如果一定要使用eval()，请说明理由。

  - 操作DOM时，发现问题，首先排查问题代码运行那一时刻，要操作的DOM元素是否已经存在、存在状态是否符合预期！

  - 变量声明会提升，但是赋值不会提升。（具体参考‘提升’章节） 为了避免混淆和意外，建议在作用域的最开始声明变量。赋值则可以在需要的地方进行。

  - 循环块和条件块在js中并不是作用域，所以不要在其内部声明变量，在其内部的变量声明都会被提升到作用域顶端，只会造成迷惑，没有意义。理想的做法是：在循环块和条件块外声明变量，然后在循环块和条件块内部仅仅赋值。

  - 对函数的声明尽量集中、分类、注释，声明之前预判函数要在哪里使用，从而放置到合理的作用域中。

  - 判断时建议使用严格模式‘===’，或者充分考虑到变量类型的可变性对转布尔值的影响，避免下述情况造成的困扰

    ```javascript
      var a = 0;
      var b = '0';

      Boolean(a); // => false
      Boolean(b); // => true
    ```  

  - 运算出现问题时，首先检查参与运算的各个变量的类型，是否符合预期，避免下列问题。

    ```javascript
      var a = 1;
      var b = '1';
      console.log(a + b); // => 11
    ```

  - **html标签描述**

    ```html
      //bad jsp自动生成的
      <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd"> 

      //good h5规范
      <!DOCTYPE html>
    ```    

  - **js脚本引用位置**
    统一在body标签的最后引用

      ```html
        <!--bad-->
        <header>
          <script src=''></script>
          <script>
            console.log('hello world');
          </script>          
        </header>
        <body>
          <!--otherCode-->
          <!--otherCode-->
          <!--otherCode-->
        </body> 

        <!--good-->
        <body>
          <!--otherCode-->
          <!--otherCode-->
          <!--otherCode-->
          <script src=''></script>
          <script>
            console.log('hello world');
          </script>
        </body> 
        
      ```  

## 完整内容

## <a name="table-of-contents">目录</a>

  1. [类型](#types)
  1. [对象](#objects)
  1. [数组](#arrays)
  1. [字符串](#strings)
  1. [函数](#functions)
  1. [属性](#properties)
  1. [变量](#variables)
  1. [提升](#hoisting)
  1. [比较运算符 & 等号](#comparison-operators--equality)
  1. [块](#blocks)
  1. [注释](#comments)
  1. [空白](#whitespace)
  1. [逗号](#commas)
  1. [分号](#semicolons)
  1. [类型转化](#type-casting--coercion)
  1. [命名规则](#naming-conventions)
  1. [构造函数](#constructors)
  1. [jQuery](#jquery)
  1. [js脚本引用优化](#importJs)
  1. [性能](#performance)

## <a name="types">类型</a>

  - **原始值**: 存取直接作用于它自身。

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1;
    var bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **复杂类型**: 存取时作用于它自身值的引用。

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2];
    var bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="objects">对象</a>

  - 使用直接量创建对象。

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - 不要使用[保留字](http://es5.github.io/#x7.6.1)作为键名，它们在 IE8 下不工作。[更多信息](https://github.com/airbnb/javascript/issues/61)。

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - 使用同义词替换需要使用的保留字。

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="arrays">数组</a>

  - 使用直接量创建数组。

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - 向数组增加元素时使用 Array#push 来替代直接赋值 *(非必须)*。

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - 当你需要拷贝数组时，使用 Array#slice *(经测试并没有显著性能提高，非必须)*。[jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];
    var i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - 使用 Array#slice 将类数组对象转换成数组。

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="strings">字符串</a>

  - 使用单引号 `''` 包裹字符串 *(强烈建议，以此与html标签中的双引号区分开)*。

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - 超过 100 个字符的字符串应该使用连接符写成多行。
  - 注：若过度使用，通过连接符连接的长字符串可能会影响性能。[jsPerf](http://jsperf.com/ya-string-concat) & [讨论](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - 程序化生成的字符串使用 Array#join 连接而不是使用连接符。尤其是 IE 下 *(非必须，经测试，对性能没有显著影响)*：[jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        // use direct assignment in this case because we're micro-optimizing.
        items[i] = '<li>' + messages[i].message + '</li>';
      }

      return '<ul>' + items.join('') + '</ul>';
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="functions">函数</a>

  - 函数表达式：

    ```javascript
    // 匿名函数表达式
    var anonymous = function() {
      return true;
    };

    // 命名函数表达式
    var named = function named() {
      return true;
    };

    // 立即调用的函数表达式（IIFE）
    // 推荐第二种写法，看起更直观
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());

    (function (privateParam) {
      console.log(privateParam);
    })(param);
    ```

  - 永远不要在一个非函数代码块（if、while 等）中声明一个函数，浏览器允许你这么做，但它们的解析表现不一致，正确的做法是：在块外定义一个变量，然后将函数赋值给它。

  - **注：** ECMA-262 把 `块` 定义为一组语句*(换言之：if,while等代码块里应该只包含语句！)*。而函数声明不是语句，所以它不应该包裹在独自存在于代码块中。[阅读对 ECMA-262 这个问题的说明](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97)。

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - 永远不要把参数命名为 `arguments`。这将取代函数作用域内的 `arguments` 对象。

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="properties">属性</a>

  - 使用 `.` 来访问对象的属性。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - 当通过变量访问属性时使用中括号 `[]`。

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="variables">变量</a>

  - 总是使用 `var` 来声明变量。不这么做将导致产生全局变量。我们要避免污染全局命名空间。

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - 使用 `var` 声明每一个变量。
    这样做的好处是增加新变量将变的更加容易，而且你永远不用再担心调换错 `;` 跟 `,`。

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // （跟上面的代码比较一下，看看哪里错了）
    var items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - 最后再声明未赋值的变量。当你需要引用前面的变量赋值时这将变的很有用。

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
    ```

  - 在作用域顶部声明变量。这将帮你避免变量声明提升相关的问题。

    ```javascript
    // bad
    function () {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function () {
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad - 不必要的函数调用
    function () {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // good
    function () {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      this.setFirstName(name);

      return true;
    }
    ```
  - null和undefined的区别	
    - null表示"没有对象"，即该处不应该有值。典型用法是：
    - undefined表示"缺少值"，就是此处应该有一个值，但是还没有定义。
  - 解释上如此，但是实际使用中区分别不多，例如：

    ```javascript
    var a;
    typeof a; // => 'undefined'
    typeof b; // => 'undefined'

    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="hoisting">提升</a>

  - 变量声明会提升至作用域顶部，但赋值不会。

    ```javascript
    // 我们知道这样不能正常工作（假设这里没有名为 notDefined 的全局变量）
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // 但由于变量声明提升的原因，在一个变量引用后再创建它的变量声明将可以正常工作。
    // 注：变量赋值为 `true` 不会提升。
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // 解释器会把变量声明提升到作用域顶部，意味着我们的例子将被重写成：
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - 匿名函数表达式会提升它们的变量名，但不会提升函数的赋值。

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  - 命名函数表达式会提升变量名，但不会提升函数名或函数体。

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // 当函数名跟变量名一样时，表现也是如此。
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - 函数声明提升它们的名字和函数体。

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 了解更多信息在 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ 回到顶部](#table-of-contents)**



## <a name="comparison-operators--equality">比较运算符 & 等号</a>

  - 优先使用 `===` 和 `!==` 而不是 `==` 和 `!=`.

  - *(当可以确定类型或者始终手动转型的情况下建议使用 ‘===’ 类型的写法，这样可以确保比较值的类型相同，避免奇怪的bug。但也不要滥用，例如直接把已有项目的‘==’直接换成‘===’，估计程序会直接挂掉)*

  - 条件表达式例如 `if` 语句通过抽象方法 `ToBoolean` 强制计算它们的表达式并且总是遵守下面的规则：

    + **对象** 被计算为 **true**
    + **Undefined** 被计算为 **false**
    + **Null** 被计算为 **false**
    + **布尔值** 被计算为 **布尔的值**
    + **数字** 如果是 **+0、-0 或 NaN** 被计算为 **false**，否则为 **true**
    + **字符串** 如果是空字符串 `''` 被计算为 **false**，否则为 **true**

    ```javascript
    if ([0]) {
      // true
      // 一个数组就是一个对象，对象被计算为 true
    }
    ```

  - 使用快捷方式。

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  - 了解更多信息在 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ 回到顶部](#table-of-contents)**


## <a name="blocks">块</a>

  - 使用大括号包裹所有的多行代码块。

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function () { return false; }

    // good
    function () {
      return false;
    }
    ```

  - 如果通过 `if` 和 `else` 使用多行代码块，把 `else` 放在 `if` 代码块关闭括号的同一行。

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ 回到顶部](#table-of-contents)**


## <a name="comments">注释</a>

  - 使用 `/** ... */` 作为多行注释。包含描述、指定所有参数和返回值的类型和值。

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - 使用 `//` 作为单行注释。在评论对象上面另起一行使用单行注释。在注释前插入空行。

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this.type || 'no type';

      return type;
    }
    ```

  - 给注释增加 `FIXME` 或 `TODO` 的前缀可以帮助其他开发者快速了解这是一个需要复查的问题，或是给需要实现的功能提供一个解决方式。这将有别于常见的注释，因为它们是可操作的。使用 `FIXME -- need to figure this out` 或者 `TODO -- need to implement`。

  - 使用 `// FIXME:` 标注问题。

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - 使用 `// TODO:` 标注问题的解决方式。

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="whitespace">空白</a>

  - 使用 4 个空格作为缩进。

    ```javascript
    // bad
    function () {
    ∙∙var name;
    }

    // bad
    function () {
    ∙var name;
    }

    // good
    function () {
    ∙∙··var name;
    }
    ```

  - 在大括号前放一个空格。

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - 在控制语句（`if`、`while` 等）的小括号前放一个空格。在函数调用及声明中，不在函数的参数列表前加空格。

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - 使用空格把运算符隔开。

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - 在文件末尾插入一个空行。

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  - 在使用长方法链时进行缩进。使用前面的点 `.` 强调这是方法调用而不是新语句。

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - 在块末和新语句前插入空行。

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function () {
      },
      bar: function () {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function () {
      },

      bar: function () {
      }
    };

    return obj;
    ```


**[⬆ 回到顶部](#table-of-contents)**

## <a name="commas">逗号</a>

  - 行首逗号: **不需要**。

    ```javascript
    // bad
    var story = [
        once
      , upon
      , aTime
    ];

    // good
    var story = [
      once,
      upon,
      aTime
    ];

    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - 额外的行末逗号：**不需要**。这样做会在 IE6/7 和 IE9 怪异模式下引起问题。同样，多余的逗号在某些 ES3 的实现里会增加数组的长度。在 ES5 中已经澄清了 ([source](http://es5.github.io/#D))：

  > Edition 5 clarifies the fact that a trailing comma at the end of an ArrayInitialiser does not add to the length of the array. This is not a semantic change from Edition 3 but some implementations may have previously misinterpreted this.

  ```javascript
  // bad
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn',
  };

  var heroes = [
    'Batman',
    'Superman',
  ];

  // good
  var hero = {
    firstName: 'Kevin',
    lastName: 'Flynn'
  };

  var heroes = [
    'Batman',
    'Superman'
  ];
  ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="semicolons">分号</a>

  - **使用分号。**

    ```javascript
    // bad
    (function () {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function () {
      var name = 'Skywalker';
      return name;
    })();

    // good (防止函数在两个 IIFE 合并时被当成一个参数
    ;(function () {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [了解更多](http://stackoverflow.com/a/7365214/1712802).

**[⬆ 回到顶部](#table-of-contents)**


## <a name="type-casting--coercion">类型转换</a>

  - 在语句开始时执行类型转换。
  - 字符串：

    ```javascript
    //  => this.reviewScore = 9;

    // bad
    var totalScore = this.reviewScore + '';

    // good
    var totalScore = '' + this.reviewScore;

    // bad
    var totalScore = '' + this.reviewScore + ' total score';

    // good
    var totalScore = this.reviewScore + ' total score';
    ```

  - 使用 `parseInt` 转换数字时总是带上类型转换的基数。

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = Number(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - 布尔:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ 回到顶部](#table-of-contents)**


## <a name="naming-conventions">命名规则</a>

  - 避免单字母命名。命名应具备描述性。

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - 使用驼峰式命名对象、函数和实例。

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var o = {};
    function c() {}

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - 使用帕斯卡式命名构造函数或类。

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - 给函数命名。这在做堆栈轨迹时很有帮助 *(非必须)*。

    ```javascript
    // bad
    var log = function (msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  - **注：** IE8 及以下版本对命名函数表达式的处理有些怪异。了解更多信息到 [http://kangax.github.io/nfe/](http://kangax.github.io/nfe/)。

**[⬆ 回到顶部](#table-of-contents)**

## <a name="constructors">构造函数</a>

  - 给对象原型分配方法，而不是使用一个新对象覆盖原型。覆盖原型将导致继承出现问题：重设原型将覆盖原有原型！

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - 方法可以返回 `this` 来实现方法链式使用。

    ```javascript
    // bad
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function jump() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function setHeight(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - 写一个自定义的 `toString()` 方法是可以的，但是确保它可以正常工作且不会产生副作用。

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="jquery">jQuery</a>

  - 使用 `$` 作为存储 jQuery 对象的变量名前缀 *(强烈建议)*。

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - 缓存 jQuery 查询 *(强烈建议)*。

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - 对 DOM 查询使用层叠 `$('.sidebar ul')` 或 父元素 > 子元素 `$('.sidebar > ul')`。 [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - 对有作用域的 jQuery 对象查询使用 `find`。

    ```javascript
    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ 回到顶部](#table-of-contents)**

## <a name="importJs">js引入优化</a>

  - 脚本要放到body标签的底部，从而减少对整个页面其他资源（DOM，CSS，图片等显示资源）下载的影响。
  - 时刻牢记，将脚本放在底部！

**[⬆ 回到顶部](#table-of-contents)**

## <a name="performance">性能</a>

  - 避免以下行为

    - 循环内定义变量和函数
    - 函数内部嵌套定义函数（闭包）

**[⬆ 回到顶部](#table-of-contents)**

