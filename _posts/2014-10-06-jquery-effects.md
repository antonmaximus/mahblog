---
layout: post
title:  "jQuery Effects"
date:   2014-10-06 14:43:58
categories: craft
---


###3.1 Slides and Structure

{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>Slides and Structure</title>
      <style>

      body {
        width: 600px;
        margin: auto;
        font-family: sans-serif;
      }

      #contact { 
        background: #e3e3e3; 
        padding: 1em 2em; 
        position: relative;
      }

      .js #contact {
        position: absolute;
        top: 0;
        width: inherit;
        display: none;
      } 

      #contact h2 { margin-top: 0; }

      #contact ul { padding: 0; }

      #contact li { 
        list-style: none;
        margin-bottom: 1em;
      }

      /* Close button on form */
      .close {
        position: absolute;
        right: 10px;
        top: 10px;
        font-weight: bold;
        font-family: sans-serif;
        cursor: pointer;
      } 

      /* Form inputs */
      input, textarea { width: 100%; line-height: 2em;}
      input[type=submit] { width: auto;  }
      label { display: block; text-align: left; }

      </style>
    </head>
    <body>


    <article>
      <h1>My Awesome Post</h1>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
      </p>
    </article>

    <div id="contact">
      <h2>Contact Me</h2>
      <form action="#">
        <ul>
          <li>
            <label for="name">Name: </label>
            <input name="name" id="name">
          </li>

          <li>
            <label for="email">Email Address: </label>
            <input name="email" id="email">
          </li>

          <li>
            <label for="comments">What's Up?</label>
            <textarea name="comments" id="comments" cols="30" rows="10"></textarea>
          </li>
          <li>
            <input type="submit" value="Submit">
          </li>
        </ul>
      </form>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>

    (function() {
      
    $('html').addClass('js');

    var contactForm = {

      container: $('#contact'),

      config: {
        effect: 'slideToggle',
        speed: 500
      },

      init: function(config) {
        $.extend(this.config, config);

        $('<button></button>', {
          text: 'Contact Me'
        })
          .insertAfter( 'article:first' )
          .on( 'click', this.show );
      },

      show: function() {
        var cf = contactForm,
          container = cf.container,
          config = cf.config;

        if ( container.is(':hidden') ) {
          contactForm.close.call(container);
          // container[config.effect](config.speed);
          container.fadeToggle();
        }
      },

      close: function() {
        var $this = $(this), // #contact
          config = contactForm.config;

        if ( $this.find('span.close').length ) return;

        $('<span class=close>X</span>')
          .prependTo(this)
          .on('click', function() {
            // this = span
            $this[config.effect](config.speed);
          })
      }
    };

    contactForm.init({
      // effect: 'fadeToggle',
      speed: 300
    });

    })();

    </script>

    </body>
    </html>
{% endraw %}



###3.2 The `this` Keyword
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>The this Keyword</title>
    </head>
    <body>
      <a href="http://tutsplus.com">Click Me</a>


      <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

      <script>
        var obj = {
          doIt: function(e) {
            console.log(this);
            e.preventDefault();
          }
        }

        $('a').on( 'click', $.proxy(obj.doIt, obj) )


        // $('a').on('click', function(e) {
        //  // this = anchor that was clicked
        //  obj.doIt.call(this);

        //  e.preventDefault();
        // });

      </script>

    </body>
    </html>
{% endraw %}




###3.3 Modifying Effect Speeds (and utilizing jQuery.extend)
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>Custom Effect Methods</title>
      <style>
      p { margin-top: 0;}
      </style>
    </head>
    <body>


    <h1>Reveal</h1>
    <div class=content>
      <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>
      <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>  
    </div>



    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>

    $('div.content').hide();

    // $.fx.speeds._default = 2000;
    $.fx.speeds.tortoise = 5000;

    $('h1').on('click', function() {
      $(this).next().slideDown('fast');
    })

    $.fn.flashFlash = function() {
      $(this).slideDown(500, function() {
        $(this).delay(1000).slideUp(500);
      })
    }

    $('div').flashFlash();


    </script>

    </body>
    </html>
{% endraw %}








###3.4 Creating Custom Effect Methods
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>Custom Effect Methods</title>
      <style>
      p { margin-top: 0;}
      </style>
    </head>
    <body>

    <h1>Click Me</h1>

    <div class=content>
      <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>
      <p>
      Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
      tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
      quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
      consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
      cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>  
    </div>



    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>

    (function() {
      var content = $('div.content').hide();

      jQuery.fn.flash = function( speed, easing, callback ) {
        var $this = $(this);

        return $this.slideDown(500, function() {
          $this.delay(2000).slideUp(500);
        });
      };

      $('h1').on('click', function() {
        content.flash();
      }); 
    })();



    </script>

    </body>
    </html>
{% raw %}



###3.5 Full Control With Animate
No code for this one.















---













