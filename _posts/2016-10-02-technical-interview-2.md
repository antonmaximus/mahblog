---
layout: post
title:  "Technical Interview Preparation 2"
date:   2016-10-02 11:00:00 -0800
categories: [Web Development]
---



## Topics:

**How many H1 should be used in a page?** You can use as many as long as you used it in context.

**What is DOM?** The Document Object Model (DOM) is a programming interface for HTML and XML documents. The DOM provides a representation of the document as a structured group of nodes and objects that have properties and methods. Essentially, it connects web pages to scripts or programming languages.

**Data Types in JavaScript.** There are 6 *primitive*  data types:  Null, Undefined, Boolean, Number, String, & Symbol, and 1 *non-primitive* data-type  Object. Primitive means incapable of being changed.

**Weakly vs Strongly?** JavaScript is a *weakly* programming language, but a better definition is it's a *loosely typed* language where you don't have do declare a variable ahead of time. 

**Timeout Scope.** Depending on where the function is defined, the function will follow its immediate scope.

**How is `this` defined in JS?** In most cases, the value of `this` is determined by how a function is called. 


**What is Dependency Injection?** Not instantiating the dependencies explicitly in the class, and, instead, declaratively expressing the dependencies in the class definition.  This enables easy replacement of the dependency concrete implementation without modifying your classes' source code.  An example would be a Constructor Injection. 

**Success and Error codes?**  Codes that indicate success are in the 2xx format. The 4xx codes are intended for cases in which the client seems to have erred, and the 5xx codes for the cases in which the server is aware that the server has erred.

**What is RESTful?**  Being RESTful means that we're not expecting the server to know what it wants. 

**Pros and cons of single-app vs multi-page?**

**Null vs Undefined?** They are both primitive data types inJavaScript. In general, The undefined value means a variable that has been declared but not assigned a value. The null value is a primitive value that represents the null, empty, or non-existent reference.

**Margins of an inline element?**  Top and bottom don't exist, while right & left can be modified. 


**What is Local Storage?** The `localStorage` property allows you to access a local `Storage` object. `localStorage` is similar to `sessionStorage`. The data stored in the former has no expiration time, while data stored in `sessionStorage` gets cleared when the browsing session ends—that is, when the browser is closed. An example of using it is `localStorage.setItem('myCat', 'Tom');`


-----

Sort an Array. Merge sort provides good time complexity (Big O(nlogn)) and a straight-forward divide & conquer algorithm. Quick sort (not shown) has better time complexity in real-life application.



    function mergeSort(arr, p, r) {
      
      // base case
      if(r === p) 
        return [arr[p]];
      
      let q = Math.floor((r-p)/2 + p) ; //half length
      
      let firstHalf = mergeSort(arr, p, q);
      let secondHalf = mergeSort(arr, q+1, r);
      
      let sorted = [];

      for(let j=0, k=0; j<firstHalf.length || k<secondHalf.length;) {
        if(firstHalf[j] < secondHalf[k] || k>=secondHalf.length) {
          sorted.push(firstHalf[j++]);
        } else {
          sorted.push(secondHalf[k++]);
        }
      }
      
      return sorted;
    }


=============

    function combineNoDups(a, b) {
      //Go through a array. Pick the shorter one.
      var shortArr;
      var longArr;
      
      if(a.length > b.length){
        longArr = a;
        shortArr = b;
      } else{
        longArr = b;
        shortArr = a;
      }
      
      for(let i=0; i<shortArr.length; i++) {
        if(longArr.indexOf(shortArr[i]) < 0) {
          longArr.push(shortArr[i]);
        }
      }
      
      console.log(longArr);
      
    }

    combineNoDups(a, b)



=============

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>Anton Accordion</title>
      <style>

        .accordion {
          margin: 0 auto;
          width: 400px;
          border: 1px solid black;
          font-family: Arial, Helvetica;
        }

        .entry {
          height: 100%;
          padding: 0;
          overflow: none;
        }

        .entry:not(:last-child) {
          border-bottom: 1px solid black;
        }

        .header {
          padding: 20px;
          background-color: #CCC;
          font-weight: bold;
        }

        .header:hover {
          cursor: pointer;
          background-color: #999;
        }

        .content {
          display: none;
          padding: 40px 20px;
        }
      </style>
    </head>
    <body>
      <div class='accordion'>
        <div class='entry'>
          <div class='header'>Header#1 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div>    
        <div class='entry'>
          <div class='header'> Header#2 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div> 
        <div class='entry'>
          <div class='header'> Header#3 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div> 
        <div class='entry'>
          <div class='header'> Header#4 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra</div>
        </div> 
      </div>

      <br >
      <br>

      <div class='accordion'>
        <div class='entry'>
          <div class='header'>Header#1 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div>    
        <div class='entry'>
          <div class='header'> Header#2 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div> 
        <div class='entry'>
          <div class='header'> Header#3 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra </div>
        </div> 
        <div class='entry'>
          <div class='header'> Header#4 </div>
          <div class='content'> Bacon ipsum dolor amet spare ribs turkey alcatra</div>
        </div> 
      </div>

      <script>
      'use strict';
      class Accordion {
        constructor(a) {
          this.accordion = a;
          this.activeEntry = null;

          let entries = a.querySelectorAll('.entry');
          for(let i=0; i<entries.length; i++) {
            let entry = entries[i];
            entry.querySelector('.header').addEventListener('click', ()=>{this.actionAcc(entry);} );
          }
        }

        updateEntry(entry) {
          this.activeEntry = entry;
        }

        actionAcc(newEntry){
          newEntry.querySelector('.header').style.display = 'none';
          newEntry.querySelector('.content').style.display = 'block';

          if(this.activeEntry !== null) {
            this.activeEntry.querySelector('.header').style.display = 'block';
            this.activeEntry.querySelector('.content').style.display = 'none';
          }

          this.updateEntry(newEntry)
        }
      }


      let accordions = document.querySelectorAll('.accordion');
      accordions.forEach(function(element){new Accordion(element);});


      </script>
    </body>
    </html>

=============

    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width">
      <title>Anton Wireframe</title>
      <style>


        #page-wrap {
          width: 800px;
          margin: 0 auto;
          font-size: 0;
        }

        #page-wrap * {
          box-sizing: border-box;
        }

        #page-wrap > * > * {
          border: 1px solid black;
        }

        footer {
          border: 1px solid black;
          margin-top: 15px;
          width: 800px;
          height: 44px;
        }

        .left {
          width: 232px;
          margin-right: 11px;
          display: inline-block;
        }

        .main {
          width: 557px;
          display: inline-block;
        }

        #logo, header {
          height: 82px;
          margin-bottom: 11px;
        }

        nav {
          height: 49px;
          margin-bottom: 15px;
        }

        main {
          height: 384px;
        }

        aside {
          height: 210px;
          margin-bottom: 33px;
        }

        #ad {
          height: 205px;
        }




      </style>
    </head>
    <body>
      <div id='page-wrap'>
        <section class='left noborder'>
          <div id='logo'></div>
          <aside></aside>
          <section id='ad'></section>
        </section>

        <section class='main noborder'>
          <header></header>
          <nav></nav>
          <main></main>
        </section>

        <footer></footer>
      </div>


    </body>
    </html>













