---
layout: post
title:  "Gee Dude. Interviewing 2"
date:   2016-03-15 11:00:00 -0800
categories: craft
---


## What Worked

* You practiced courage in taking a high-stake, anxiety-inducing situation.
* You studied for the short-term.


## What Didn't

* You weren't ready... like long-term ready.  There are things that you can review up on in the imminent term, and then there are things that could only be learned in the long-term.
* Scheduled the talk a tad bit earlier than your normal functioning hours.
* You didn't effuse and laugh.


## Topics:

**Stacks vs. Queues**. For the former, a recursive function and a depth-first search are good examples of stack usage that allows you to go back a previous state. For queue usage, breadth-first search is a good approach where you go through one-level at a time.



**Interpreted vs. Compiled language**.  JavaScript is an interpreted language, not a compiled language (well, not anymore because of Google's V8 that does compilation). A program such as C++ or Java needs to be compiled before it is run. The source code is passed through a program called a compiler, which translates it into bytecode that the machine understands and can execute. In contrast, JavaScript has no compilation step. Instead, an interpreter in the browser reads over the JavaScript code, interprets each line, and runs it. More modern browsers use a technology known as Just-In-Time (JIT) compilation, which compiles JavaScript to executable bytecode just as it is about to run. [Source](http://web.stanford.edu/class/cs98si/slides/overview.html).  Futhermore, another good explanation [can be found here](https://www.quora.com/What-is-the-difference-between-a-compiler-and-an-interpreter/answers/7670223).


**What is a JavaScript Engine?**

A JavaScript engine is a program or library which executes JavaScript code. A JavaScript engine may be a traditional interpreter, or it may utilize just-in-time compilation to bytecode in some manner. Although there are several uses for a JavaScript engine, it is most commonly used in web browsers


**What does asynchronous mean in JavaScript?**

A good video on event loop and where asynchronous come into play: [Video Here](https://www.youtube.com/watch?v=8aGhZQkoFbQ).

* JavaScript is a single-threaded programming language (i.e., one call stack, one thing at a time).

* The JavaScript run-time can only do one thing at a time. The reason we can do things concurrently is because the browser is more than the just run-time.  The browser gives us these other things, like APIs, that are effectively like threads, and they are aware of this conconcurrency (12:21 in the video).

* When the API is done with the asychronous call, it pushes the  callback function to the task queue.  The **event loop** then does it job -- that is to look at the stack and the task queue, and if the stack is empty it takes the first thing in the task queue and push it on to the stack.


**What's Lexical Scoping?**  In JavaScript, the scope of a variable is defined by its location within the source code (it is apparent lexically) and nested functions have access to variables declared in their outer scope.


**What's a closure?**  A closure is a special kind of object that combines two things: a function, and the environment in which that function was created. The environment consists of any local variables that were in-scope at the time that the closure was created. 

**What's cross-site scripting?**  Cross-site scripting (XSS) is a type of computer security vulnerability typically found in web applications. XSS enables attackers to inject client-side scripts into web pages viewed by other users. A cross-site scripting vulnerability may be used by attackers to bypass access controls such as the same-origin policy.

**What's a Hashtable**. A hash table (hash map) is a data structure used to implement an associative array, a structure that can map keys to values.

Remove duplicates from an array using a hashtable. 

      'use strict';

      let myArr = ['apple', 'orange', 'mango', 'apple'];

      function removeDuplicates(arr) {
        let itemInstances = {};
        for(let i = 0; i < arr.length; i++ ) {
          let currItem = arr[i];
          itemInstances[currItem] = itemInstances[currItem] ? 
            itemInstances[currItem] + 1 : 1;
        }
        
        console.log(itemInstances);
        
        let newArray = [];
        for (let key in itemInstances) {
          newArray.push(key);
        }
        // OR newArray = Object.keys(itemInstances);

        return newArray;
      }


      console.log(removeDuplicates(myArr));


Compare two strings and check if first string can be rearranged as the second string.

      'use strict';

      function compareStrings(string1, string2) {
          if(string1.length !== string2.length) 
              return false;

          var usedLetters = []; // to keep track of instances
          
          // Count instances of letters first, so we can handle duplicates.
          for(var i=0; i<string1.length; i++) {
            var currLet = string1[i]; // current letter
            usedLetters[currLet] = usedLetters[currLet] ? 
              usedLetters[currLet] + 1 : 1;
          }
          
          // Compare if instances are in the second string
          for(var j=0; j<string2.length; j++) {
            var currLet = string2[j];
            var instances = usedLetters[currLet];
            
            if (instances >= 1)
              usedLetters[currLet] -= 1;
            else 
              return false;
          }
          
          return true;
      }


      console.log(compareStrings('abca', 'abec'));


**When is an Array Used instead of Linked List? Vice Versa?**

* Arrays: The size of the arrays is fixed (in most languages).  Inserting a new element in an array of elements is expensive, because room has to be created for the new elements and to create room existing elements have to be shifted.  Arrays allows for better access performance which is O(1);

* Linked List: The size is dynamic and allows for ease in insertion/deletion. A drawback of Linked List is it does not allow for random access and search has to be done sequentially.  



**How to persist data between page loads?**

* **Web Storage.** With the introduction of HTML5 we also got Web Storage, which allows you to save information in the browser across page loads. There is `localStorage` which can save data for a longer period (as long as the user doesn't manually clear it) and `sessionStorage` which saves data only during your current browsing session.

* **Cookies.** An alternative to Web Storage is saving the data in a cookie. Cookies are mainly made to read data server-side, but can be used for purely client-side data as well.

* **window.name.** Although this seems like a hack that probably originated from before `localStorage`/`sessionStorage`, you could store information in the `window.name` property: `window.name = "my value"`.

* **Query string.**  When submitting a form using the GET method, the url gets updated with a query string (?parameter=value&something=42). You can utilize this by setting an input field in the form to a certain value. 

<s>Compare @Autowired vs @Resource</s>


Closure Example

    function Counter() {
        var count = 0;
        
        return function(){
          return ++count;
        }
    }

    var myCounter = Counter();
    console.log(myCounter()); // outputs "1"
    console.log(myCounter()); // outputs "2"
    
    var myCounter2 = Counter();
    console.log(myCounter2()); // outputs "1"
    console.log(myCounter()); // outputs "3"
    console.log(myCounter2()); // outputs "2"


---



