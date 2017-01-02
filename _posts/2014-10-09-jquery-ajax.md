---
layout: post
title:  "jQuery AJAX"
date:   2014-10-09 14:44:58
categories: [Web Development]
---


### 6.1 Loading Pages Asynchronously
index.html
{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>AJAX: load</title>
    </head>
    <body>

    <nav>
      <ul>
        <li><a href="about.html">About</a></li>
        <li><a href="contact.html">Contact</a></li>
      </ul>
    </nav>

    <div class="wrap"></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>

    (function() {
      var wrap = $('div.wrap');

      $('a').on('click', function( e ) {
        var href = $(this).attr('href');

        wrap.load( href + ' .container' );

        e.preventDefault();
      }); 
    })();

    </script>

    </body>
    </html>

{% endraw %}

about.html
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>AJAX: load</title>
    </head>
    <body>


    <div class="container">
      <h2>About Me</h2>
      <p>I work for Envato!</p>
    </div>



    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    </body>
    </html>
{% endraw %}

contact.html
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>AJAX: load</title>
    </head>
    <body>


    <div class="container">
      <h2>Contact Me</h2>
      <p>Represent contact form.</p>
    </div>



    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    </body>
    </html>
{% endraw %}




### 6.2 Interacting with the Server-Side
index.html
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>AJAX: POST</title>
    </head>
    <body>

    <h1>Something to Save</h1>
    <form action="#">
      <textarea name="content" id="content" cols="30" rows="10"></textarea>
      <p><button type="submit">Save</button></p>
    </form>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>

      $.post( 'load.php', function(data) {
        if ( data ) {
          $('#content').val( data );
        }
      });


      // listen for click
      $('form').on('submit', function(e) {
        $.post( 'save.php', $(this).serialize(), function(response) {
          alert( response );
        });
        // disable default action
        e.preventDefault();
      });

      

      // grab textarea content

      // post content to script, and save
    </script>
      
    </body>
    </html>
{% endraw %}

save.php
{% raw %}
    <?php

    $f = fopen('data.txt', 'w');
    fwrite( $f, $_POST['content']);
    fclose($f);

    echo 'Comment has been saved';
{% endraw %}

load.php
{% raw %}
    <?php

    $data = file( 'data.txt');
    echo stripslashes( $data[0] );
{% endraw %}


### 6.3 
index.html

{% raw %}
{% endraw %}

### 6.5 Deferreds
{% raw %}
    <html>
    <head>
      <meta charset=utf-8>
      <title>jQuery Deferreds</title>
    </head>
    <body>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>

    function getTweets( search ) {
      return $.ajax({
        url: 'http://search.twitter.com/search.json',
        data: { q: search },
        dataType: 'jsonp'
      });
    }


    $.when( getTweets('dogs'), getTweets('cats') ).done(function(results1, results2) {
      console.log(results1);
      console.log(results2);
    });

      
    // $.searchTwitter = function( search ) {
    //  return $.ajax({
    //    url: 'http://search.twitter.com/search.json',
    //    data: { q: search },
    //    dataType: 'jsonp'
    //  }).promise();
    // };

    // var outer = $.searchTwitter('cats');

    // outer.then(function  ( results ) {
    //  console.log(results);
    // });

    // // ...

    // outer.then(function(results) {
    //  console.log(results);
    // });



    </script>

    </body>
    </html>
{% endraw %}




---



