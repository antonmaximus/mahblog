---
layout: post
title:  "jQuery Custom Events"
date:   2014-10-09 14:43:58
categories: [Web Development]
---


### 5.1 Custom Events and the Observer Pattern
index.html

{% raw %}{{

    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>Custom Events</title>
    </head>
    <body>

    <h1>Hi There</h1>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>
      
    // PubSub
    (function( $ ) {
      var o = $( {} );

      $.each({
        trigger: 'publish',
        on: 'subscribe',
        off: 'unsubscribe'
      }, function( key, val ) {
        jQuery[val] = function() {
          o[key].apply( o, arguments );
        };
      });
    })( jQuery );


    $.getJSON('http://search.twitter.com/search.json?q=dogs&callback=?', function( results) { 
      $.publish( 'twitter/results', results );
    });

    // ...
    $.subscribe( 'twitter/results', function( e, results ) {
      $('body').html(
        $.map( results.results, function( obj, index) {
          return '<li>' + obj.text + '</li>';
        }).join('')
      );
    });


    </script>

    </body>
    </html>



tweets.html


    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>Custom Events</title>
      <style>
      body { width: 600px; margin: auto; font-family: sans-serif; text-align: center; }
      li { text-align: left; padding-bottom: 1em; }
      </style>
    </head>
    <body>

    <h2>What Are You Interested In?</h2>

    <form action="#">
      <p><input type="text" name="q" id="q"></p>
    </form>

    <ul class="tweets"></ul>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>


    <script>

    (function($) {
      var o = $( {} );

      $.each({
        on: 'subscribe',
        trigger: 'publish',
        off: 'unsubscribe'
      }, function( key, api ) {
        $[api] = function() {
          o[key].apply( o, arguments );
        }
      });

    })(jQuery);


    (function($) {
        
      var Twitter = {
        init: function() {
          this.template = '<li>{{tweet}}</li>';
          this.query = '@tutspremium';
          this.tweets = [];
          this.timer;

          this.cache();
          this.bindEvents();
          this.subscriptions();


          $.publish( 'twitter/query' );
          this.searchInput.val( this.query );


          return this;
        },

        cache: function() { 
          this.container = $('ul.tweets');
          this.searchInput = $('#q');
        },

        bindEvents: function() {
          this.searchInput.on( 'keyup', this.search );
        },

        subscriptions: function() {
          $.subscribe( 'twitter/query', this.fetchJSON );
          $.subscribe( 'twitter/results', this.renderResults );
        },

        search: function() {
          var self = Twitter,
            input = this;

          clearTimeout( self.timer );

          self.timer = ( input.value.length >= 3 ) && setTimeout(function() {
            self.query = input.value;
            $.publish( 'twitter/query' );
          }, 400);
        },

        fetchJSON: function() {
          var url = 'http://search.twitter.com/search.json?callback=?&q=';

          return $.getJSON( url + Twitter.query, function( data ) {
            Twitter.tweets = data.results;
            $.publish( 'twitter/results' );
          });
        },

        renderResults: function() {
          var self = Twitter,
            frag = [],
            tweet;

          self.container.html(
            $.map( self.tweets, function( obj, index ) {
              var t = 
                obj.text.replace(/(http:[^\s]+)/, '<a href="$1">$1</a>')
                    .replace(/@([^\s:]+)/, '<a href="http://twitter.com/$1">@$1</a>');

              return self.template.replace(/{{tweet}}/, t);
            }).join('')
          );
        }
      };

      window.Twitter = Twitter.init();

    })(jQuery);

    </script>

    </body>
    </html>
    
}}{% endraw %}

---



