---
layout: post
title:  "jQuery Basics"
date:   2014-10-04 14:43:58
categories: exercise
---


###1. Simple jQuery

{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title></title>
      <style>
      ul li { color: red;}
      .emphasis { color: green;}
      </style>
      
    </head>
    <body>

    <ul>
      <li>hello</li>
      <li>hello 2</li>
      <li>hello 3</li>
    </ul>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>

    <script>
      jQuery('ul li').addClass('emphasis');
      $('ul li').addClass('emphasis');
    </script>

    </body>
    </html>
{% endraw %}


###2. Document Ready?
{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title></title>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>
      <style>
      .emphasis { font-weight: bold;}
      </style>

      <script>

      // This commented function is identical to the other one below
      // $(function() {
      //  $('li:first-child').addClass('emphasis');
      // });

      $(document).ready(function() {
        $('li:first-child').addClass('emphasis');
      });
      </script> 
    </head>
    <body>

    <ul>
      <li>hello</li>
      <li>hello 2</li>
      <li>hello 3</li>
    </ul>

    </body>
    </html>
{% endraw %}


###3. The Basics of Querying the DOM

See [api.jquery.com](http://api.jquery.com/) for functions.
{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title></title>
      <style>
      .container { color: green;}
      </style>
    </head>
    <body>

    <div class="container">
      <h1>Welcome to My Website </h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
        quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
        consequat.
      </p>

      <div class="container">
        <ul class="emphasis container">
          <li>item</li>
          <li>item 2</li>
          <li>item 2</li>
          <li>item 4</li>
          <li>item 5</li>
          <li>
            <ul>
              <li>hello from the nest.</li>
            </ul

          </li>
        </ul>
      </div>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js"></script>

    <script>
      // Chaining
      // $('ul.emphasis')
      //  .children('li')
      //    .eq(3)
      //    .prev().text('added with jQuery');

      // Parents vs Closest
      // console.log($('ul').parents('.container'));
      // console.log($('ul').closest('.container'));
    </script>

    </body>
    </html>
{% endraw %}



###4. Events 101

index.html
{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>jQuery Events</title>
      <link rel="stylesheet" href="day.css">
    </head>
    <body>

    <h1>My Website</h1>

    <button data-file="day">Day</button>
    <button data-file="night">Night</button>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>

    (function() {
      var link = $('link');
      
      $('button').on('click', function() {
        var $this = $(this),
          stylesheet = $this.data('file');

        link.attr('href', stylesheet + '.css');

        $this
          .siblings('button')
            .removeAttr('disabled')
            .end()
          .attr('disabled', 'disabled');
      });
    })();

    </script>



    </body>
    </html>
{% endraw %}

day.css
{% raw %}
    body {
      background: yellow;
    }
{% endraw %}

night.css
{% raw %}
    body {
      background: black;
      color: white;
    }
{% endraw %}



---
