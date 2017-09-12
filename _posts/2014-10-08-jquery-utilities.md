---
layout: post
title:  "jQuery Utilities"
date:   2014-10-08 14:43:58
categories: [Web Development]
---


### 4.1 $.each and Templating

{% raw %}{{
    <!doctype html>
    <html>
    <head>
      <meta charset=utf-8>
      <title>Q&A</title>
    </head>
    <body>

    <script id=blogTemplate type=tuts/template>
      <h2> {-{title}-} </h2>
      <img src={-{thumbnail}-} alt={-{title}-}>
    </script>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>

    (function() {
      var content = [
        {
          title: 'My awesome blog post',
          thumbnail: 'http://d2o0t5hpnwv4c1.cloudfront.net/1137_terminal/preview.png',
        },
        {
          title: 'My second awesome blog post',
          thumbnail: 'http://d2o0t5hpnwv4c1.cloudfront.net/1133_newrelic/200.jpg',
        },
        {
          title: 'My third blog post',
          thumbnail: 'http://d2o0t5hpnwv4c1.cloudfront.net/1137_terminal/preview.png',
        }
      ],
        template = $.trim( $('#blogTemplate').html() ),
        frag = '';



      $.each( content, function( index, obj ) {
        frag +=
          template.replace( /{-{title}-}/ig, obj.title )
              .replace( /{-{thumbnail}-}/ig, obj.thumbnail );   
      });

      $('body').append(frag);

    })();

    </script>
    </body>
    </html>
}}{% endraw %}
   

### 4.2 Say Hello to Handlebars
{% raw %}{{
    <html>
    <head>
      <meta charset=utf-8>
      <title>Mustache</title>
    <style>
    h2 span { color: gray; font-size: .8em; }
    </style>
    </head>
    <body>

    <ul class="tweets">
      <script id="template" type="text/x-handlebars-template">
        {-{#each this}-}
        <li>
          <h2>{-{fullName author}-}</h2>
          <p>{-{tweet}-}}</p>

          {-{#if quote}-}
            <h5>{-{quote}-}</h5>
          {-{else}-}
            <h5>Author does not have a quote.</h5>
          {-{/if}-}
        </li>
        {-{/each}-}
      </script>
    </ul>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script src="http://cloud.github.com/downloads/wycats/handlebars.js/handlebars-1.0.0.beta.6.js"></script>

    <script>

    (function() {
      var context = [
        {
          author: { first: 'Jeffrey', last: 'Way', age: 27 },
          tweet: '30 Days to Learn jQuery Rocks',
          quote: 'Never ever, ever, ever give up.'
        },
        {
          author: { first: 'John', last: 'Doe', age: 45 },
          tweet: '<strong>30 Days</strong> to Learn jQuery Rocks',
        }
      ],
        template = Handlebars.compile( $('#template').html() );

      Handlebars.registerHelper('fullName', function( author ) {
        return author.first + ' ' + author.last + ' - ' + author.age;
      });

      $('ul.tweets').append( template(context) );

    })();

    </script>


    </body>
    </html>
}}{% endraw %}


### 4.3 The Twitter API
{% raw %}{{
    <html>
    <head>
      <meta charset=utf-8>
      <title>Twitter</title>
      <style>
      body { width: 600px; margin: auto; }
      ul { list-style: none; }
      li { padding-bottom: 1em; }
      img { float: left; padding-right: 1em; }
      a { text-decoration: none; color: #333; }
      </style>
    </head>
    <body>

    <ul id="biebster-tweets">
      <script id="tweets-template" type="text/x-handlebars-template">
        {-{#each this}-}
        <li>
          <img src="{-{thumb}-}" alt="{-{author}-}">
          <p><a href="{-{url}-}">{-{tweet}-}</a></p>
        </li>
        {-{/each}-}
      </script>   
    </ul>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script src="http://cloud.github.com/downloads/wycats/handlebars.js/handlebars-1.0.0.beta.6.js"></script>

    <script>

    (function() {

      var Twitter = {
        init: function( config ) {
          this.url = 'http://search.twitter.com/search.json?q=' + config.query + '&callback=?';
          this.template = config.template;
          this.container = config.container;

          this.fetch();
        },

        attachTemplate: function() {
          var template = Handlebars.compile( this.template );

          this.container.append( template( this.tweets ) );

        },

        fetch: function() {
          var self = this;

          $.getJSON( this.url, function( data ) {
            self.tweets = $.map( data.results, function( tweet ) {
              return {
                author: tweet.from_user,
                tweet: tweet.text,
                thumb: tweet.profile_image_url,
                url: 'http://twitter.com/' + tweet.from_user + '/status/' + tweet.id_str
              };
            });

            // For future lessons, research $.deferred, viewers. :)
            self.attachTemplate(); 
          });
        }
      };

      Twitter.init({
        template: $('#tweets-template').html(),
        container: $('#biebster-tweets'),
        query: 'Justin Bieber'
      });

    })();

    </script>


    </body>
    </html>
}}{% endraw %}



### 4.4 Filtering with jQuery.grep
{% raw %}{{
    <html>
    <head>
      <meta charset=utf-8>
      <title>$.grep</title>
    </head>
    <body>


    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js"></script>

    <script>
      (function() {
        
        var arr = [
          {
            first: 'Jeffrey',
            last: 'Way'
          },
          {
            first: 'Allison',
            last: 'Way'
          },
          {
            first: 'Jeffrey',
            last: 'Smith'
          },
          {
            first: 'John',
            last: 'Doe'
          },
          {
            first: 'Thomas',
            last: 'Way'
          }
        ];

        arr = $.grep( arr, function( obj, index) {
          return obj.last === 'Way';
        });

        console.log(arr);

      })();
    </script>

    </body>
    </html>
{% raw %}{{


















---













