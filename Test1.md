# Javascript Coding Style

## **GOLDEN RULE**

This rules are the recommended and should be followed at any new project.

On existing projects: project's consistency should be a priority over this document. Choose the style your team prefeers but you **ABSOLUTELY MUST** enforce consistency.

> "Arguments over style are pointless. There should be a style guide, and you should follow it"
>
> Rebecca Murphey


> "Part of being a good steward to a successful project is realizing that writing code for yourself is a Bad Idea™. If thousands of people are using your code, then write your code for maximum clarity, not your personal preference of how to get clever within the spec."
>
> Idan Gazit

## <a name='TOC'>Table of Contents</a>

  1. Javascript Basics
    1. [Types](#types)
    1. [Properties](#properties)
    1. [Variables](#variables)
    1. [Hoisting](#hoisting)

  1. Coding Style
    1. [Objects](#objects)
    1. [Arrays](#arrays)
    1. [Strings](#strings)
    1. [Functions](#functions)
    1. [Variables](#variables)
    1. [Conditional Expressions & Equality](#conditionals)
    1. [Parentheses](#parentheses)
    1. [Blocks](#blocks)
    1. [Comments](#comments)
    1. [Whitespace](#whitespace)
    1. [Leading Commas](#leading-commas)
    1. [Semicolons](#semicolons)
    1. [Type Casting & Coercion](#type-coercion)
    1. [Naming Conventions](#naming-conventions)
    1. [Accessors](#accessors)
    1. [Constructors](#constructors)
    1. [Modules](#modules)

  1. Angular specific
    1. [[Open to develop]]

  1. Linting
    1. [Lint](#lint)
    1. [jsHint](#jshint)

## <a name='types'>Types</a>

  - **Primitives**: When you access a primitive type you work directly on its value

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    var foo = 1,
        bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - **Complex**: When you access a complex type you work on a reference to its value

    + `object`
    + `array`
    + `function`

    ```javascript
    var foo = [1, 2],
        bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

    **[[⬆]](#TOC)**

## <a name='properties'>Properties</a>

  - Use dot notation when accessing properties.

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

  - Use subscript notation `[]` when accessing properties with a variable.

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

    **[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration for each variable unless you don't assign any values for them.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z',
        some,
        value;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    var some, value;
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball;
    var items = getItems();
    var goSportsTeam = true;

    // bad
    var i, items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;

    // better
    var items = getItems();
    var goSportsTeam = true;
    var dragonball, length, i;
    ```

  - Group lines of code by it's relation separated by a single empty line:

    ```javascript
    // bad
    function() {
      test1();
      test2();
      console.log('doing stuff..');
      var name = getName();
      if (name !== 'test') {
        return name;
      }

      return false;
    }

    // good
    function() {
      test1();
      test2();

      console.log('doing stuff..');

      var name = getName();
      if (name !== 'test') {
        return name;
      }

      return false;
    }
    ```
    **[[⬆]](#TOC)**


## <a name='hoisting'>Hoisting</a>

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };


      // the same is true when the function name
      // is the same as the variable name.
      function example() {
        console.log(named); // => undefined

        named(); // => TypeError named is not a function

        var named = function named() {
          console.log('named');
        };
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - For more information refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) by [Ben Cherry](http://www.adequatelygood.com/)

  **[[⬆]](#TOC)**


## <a name='objects'>Objects</a>

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Reserved_Words) as keys.

    ```javascript
    // bad
    var superman = {
      class: 'superhero',
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      klass: 'superhero',
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  **[[⬆]](#TOC)**


## <a name='arrays'>Arrays</a>

  - Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    var someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length;
    var itemsCopy = [];

    // bad
    for (var i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = Array.prototype.slice.call(items);
    ```

  **[[⬆]](#TOC)**


## <a name='strings'>Strings</a>

  - Use single quotes `''` for strings

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

  - Strings longer than 120 characters should be written across multiple lines using string concatenation.

      ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that \
    was thrown because of Batman. \
    When you stop to think about \
    how Batman had anything to do \
    with this, you would get nowhere \
    fast.';


    // good
    var errorMessage = 'This is a super long error that ' +
      'was thrown because of Batman.' +
      'When you stop to think about ' +
      'how Batman had anything to do ' +
      'with this, you would get nowhere ' +
      'fast.';
    ```

  **[[⬆]](#TOC)**


## <a name='functions'>Functions</a>

  - Function expressions:

    ```javascript
    // function declaration
    function declared() {
      return true;
    }

    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    if (currentUser) {
      var test = function test() {
        console.log('Yup.');
      };
    }
    ```

  **[[⬆]](#TOC)**


## <a name='variables'>Variables</a>

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration for each variable unless you don't assign any values for them.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z',
        some,
        value;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    var some, value;
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball;
    var items = getItems();
    var goSportsTeam = true;

    // bad
    var i, items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;

    // better
    var items = getItems();
    var goSportsTeam = true;
    var dragonball, length, i;
    ```

  - Assign variables when they are needed. This is helpful in order to add or delete a block of code. If a variable is used at many lines blocks is preferred to declare it at the top of the function.

    ```javascript
    // bad
    function() {
      var name;

      if (!arguments.length) {
        return false;
      }

      name = getName();
      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();
      return true;
    }

    // bad
    function() {
      var name = getName();
      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }
    ```

  **[[⬆]](#TOC)**


## <a name='parentheses'>Parentheses</a>

  - Only where required
  - Use sparingly and in general only where required by the syntax and semantics.
  - Never use parentheses for unary operators such as delete, typeof and void or after keywords such as return, throw as well as others (case, in or new).

    ```javascript
    // bad
    if (typeof(0) !== 'number') throw(new Error('WTF'));

    // good
    if (typeof 0 !== 'number') throw new Error('WTF');
    ```

  **[[⬆]](#TOC)**


## <a name='conditionals'>Conditional Expressions & Equality</a>

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evalute to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

  - Use shortcuts.

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

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll

  **[[⬆]](#TOC)**

## <a name='blocks'>Blocks</a>

  - Use braces with all multi-line blocks.
  - No braces are only allowed for single-sentence `if`s and if the sentence is on the same line

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
    function() { return false; }

    // good
    function() {
      return false;
    }
    ```

  **[[⬆]](#TOC)**


## <a name='comments'>Comments</a>

  - Use `/** ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param <String> tag
     * @return <Element> element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an emptyline before the comment.

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
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  **[[⬆]](#TOC)**


## <a name='whitespace'>Whitespace</a>

  - Use hard tabs

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙∙var name;
    }

    // good
    function() {
    --->var name;
    }
    ```
  - Place 1 space before the leading brace.

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

  - Place 1 space before opening parens except for function declaration and invocation

    ```javascript
    // bad
    if(test) return false

    // good
    if (test) return false

    // bad
    this.set = function set (key, value) {
      this[key] = value;
    };
    this.set ('age', 30);

    // good
    this.set = function set(key, value) {
      this[key] = value;
    };
    this.set('age', 30);
    ```

  - Place 1 space before and after operators

    ```javascript
    // bad
    var age=date.getYear()-me.bornYear();
    if (age===30) return true;

    // good
    var age = date.getYear() - me.bornYear();
    if (age === 30) return true;
    ```

  - Place an empty newline at the end of the file.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);

    ```

  **[[⬆]](#TOC)**


## <a name='leading-commas'>Leading Commas</a>

  - **Nope.**

    ```javascript
    // bad
    var array = [
        once
      , upon
      , aTime
    ];

    // good
    var array = [
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

  **[[⬆]](#TOC)**


## <a name='semicolons'>Semicolons</a>

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();

    // good
    ;(function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

  **[[⬆]](#TOC)**


## <a name='type-coercion'>Type Casting & Coercion</a>

  - Perform type coercion at the beginning of the statement.
  - Strings:

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

  - Use `parseInt` for Numbers and always with a radix for type casting.
  - If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

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

    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    var val = inputValue >> 0;
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = Boolean(age);

    // good
    var hasAge = !!age;
    ```

  **[[⬆]](#TOC)**


## <a name='naming-conventions'>Naming Conventions</a>

  - Avoid single letter names. Be descriptive with your naming.

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

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    var this-is-my-object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes

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

  - Use a leading underscore `_` when naming private properties

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - When saving a reference to `this` use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

  - Name your functions. This is helpful for stack traces.

    ```javascript
    // bad
    var log = function(msg) {
      console.log(msg);
    };

    // good
    var log = function log(msg) {
      console.log(msg);
    };
    ```

  **[[⬆]](#TOC)**


## <a name='accessors'>Accessors</a>

  - Accessor functions for properties are not required
  - If you do make accessor functions use getVal() and setVal('hello')

    ```javascript
    // bad
    dragon.age();

    // good
    dragon.getAge();

    // bad
    dragon.age(25);

    // good
    dragon.setAge(25);
    ```

  - If the property is a boolean, use isVal() or hasVal()

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - It's okay to create get() and set() functions, but be consistent.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      var lightsaber = options.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    }

    Jedi.prototype.set = function(key, val) {
      this[key] = val;
    };

    Jedi.prototype.get = function(key) {
      return this[key];
    };
    ```

  **[[⬆]](#TOC)**


## <a name='constructors'>Constructors</a>

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

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

  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

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

  **[[⬆]](#TOC)**

## <a name='modules'>Modules</a>

  - The file should be named lowercase separated by underscore and match the name of the single export.
  - Always declare `'use strict';` at the top of the module.

    ```javascript
    // fancy_input.js

    !function(global) {
      'use strict';

      var previousFancyInput = global.FancyInput;

      function FancyInput(options) {
        this.options = options || {};
      }

      FancyInput.noConflict = function noConflict() {
        global.FancyInput = previousFancyInput;
        return FancyInput;
      };

      global.FancyInput = FancyInput;
    }(this);
    ```

  **[[⬆]](#TOC)**


## <a name='lint'>Lint</a>

  - Linters are tools than statically analyzes the code looking for consistency and bad patterns.
  - It just flags suspicious code, it won't prevent the code to run.
  - It's up to the developer to lint the code before commit it.

  **[[⬆]](#TOC)**


## <a name='jshint'>jsHint</a>

  - jsHint was choosen as linter because it's flexibility
  - It runs on nodeJS
  - Plugins for many IDEs/editors

  - It allows per-file specific rules, can be defined creating a `//jshint` comment

    ```javascript
    // Disable "undefined myModule" and "undefined $" warning, it's a global variable
    /*globals myModule, $ */

    // Disable unused variables warning and
    //jshint unused:false

    // Disable a concrete warning (code provided by "jshint --verbose project/js")
    //jshint -W001
    ```

  - Every file-specific rule should be preceded by a comment explaining the reason of the rule modification
  - It should be enforced to fix the file instead of adding a specific rule

Usage:

  - Command line:
    - If running by command line you should have a `.jshintrc` file at the working directory
    - It needs one argument: the path to look for `.js` files (**WARN**: Fails silently if not provided)

  - Sublime Text 2/3 plugin:
    - The plugin can be installed with [Package Control](https://sublime.wbond.net/installation), plugin name is `JSHint Gutter`
    - It requires `.jshintrc` file to be at your home directory

  **[[⬆]](#TOC)**


