---
layout: post
title:  "jQuery Plugin Development"
date:   2014-10-10 14:44:58
categories: [Web Development]
---


### 7.1 Head First Into Plugin Development
index.html
{% raw %}
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>jQuery Plugins</title>
    </head>
    <body>

    <h1>My Tweets</h1>
    <div class="tweets"></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>
    <script src="searchTwitter.jquery.js"></script>

    <script>
    $('div.tweets').queryTwitter({
      search: 'your',
      limit: 10,
      // onComplete: function() {
      //  console.log(arguments);
      // },
      refresh: 4000,
      transition: 'slideToggle'
    });

    </script>



    </body>
    </html>
{% endraw %}

searchTwitter.jquery.js
{% raw %}
    // Utility
    if ( typeof Object.create !== 'function' ) {
      Object.create = function( obj ) {
        function F() {};
        F.prototype = obj;
        return new F();
      };
    }

    (function( $, window, document, undefined ) {
      var Twitter = {
        init: function( options, elem ) {
          var self = this;

          self.elem = elem;
          self.$elem = $( elem );

          self.url = 'http://search.twitter.com/search.json';

          self.search = ( typeof options === 'string' )
            ? options
            : options.search;

          self.options = $.extend( {}, $.fn.queryTwitter.options, options );

          self.refresh( 1 );
        },

        refresh: function( length ) {
          var self = this;

          setTimeout(function() {
            self.fetch().done(function( results ) {
              results = self.limit( results.results, self.options.limit );

              self.buildFrag( results );

              self.display();

              if ( typeof self.options.onComplete === 'function' ) {
                self.options.onComplete.apply( self.elem, arguments );
              }

              if ( self.options.refresh ) {
                self.refresh();
              }
            });
          }, length || self.options.refresh );
        },

        fetch: function() {
          return $.ajax({
            url: this.url,
            data: { q: this.search },
            dataType: 'jsonp'
          });
        },

        buildFrag: function( results ) {
          var self = this;

          self.tweets = $.map( results, function( obj, i) {
            return $( self.options.wrapEachWith ).append ( obj.text )[0];
          });
        },

        display: function() {
          var self = this;

          if ( self.options.transition === 'none' || !self.options.transition ) {
            self.$elem.html( self.tweets ); // that's available??
          } else {
            self.$elem[ self.options.transition ]( 500, function() {
              $(this).html( self.tweets )[ self.options.transition ]( 500 );
            });
          }
        },

        limit: function( obj, count ) {
          return obj.slice( 0, count );
        }
      };

      $.fn.queryTwitter = function( options ) {
        return this.each(function() {
          var twitter = Object.create( Twitter );
          
          twitter.init( options, this );

          $.data( this, 'queryTwitter', twitter );
        });
      };

      $.fn.queryTwitter.options = {
        search: '@tutspremium',
        wrapEachWith: '<li></li>',
        limit: 10,
        refresh: null,
        onComplete: null,
        transition: 'fadeToggle'
      };

    })( jQuery, window, document );

{% endraw %}




---



