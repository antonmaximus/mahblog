---
layout: post
title:  "Anatomy of Backbone B1"
date:   2014-11-06 14:00:00
categories: craft
---


## 1. More Models

### Parsing non-standard JSON into your Models

    // Response from Server
    {todo:{id: 1, description: 'Pick up milk', status: 'incomplete' }}

<!-- tsk -->
Defaul Backbone `parse` just returns response, so we edit it like so...

    var TodoItem = Backbone.Model.extend({
      parse: function(response){
    return response.todo; }
    });

<!-- tsk -->
Instantiating Models, doesn't call parse by default. So we call it with `parse: true`

    var todoItem = new TodoItem({
      todo:{id: 1, description: 'Pick up milk', status: 'incomplete'}
    }, { parse: true });



### Changing Attribute Names

    // Response from Server
    {todo:{id: 1, desc: 'Pick up milk', status: 'incomplete'}}

<!-- tsk -->
    var TodoItem = Backbone.Model.extend({
      parse: function(response){
        response = response.todo;
        response.description = response.desc;
        delete response.desc;
        return response;
      }
    });


### Sending JSON back to the Server

Defaut Backbone `toJSON()` 

    var TodoItem = Backbone.Model.extend({
      toJSON: function(){
        return _.clone(this.attributes);
      }
    });

Overridng the `toJSON()` function like so...

    var TodoItem = Backbone.Model.extend({
      toJSON: function(){
        var attrs = _.clone(this.attributes);
        attrs.desc = attrs.description;
        attrs = _.pick(attrs, 'desc', 'status');  //returns an object with desc & status properties
        return { todo: attrs };
    } });

Then, replace Render View attributes from `toJSON()` to `attributes`

    var TodoView = Backbone.View.extend({
      render: function(){
        this.$el.html(this.template(this.model.attributes));  // this used to be .toJSON()
        return this;
      }
    });

### Unconventional ID Attribute

Common way:

    var todoItem = new TodoItem({id: 1})

Workaround:

    var TodoItem = Backbone.Model.extend({
      idAttribute: '_id'
    });


## 2. Customizing Collections

### Review Fetching Data from the Server

    // URL to get JSON data from
    var TodoItems = Backbone.Collection.extend({
      url: '/todos'
    });

<!-- tsk -->
    // Populate collection from server
    todoItems.fetch();


<!-- tsk -->
    // Response from server after calling `fetch()`
    [
      {description: 'Pick up milk.', status: 'incomplete', id: 1},
      {description: 'Get a car wash.', status: 'incomplete', id: 2}
    ]

### Handling non-standard Response from Server

    todoItems.fetch()

<!-- tsk -->
    //Response from Server
    {
      "total": 25, "per_page": 10, "page": 2,
      "todos": [ {"id": 1, ... }, {"id": 2, ... } ]
    }   

Common Way to parse:

    var TodoItems = Backbone.Collection.extend({
      parse: function(response){
        return response;
      }
    });

New Way to accommodate non-standard format:

    var TodoItems = Backbone.Collection.extend({
      parse: function(response){
        this.perPage = response.per_page;
        this.page = response.page;
        this.total = response.total; return response.todos;
      } 
    });


### Fetching with Extra Parameters

    todoItems.fetch({data: { page: 6 }}); // GET /todos?page=6

<!-- tsk -->
    todoItems.fetch({data: {page: todoItems.page + 1}}); // GET /todos?page=2

Review on Collection View

    var TodosView = Backbone.View.extend({
      initialize: function(){
        this.collection.on('reset', this.render, this);
      },
      render: function(){
        this.addAll();
        return this;
      },
      addAll: function(){
        this.$el.empty();
        this.collection.forEach(this.addOne);
      },
      addOne: function(todoItem){
        var todoView = new TodoView({model: todoItem});
        this.$el.append(todoView.render().el);
      }
    });

Render the Next Link

    var TodosView = Backbone.View.extend({
      template: _.template('<a href="#/todos/p<%= page %>">next page</a>'),  // !!! Add this line

      initialize: function(){
        this.collection.on('reset', this.render, this);
      },

      render: function(){
        this.addAll();
        this.$el.append(this.template({page: this.collection.page + 1}); // !!! Add this line too
        return this;
      },
      ... 
    });


Review our Router

    var TodoApp = new (Backbone.Router.extend({
      routes: {
        "": "index"
      },
      initialize: function(){
        this.todoItems = new TodoItems();
        this.todosView = new TodosView({collection: this.todoItems});
        this.todosView.render();
        $('#app').append(this.todosView.el);
      },
      index: function(){
        this.todoItems.fetch();
      }
    });

Implementing the Base Route
  
    var TodoApp = new (Backbone.Router.extend({
      routes: {
        "todos/p:page": "page",  // !!! Add this line
          "": "index"
      },

      page: function(page){  // !!! Add this function too
        this.todoItems.fetch({data: {page: page}});
      },

      initialize: function(){
        this.todoItems = new TodoItems();
        this.todosView = new TodosView({collection: this.todoItems});
        this.todosView.render();
        $('#app').append(this.todosView.el);
      },

      index: function(){
        this.todoItems.fetch();
      }
    });
      
### Sorting Collections


Sort by Comparator `status`

    var TodoItems = Backbone.Collection.extend({
      comparator: 'status';
    });

<!-- tsk -->
    todoItems.fetch(); // When this is called, the collection gets sorted by status

Sort by Function

    var TodoItems = Backbone.Collection.extend({
      comparator: function(todo1, todo2) {
        return todo1.get('status') < todo2.get('status')
      }
    });
    
<!-- tsk -->
    // When this is called, the collection gets sorted by status in reverse order
    todoItems.fetch(); 

### Aggregate Values

Display amount of completed todo items

    var TodoItems = Backbone.Collection.extend({
      completedCount: function() {
        return this.where({status: 'complete'}).length;
      }
    });

    todoItems.completedCount(); // This returns 1



## 3. Real Routes

### Optional Routes

Use parentheses for optional parts like so: `(/p:page)`.  
Optional Trailing Slash at the end of the URI can be accommodated by adding `(/)`

    var TodoRouter = new (Backbone.Router.extend({ 
      routes: {
        'search/:query(/p:page)(/)': 'search',
      },
      search: function(query, page) {
        page = page || 0;
        console.log(query);
        console.log(page);
      } 
    }));


<!-- tsk -->
    // Calling the above would yield...
    TodoRouter.navigate('search/milk', {trigger: true});  // ==> "milk", 0

<!-- tsk -->
    TodoRouter.navigate('search/milk/p2', {trigger: true}); // ==> "milk", 2

<!-- tsk -->
    // For the trailing slash
    TodoRouter.navigate('search/milk/p2/', {trigger: true}) ; // ==> "milk", 2


URI with Spaces Gotcha


Use `decodeURIComponent` like so...

    var TodoRouter = new (Backbone.Router.extend({
      routes: {
        'search/:query(/p:page)(/)': 'search',
      },
      search: function(query, page) {
        page = page || 0;
        query = decodeURIComponent(query);
        console.log(query);
        console.log(page);
      }
    }));

<!-- tsk -->
    // Encoded input ==> Decoded output
    TodoRouter.navigate('search/Hello%20World/p2', {trigger: true})  // ==> "Hello World", 2


### Regex in Routes

To handle regex, we use `Router.route` format...

    var TodoRouter = new (Backbone.Router.extend({
      initialize: function(){ 
        this.route(/^todos\/(\d+)$/, 'show');
      },

      show: function(id) {
        console.log("id = " + id);
      } 
    }));


!!! Needs more data


### Catch-all Routes

Alert user when no route matches

    var TodoRouter = Backbone.Router.extend({
      routes: {
        '*path': 'notFound'
      },
      notFound: function(path) {
        alert('Sorry!  There is no content here.');
    } });

<!-- tsk -->
    TodoRouter.navigate('nothinghere', {trigger: true});

### File Path Route

Accept a file path and get its parts

    var TodoRouter = new (Backbone.Router.extend({
      routes: {
        'file/*path': 'file'
      },
        file: function(path) {
        var parts = path.split("/");
        console.log(parts);
      } 
    }));

<!-- tsk -->
    TodoRouter.navigate("file/this/is/a/file.txt", {trigger: true});
    // ==> ["this", "is", "a", "file.txt"]
    ￼￼


## 4. Varying Views

### View Initialization Options Review  

    //Pass in the Model
    var todoView = new TodoView({
      model: todoItem
    });

<!-- tsk -->
//Pass in the Collection
var todoView = new TodoView({
  collection: todoItems
});


### Using Existing DOM Elements as a parameter

You can use the existing `div`  (or any element) in the DOM instead of 
having the view instance to create a new one.

    //HTML
    <div class="todo"></div>

<!-- tsk -->
    var TodoView = Backbone.View.extend({
      template: _.template("<%= description %>"),
      render: function(){
        this.$el.html(this.template(this.model.attributes));
        return this;
      }
    });

<!-- tsk -->
    // !!! Take note of the `el` option
    var todoView = new TodoView({model: todoItem, el: $('.todo')});  

<!-- tsk -->
    todoView.render(); // ==> <div class="todo">Pickup Milk.</div>


### Custom Initialization Options

Pass in an extra option. 

    var todoView = new TodoView({
      model: todoItem,
      user: currentUser
    });


Access extra options in initialize

    var TodoView = Backbone.View.extend({
      initialize: function(options){
        this.user = options.user;
      }
    });

### Escaping User Content

Rendering user supplied strings can lead to XSS attack.  You can use 

    var TodoView = Backbone.View.extend({
    template: _.template('<%= model.escape("description") %>'),
    render: function(){
    this.$el.html(this.template({model: this.model}));
        return this;
      }
    });

<!-- tsk -->
    var todoView = new TodoView({model: todoItem});

<!-- tsk -->
    todoItem.set('description', "<script src='attack.js' />")


When, `todoView.render().el` is called, this outputs:

    todoView.render().el; // ==> <div>&lt;script src=&#x27;attack.js&#x27; /&gt;</div>


### Passing Extra Options to Event Handlers

    ￼
    var TodoView = Backbone.View.extend({
      template: _.template('<span><%= description %></span>'),
      initialize: function(){
        this.model.on('change:description', this.change, this);
      },
      render: function(){
        this.$el.html(this.template(this.model.attributes));
      },
      change: function(model, value, options){
          this.$('span').html(value);
        if (options.highlight !== false){
          this.$el.effect("highlight", {}, 1000);
        }
      }, 

    });

To make change without firing off events, pass `silent: true`

    todoItem.set({description: "Pickup Kids"}, {silent: true});

To stop specific action (e.g., highlighting), edit the function above to accommodate
an `if` check, then pass that option

    todoItem.set({description: "Pickup Kids"}, {highlight: false});


### View Event Cleanup

**Old Approach:** Using model to notify the view.  

    var TodoView = Backbone.View.extend({
      initialize: function() {
        this.model.on('change', this.render, this);
      }
    });

When the view instance is removed using `todoView.remove()`, this breaks
because Model still holds reference to the view.


**New Approach:** Use view to "listen to" model.  Added in Backbone 0.9.9

    var TodoView = Backbone.View.extend({
      initialize: function() {
        this.listenTo(this.model, 'change', this.render);
      }
    });

<!-- tsk -->
    todoView.stopListening(); // Stops all listeners for this view instance

<!-- tsk -->
    todoView.remove(); //Automatically calls stopListening()



## 5. Working with Forms

### The Plain (no Backbone) Way

Create an Ajax form for creating new Todos

    <form action="/todos" method="POST">
        <input name=description value="What do you need to do?" /> 
        <button>Save</button>
    </form>

<!-- tsk -->
    // Plain jQuery
    $('button').click(function(e){
      e.preventDefault();
      var uri = $('form').attr('action');
      var description = $('input[name=description]').val();
      $.post(uri, {description: description});
    });

### The Backbone Way

    var TodoItem = Backbone.Model.extend({
      urlRoot: "/todos"
    });

<!-- tsk -->
    var todoItem = new TodoItem({description: "What do you need to do?"}); 
    todoItem.save({description: 'Pickup Kids.'});



Build the Form with Backbone View

    var TodoForm = Backbone.View.extend({
      template: _.template('<form>' +
        '<input name=description value="<%= description %>" />' +
        '<button>Save</button></form>'),
      render: function(){
        this.$el.html(this.template(this.model.attributes));
        return this;
      } 
    });

<!-- tsk -->
    var todoItem = new TodoItem({description: "What do you need to do?"});
    var todoForm = new TodoForm({model: todoItem});
    $('#app').html(todoForm.render().el);

This outputs:
    <form><input name=description value="What do you need to do?" />
    <button>Save</button></form>


### Capture Button Click and Return Key to Save Model


Using `submit` allows processing on either click or pressing enter.

    var TodoForm = Backbone.View.extend({
      template: _.template('<form>' +
        '<input name=description value="<%= description %>" />' +
        '<button>Save</button></form>'),
      events: {
        submit: 'save'  // !!! You can also use 'click' to use just 'click'
      },
      save: function(e) {
        e.preventDefault();
        var newDescription = this.$('input[name=description]').val();
        this.model.save({description: newDescription});
      } 
    });


### Reusing Form to Edit existing TodoItem

Get existing TodoItem from already fetched collection

    var todoItem = todoItems.get(1); 

Pass in existing model

    var editTodoForm = new TodoForm({model: todoItem}); 

Replace '#app' with the HTML of the form

    $('#app').html(editTodoForm.render().el);
￼


### Review of our App’s Router
    ￼
    var TodoApp = new (Backbone.Router.extend({
      routes: {
        "": "index"
      },
      initialize: function(){
        this.todoItems = new TodoItems();
        this.todosView = new TodosView({collection: this.todoItems});
      },
      index: function(){
        this.todoItems.fetch();
        $('#app').html(this.todosView.render().el);
      }
    }));

### Add Route to render Edit Form

    var TodoApp = new (Backbone.Router.extend({
      routes: {
        "": "index",
        "todos/:id/edit": "edit" // !!! Add this line
      },

      initialize: function(){
        this.todoItems = new TodoItems();
        this.todosView = new TodosView({collection: this.todoItems});
      },

      index: function(){
        this.todoItems.fetch();
        $('#app').html(this.todosView.render().el);
      }
      ￼￼
      edit: function(id){ // !!! Add this function
        var todoForm = new TodoForm({model: this.todoItems.get(id) });
        $('#app').html(todoForm.render().el);
      }
    }));


### Add Route to render New Form

    var TodoApp = new (Backbone.Router.extend({
      routes: {
        "": "index", 
        "todos/:id/edit": "edit",
        "todos/new": "newTodo"
      },

      ...

      newTodo: function(){
        var todoItem = new TodoItem({description: "What do you have to do?"});
        var todoForm = new TodoForm({model: todoItem});
        $('#app').append(todoForm.render().el);
      },
    ￼￼
      edit: function(id){
        var todoForm = new TodoForm({model: this.todoItems.get(id) });
        $('#app').html(todoForm.render().el);
      }
    }));


### Get Back to the List after saving **success** or **error**

    var TodoForm = Backbone.View.extend({
      ...
      save: function(e) {
        e.preventDefault();
        var newDescription = this.$('input[name=description]').val(); 
        this.model.save({description: newDescription}, {
          success: function(model, response, options) {
            Backbone.history.navigate('', { trigger: true });
          }, 
          error: function(model, xhr, options){
            var errors = JSON.parse(xhr.responseText).errors;
            alert('Oops, something went wrong with saving the TodoItem: ' + errors);
          }
        });
      }
    }); 




## 6. App Organization

### Class Naming

Before, everything is placed in the Global Scope, and this provides drawbacks:

* Leads to naming collisions
* Need to put “what kind of object” it is in the name e.g. “TodoItemView”
* Maintainability doesn’t scale with large applications

<!-- tsk -->
    var TodoItem = Backbone.Model.extend({});
    var TodoItemView = Backbone.View.extend({}); 
    var TodoItems = Backbone.Collection.extend({}); 
    var TodoItemsView = Backbone.View.extend({});
    var TodoRouter = Backbone.Router.extend({});


Use a Global Object for Namespace.  Create a single global object where everything is stored:

<!-- tsk -->
    var App = {
      Models: {},
      Views: {},
      Collections: {}
    }

<!-- tsk -->
    App.Models.TodoItem = Backbone.Model.extend({}); 
    App.Views.TodoItem = Backbone.View.extend({});
    App.Collections.TodoItems = Backbone.Collection.extend({}); 
    App.Views.TodoItems = Backbone.View.extend({});
    App.TodoRouter = Backbone.Router.extend({});  //Store one-off objects on App



Reference classes with the namespace

    var todoItem = new App.Models.TodoItem({...})



### Handle Links Outside of Backbone Views

<!-- tsk -->
{% raw %}
    <ul>
      <li><a href="/completed">Show Completed</a></li>
      <li><a href="/support">Support</a></li>
    </ul>
{% endraw %}


Solution A: Using Plain jQuery


    $('a').click(function(e){
      e.preventDefault();
      Backbone.history.navigate(e.target.pathname, {trigger: true});
    });

Solution B: Move jQuery inside of the App Object and call `App.start()`.

    var App = {
    Models: {}, Views: {}, Collections: {}, start: function(){
        $('a').click(function(e){
          e.preventDefault();
          Backbone.history.navigate(e.target.pathname, {trigger: true});
        });
        Backbone.history.start({pushState: true});
      }
    }


<!-- tsk -->
    $(function(){ App.start(); });

Solution C: Make the App a Backbone View, so you can use event handler instead
when a link is clicked.

    var App = Backbone.View.extend({ // !!! App now is a View Class
      Models: {},
      Views: {},
      Collections: {},

      events: { // !!! Use View event to handle clicks
        'click a': function(e){
          e.preventDefault();
          Backbone.history.navigate(e.target.pathname, {trigger: true});
        }
      },

      start: function(){
        Backbone.history.start({pushState: true});
      } 
    })


<!-- tsk -->
    var app = new App({el: document.body}); // Using body will capture everything


Clean-up Solution C further by creating 
instance without creating a class

    var App = new (Backbone.View.extend({  // !!! `new` here!
      ...
    }))({el: document.body});

<!-- tsk -->
    $(function(){ App.start(); });
￼


### Skip Links

Handle specific links by adding specificity

    <ul>
      <li><a href="/completed" data-internal="true">Show Completed</a></li>
      <li><a href="/support">Support</a></li>
    </ul>

<!-- tsk -->
Event Handler

    ...
    events: {
      'click a[data-internal]': function(e){
        e.preventDefault();
        Backbone.history.navigate(e.target.pathname, {trigger: true});
      }
    }
    ...

### Build Initial HTML

Since we now have a View that encompasses the entire document body, 
if we want we can put some template code in the App view which has some of the 
initial HTML.  
￼
    var App = new (Backbone.View.extend({
      ...
      template: _.template('<h1>ToDo List!</h1>' + '<div id="app"></div>'),
      render: function(){
        this.$el.html(this.template());
      }
    }))({el: document.body});

<!-- tsk -->
    $(function(){
      App.render();
      App.start();
    })

This results in...

<!-- tsk -->
    <body>
      <h1>ToDo List!</h1>
      <div id="app"></div>
    </body>


### Object Initialization

Inside the `start` method is also a good place to instantiate our collections, our views,
rendering out the views, and fetching the todo items.

    var App = new (Backbone.View.extend({
      ...
      start: function(){
        var todos = new App.Collections.TodoItems();
        var todosView = new App.Views.TodoItems({collection: todos}); 
        this.$el.append(todosView.render().el);
        todos.fetch();
      } 
    }))({el: document.body});


<!-- tsk -->
    $(function(){ App.start(); })

...However, we can save an AJAX call that is used on `fetch` in the initial load by
boostrapping the HTML data which will be called on `append` anyways...

### Bootstrap Model Data

<!-- tsk -->
    var App = new (Backbone.View.extend({ 
      start: function(bootstrap){
        var todos = new App.Collections.TodoItems(bootstrap.todos); 
        var todosView = new App.Views.TodoItems({collection: todos});
        this.$el.append(todosView.render().el);
      }
    }))({el: document.body});


Bootstrap data comes from rendered HTML page

<!-- tsk -->
    var bootstrap = {
      todos: [
        {id: 1, description: "Pickup Milk.", status: "complete"},
        {id: 2, description: "Pickup Kids.", status: "incomplete"},
      ]
    }

Pass in data to start

    $(function(){ App.start(bootstrap); });


## 7. Customizing Backbone

### Using Other Templates


￼Underscore template:

    _.template("<span><%= description %></span" +
      "<em><%= assigned_to %></em>")

{{mustache template}}: 

    Mustache.compile("<span>{{ description }}</span" +
      "<em>{{ assigned_to }}</em>")


NOTE: Mustache doesn't allow arbitrary js, but Underscore does.  Underscore could be strange
and verbose, and Mustache is cleaner.



### Default RESTful Persistence Strategy


var todoItem = new TodoItem({id: 1}) 

C.R.U.D.: 
    ￼￼￼
    // Read￼
    todoItem.fetch(); // ==> GET /todos/1

    // Update
    todoItem.save(): // ==> PUT /todos/1

    // Delete
    todoItem.delete(); // ==> DELETE /todos/1

    // Create
    (new TodoItem({description: "Pickup Kids"})).save(); // ==> POST /todos



### Make Read-only Model

Override the sync function.  `method` below is either 'create', 'read', 'update',
or 'delete'.

    var TodoItem = Backbone.Model.extend({
      sync: function(method, model, options){
        if (method === "read"){
          Backbone.sync(method, model, options);
        }else{
          console.error("You can not " + method + " the TodoItem model");
        }
      }
    });

<!-- tsk -->
    todoItem.fetch(); // This works

<!-- tsk -->
    todoItem.save();  // ==> "You can not update the TodoItem model"


### Completely Replace Persistence Strategy

￼
var TodoItem = Backbone.Model.extend({
  sync: function(method, model, options){
    options || (options = {});

    switch(method){
      case 'create':
        var key = "TodoItem-" + model.id;
        localStorage.setItem(key, JSON.stringify(model));
      break;

      case 'read':
        var key = "TodoItem-" + model.id;
        var result = localStorage.getItem(key);
        if (result){
          result = JSON.parse(result);
          options.success && options.success(result);
        }else if (options.error){
          options.error("Couldn't find TodoItem id=" + model.id);
        } 
      break;


      case 'update':
      break;

      case 'delete':
      break;
} }
});


































---



