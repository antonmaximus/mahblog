---
layout: post
title:  "Anatomy of Backbone A2"
date:   2014-11-05 14:00:01
categories: [Web Development]
---


## 5. Collections

Set of Models

    var TodoList = Backbone.Collection.extend({
      model: TodoItem
    });
    var todoList = new TodoList();


Add/remove/get

    todoList.length; //# of models
    todoList.add(todoItem1); // add model instance
    todoList.at(0); //get model instance at index 0
    todoList.get(1); // get by id
    todoList.remove(todoItem1); //remove model instance

Bulk Population

    var todos = [
      {description: 'Pick up milk.', status: 'incomplete'},
      {description: 'Get a car wash', status: 'incomplete'},
      {description: 'Learn Backbone', status: 'incomplete'}
    ];
    todoList.reset(todos);

### Fetching Data from the Server

URL to get JSON data from

    var TodoList = Backbone.Collection.extend({
      url: '/todos'
    });


Populate collection from server

    todoList.fetch();  // => GET /todos


### Collections can have Events

To listen for an event on a collection, and run an event: (works just like models)

    todoListon('event-name', function(){
      alert('event-name happened!');
    });
    todoList.trigger('event-name');

### Special Events

    //Listen for events
    todoList.on('reset', doThing);  

    // Event Triggered on reset and fetch
    todoList.fetch();
    todoList.reset();

    // Event triggered without notification
    todoList.fetch({silent: true});
    todoList.reset({silent: true});

    // Remove event listener
    todoList.off('reset', doThing);

Syntax

    todoList.on(<event>, <function>);

Built-in Events. 
<table>
  <tr><td>add</td><td>When a model is added</td></tr>
  <tr><td>removed</td><td>When a model is removed</td></tr>
  <tr><td>reset</td><td>When reset or fetched</td></tr>
</table>

Models in collection.  Events triggered on a model in a collection will also be triggered on the collection.
<table>
  <tr><td>change</td><td>When an attribute is modified</td></tr>
  <tr><td>change:&lt;attr&gt;</td><td>When &lt;attr&gt; is modified</td></tr>
  <tr><td>destroy</td><td>When a model is destroyed</td></tr>
  <tr><td>sync</td><td>Whenever successfully synced</td></tr>
  <tr><td>error</td><td>When model save or validation fails</td></tr>
  <tr><td>all</td><td>Any triggered event</td></tr>
</table>


### Iteration

Example collection

    todoList.reset([
      {description: 'Pick up milk.', status: 'incomplete', id: 1},
      {description: 'Get a car wash.', status: 'complete', id: 2}
    ]);

Alert each model's description

    todoList.forEach(function(todoItem){
      alert(todoItem.get('description'));
    });

Build an array of description

    todoList.map(function(todoItem){
      return todoItem.get('description');
    });

Filter models of some criteria

    todoList.filter(function(todoItem){
      return todoItem.get('status') === "incomplete";
    });

Other Iteration Functions
<table>
  <tr><td>forEach</td><td>reduce</td><td>reduceRight</td></tr>
  <tr><td>find</td><td>filter</td><td>reject</td></tr>
  <tr><td>every</td><td>all</td><td>some</td></tr>
  <tr><td>include</td><td>invoke</td><td>max</td></tr>
  <tr><td>min</td><td>sortBy</td><td>groupBy</td></tr>
  <tr><td>sortedIndex</td><td>shuffle</td><td>toArray</td></tr>
  <tr><td>size</td><td>first</td><td>initial</td></tr>
  <tr><td>rest</td><td>last</td><td>without</td></tr>
  <tr><td>indexOf</td><td>lastIndexOf</td><td>isEmpty</td></tr>
  <tr><td>chain</td></tr>
</table>

[Link to Functions](http://documentcloud.github.io/backbone/#Collection-Underscore-Methods)


## 6. Collections & Views

Collection + View == Collection View!

Review our Model View

    var TodoView = Backbone.View.extend({
      render: function(){
        this.$el.html(this.template(this.model.toJSON()));
        return this;
      }
    ... });
    var todoItem = new TodoItem();
    var todoView = new TodoView({model: todoItem});
    console.log(todoView.render().el);

### Define and Render

    var TodoListView = Backbone.View.extend({});
    var todoListView = new TodoListView({collection: todoList});

First Crack at Render

    render: function(){
      this.collection.forEach(function(todoItem){
        var todoView = new TodoView({model: todoItem});
        this.$el.append(todoView.render().el);  // !!! `this` changes context in forEach
    }); }

Use addOne (2nd Crack at Render)

    render: function(){ this.collection.forEach(this.addOne, this);  // !!! 2nd param saves context
    }
    addOne: function(todoItem){
      var todoView = new TodoView({model: todoItem});
      this.$el.append(todoView.render().el);
    }



To finish off...

    var todoListView = new TodoListView({collection: todoList});
    todoListView.render();
    console.log(todoListView.el);


### Adding new Models

Since this is a collection, you must listen to the `add` event in the initalize function


    var TodoListView = Backbone.View.extend({
      initialize: function(){
        this.collection.on('add', this.addOne, this);
    },
      addOne: function(todoItem){
        var todoView = new TodoView({model: todoItem});
        this.$el.append(todoView.render().el);
      },
      render: function(){
        this.collection.forEach(this.addOne, this);
      }
    });

<!-- tsk -->
    var newTodoItem = new TodoItem({
      description: 'Take out trash.',
      status: 'incomplete'
    });
    todoList.add(newTodoItem);

### Reset Event

    var TodoListView = Backbone.View.extend({
      initialize: function(){
        this.collection.on('add', this.addOne, this);
        this.collection.on('reset', this.addAll, this);
      },
      addOne: function(todoItem){
        var todoView = new TodoView({model: todoItem});
        this.$el.append(todoView.render().el);
      },
      addAll: function(){
        this.collection.forEach(this.addOne, this);
    ￼  },
      render: function(){
        this.addAll();
      }
    });

<!-- tsk -->
    var todoList = new TodoList();
    var todoListView = new TodoListView({
      collection: todoList
    });
    todoList.fetch();


### Fixing remove with custom events

    todoList.remove(todoItem);


TodoList Collection

    initialize: function(){
      this.on('remove', this.hideModel);
    },
    hideModel: function(model){
      model.trigger('hide');
    }


TodoItem View

    initialize: function(){
      this.model.on('hide', this.remove, this);
    }



## 7. Router & History


### The Router

Routers map URLs to actions

    // Index Action
    var router = new Backbone.Router({
      routes: { "todos": 'index' },
      index: function(){
        ...
    } });

    // OR Show Action
    var router = new Backbone.Router({
      routes: { "todos/:id": 'show' }
      show: function(id){ ... }
    })


### Triggering Routes

    router.navigate("todos/1", {
      trigger: true
    });

Backbone History

    Backbone.history.start({pushState: true});  // !!! pushState on!

<!-- tsk -->
    router.navigate("todos/1")


### Show Action

Define Router Class

    var TodoRouter = Backbone.Router.extend({
      routes: { "todos/:id": "show" },
      show: function(id){
        this.todoList.focusOnTodoItem(id);
      },
      initialize: function(options){
        this.todoList = options.todoList;
      } 
    });

Instantiate router instance

    var todoList = new TodoList();
    var TodoApp = new TodoRouter({todoList: todoList});

### Index Action

    var TodoRouter = Backbone.Router.extend({ routes: { "": "index",
                "todos/:id": "show" },
      index: function(){
        this.todoList.fetch();
    },
      show: function(id){
        this.todoList.focusOnTodoItem(id);
    },
      initialize: function(options){
        this.todoList = options.todoList;
    } });


### App Organization

Since there's only one router

    var TodoApp = new (Backbone.Router.extend({
      routes: { "": "index", "todos/:id": "show" },
      initialize: function(){
        this.todoList = new TodoList();
        this.todosView = new TodoListView({collection: this.todoList});
        $('#app').append(this.todosView.el);
      },
      start: function(){
        Backbone.history.start({pushState: true});
      },
      index: function(){
        this.todoList.fetch();
      },
      show: function(id){
        this.todoList.focusOnTodoItem(id);
      }
    }));

<!-- tsk -->
    $(function(){ TodoApp.start() });




---
