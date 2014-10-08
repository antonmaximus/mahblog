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
{% endraw %}



###3.5 Full Control With Animate
No code for this one.



###3.6 Homework Solutions
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>Animate</title>
      <style>
      body { width: 400px; margin: 100px auto; }

      .box {
        width: 400px;
        background: red;
        position: relative;
        overflow: hidden;
        padding: 1em;
      }
      </style>
    </head>
    <body>


    <div class="box">
      <h2>Hi There</h2>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
      </p>
    </div>

    <p><button>FadeSlideToggle</button></p>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>

    (function() {
      var box = $('div.box');

      $.fn.FadeSlideToggle = function(speed, fn) {
        // fadeToggle = opacity
        // slideToggle = height
        return $(this).animate({
          'height': 'toggle',
          'opacity': 'toggle'
        }, speed || 400, function() {
          $.isFunction(fn) && fn.call(this);
        });
      };

      $('button').on('click', function() {
        box.FadeSlideToggle(500);
      }); 

    })();

    </script>
    </body>
    </html>
{% endraw %}




###3.7 The Obligatory Slider (First Attempt)

index.html
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>The Obligatory Slider</title>
      <style>
      body {
        width: 600px;
        margin: 100px auto 0;
      }
      * { margin: 0; padding: 0; }
      </style>
      <link rel="stylesheet" href="slider.css">
    </head>
    <body>

    <div class="slider">
      <ul>
        <li><img src="img/img1.gif" alt="image"></li>
        <li><img src="img/img2.gif" alt="image"></li>
        <li><img src="img/img3.gif" alt="image"></li>
        <li><img src="img/img4.gif" alt="image"></li>
        <li><img src="img/img1.gif" alt="image"></li>
        <li><img src="img/img2.gif" alt="image"></li>
        <li><img src="img/img3.gif" alt="image"></li>
        <li><img src="img/img4.gif" alt="image"></li>
      </ul>
    </div>

    <div id="slider-nav">
      <button data-dir="prev">Previous</button>
      <button data-dir="next">Next</button>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
    <script src="slider.js"></script>

    </body>
    </html>
{% endraw %}

slider.css
{% raw %}
    #slider-nav { 
      display: none; 
      margin-top: 1em;
    }

    #slider-nav button {
      padding: 1em;
      margin-right: 1em;
      border-radius: 10px;
      cursor: pointer;
    }

    .slider {
      width: inherit;
      height: 300px;
      overflow: scroll;
    }

    .slider ul {
      width: 10000px;
      list-style: none;
    }

    .slider li {
      float: left;
    }
{% endraw %}

slider.js
{% raw %}
    // the procedural method
    (function($) {
      var sliderUL = $('div.slider').css('overflow', 'hidden').children('ul'),
        imgs = sliderUL.find('img'),
        imgWidth = imgs[0].width, // 600
        imgsLen = imgs.length, // 4
        current = 1,
        totalImgsWidth = imgsLen * imgWidth; // 2400

      $('#slider-nav').show().find('button').on('click', function() {
        var direction = $(this).data('dir'),
          loc = imgWidth; // 600

        // update current value
        ( direction === 'next' ) ? ++current : --current;

        // if first image
        if ( current === 0 ) {
          current = imgsLen;
          loc = totalImgsWidth - imgWidth; // 2400 - 600 = 1800
          direction = 'next';
        } else if ( current - 1 === imgsLen ) { // Are we at end? Should we reset?
          current = 1;
          loc = 0;
        }

        transition(sliderUL, loc, direction);
      });

      function transition( container, loc, direction ) {
        var unit; // -= +=

        if ( direction && loc !== 0 ) {
          unit = ( direction === 'next' ) ? '-=' : '+=';
        }

        container.animate({
          'margin-left': unit ? (unit + loc) : loc
        });
      }

    })(jQuery);
{% endraw %}



###3.8 Prototypal Inheritance and Refactoring the Slider

index.html
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>The Obligatory Slider</title>
      <style>
      body {
        width: 600px;
        margin: 100px auto 0;
      }
      * { margin: 0; padding: 0; }
      </style>
      <link rel="stylesheet" href="slider.css">
    </head>
    <body>

    <div class="slider">
      <ul>
        <li><img src="img/img1.gif" alt="image"></li>
        <li><img src="img/img2.gif" alt="image"></li>
        <li><img src="img/img3.gif" alt="image"></li>
        <li><img src="img/img4.gif" alt="image"></li>
      </ul>
    </div>

    <div id="slider-nav">
      <button data-dir="prev">Previous</button>
      <button data-dir="next">Next</button>
    </div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script src="slider.js"></script>
    <script>

    (function() {
      var container = $('div.slider').css('overflow', 'hidden').children('ul'),
        slider = new Slider( container, $('#slider-nav') );

      slider.nav.find('button').on('click', function() {
        slider.setCurrent( $(this).data('dir') );
        slider.transition();
      });   
    })();



    </script>

    </body>
    </html>
{% endraw %}

{% raw %}
    #slider-nav { 
      display: none; 
      margin-top: 1em;
    }

    #slider-nav button {
      padding: 1em;
      margin-right: 1em;
      border-radius: 10px;
      cursor: pointer;
    }

    .slider {
      width: inherit;
      height: 300px;
      overflow: scroll;
    }

    .slider ul {
      width: 10000px;
      list-style: none;
    }

    .slider li {
      float: left;
    }
{% endraw %}

{% raw %}
    function Slider( container, nav ) {
      this.container = container;
      this.nav = nav.show();

      this.imgs = this.container.find('img');
      this.imgWidth = this.imgs[0].width; // 600
      this.imgsLen = this.imgs.length;

      this.current = 0;
    }

    Slider.prototype.transition = function( coords ) {
      this.container.animate({
        'margin-left': coords || -( this.current * this.imgWidth )
      });
    };

    Slider.prototype.setCurrent = function( dir ) {
      var pos = this.current;

      pos += ( ~~( dir === 'next' ) || -1 );
      this.current = ( pos < 0 ) ? this.imgsLen - 1 : pos % this.imgsLen;

      return pos;
    };
{% endraw %}


###3.9 Your Questions Answered
No code for this one.
---













