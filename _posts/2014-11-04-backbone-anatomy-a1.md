---
layout: post
title:  "Anatomy of Backbone A1"
date:   2014-11-04 14:00:00
categories: [Web Development]
---


## 2. Models


Generating a model class and a model instance:

    var TodoItem = Backbone.Model.extend({});￼
    var todoItem = new TodoItem(
      { description: 'Pick up milk', status: 'incomplete', id: 1 }
    );


To get, set, and save an attribute

    todoItem.get('description'); 
    todoItem.set({status: 'complete'}); 
    todoItem.save();


### Fetching Data from The Server

URL to get JSON data for model. RESTful web service flavor.  

    var TodoItem = Backbone.Model.extend({urlRoot: '/todos' });


To populate model from server.  Fetch todo with id=1.

    var todoItem = new TodoItem({id: 1})
    todoItem.fetch();

Destroying a todo:

    todoItem.get('id');
    todoItem.destroy();


Creating Default Values

    var TodoItem = Backbone.Model.extend({
      defaults: {
        description: 'Empty todo...',
        status: 'incomplete'
      }
    });

### Models can have Events

To listen for an event on a model, and run the event:

    todoItem.on('event-name', function() {
      alert('event-name happened!');
    });
    todoItem.trigger('event-name');

### Special Events

Syntax

    todoItem.on(<event>, <method>);

To listen for changes,  to see how it's triggered on change, to set without triggering, and to remove listener:

    todoItem.on('change', doThing);
    todoItem.set({description: 'Fill prescription.'});
    todoItem.set({description: 'Fill prescription.'}, {silent: true});
    todoItem.off('change', doThing);

Built-in Events

<table>
  <tr><td>change</td><td>When an attribute is modified</td></tr>
  <tr><td>change:&lt;attr&gt;</td><td>When &lt;attr&gt; is modified</td></tr>
  <tr><td>destroy</td><td>When a model is destroyed</td></tr>
  <tr><td>sync</td><td>Whenever successfully synced</td></tr>
  <tr><td>error</td><td>When model save or validation fails</td></tr>
  <tr><td>all</td><td>Any triggered event</td></tr>
</table>







## 3. Views

Create a view class and a view instance

    var TodoView = Backbone.View.extend({});￼
￼   var todoView = new TodoView({ model: todoItem });


Rendering the view.  Every view has a top level `el` (element) where `<div>` is the default tag.

    var TodoView = Backbone.View.extend({
      render: function(){
        var html = '<h3>' + this.model.get('description') + '</h3>';
        todoView.$el.html(html);  // todoView.$el.html(html) works too, but slower.
      }
    });


To use a different tag:

    var SimpleView = Backbone.View.extend({tagName: 'li'});
    var simpleView = new SimpleView();


### Using a Template

Underscore Library is part of Backbone

    var TodoView = Backbone.View.extend({
      tagName: 'article',
      id: 'todo-view',
      className: 'todo',
      template: _.template('<h3><%= description %></h3>'),
    ￼} });


    var todoView = new TodoView({ model: todoItem });
    todoView.render();
    console.log(todoView.el);


Other Templating Engines

* Underscore.js
* Mustache.js
* Haml-js
* Edo


### Adding View Events

Syntax
{% raw %}
    "<event> <selector>": "<method>"
{% endraw %}

Example:
{% raw %}
    var TodoView = Backbone.View.extend({
      events: {
        "click h3": "alertStatus"
      },
      alertStatus: function(e){
        alert('Hey you clicked the h3!');
    } });
{% endraw %}

Selector is scoped to the `el`
{% raw %}
    this.$el.delegate('h3', 'click', alertStatus);  //delegate is deprecated, but same idea
{% endraw %}
Views can have many events on the `el`
{% raw %}
    var DocumentView = Backbone.View.extend({
      events: {
        "dblclick"                : "open",
        "click .icon.doc"         : "select",
        "click .show_notes"       : "toggleNotes",
        "click .title .lock"      : "editAccessLevel",
        "mouseover .title .date"  : "showTooltip"
      }, 
      ...

    });
{% endraw %}

### View Event Options
    var SampleView = Backbone.View.extend({
      events: {
        "<event> <selector>": "<method>"
      },
    ... });


Events: 

change click dblclick focus focusin
focusout hover keydown keypress load
mousedown mouseenter mouseleave mousemove mouseout
mouseover mouseup ready resize scroll
select unload


## 4. Models and Views

Review of model & view

    var TodoView = Backbone.View.extend({
      template: _.template('<h3><%= description %></h3>'),
      render: function(){
        this.$el.html(this.template(this.model.toJSON()));
    } });


    var todoView = new TodoView({ model: todoItem });
    todoView.render();
    console.log(todoView.el);


Adding a Checkbox (that can be updated on the UI)

    var TodoView = Backbone.View.extend({ 
      template: _.template('<h3>' +
        '<input type=checkbox ' +
        '<% if(status === "complete") print("checked") %>/>' +
        '<%= description %> </h3>'),

      render: function() {
        this.$el.html(this.template(this.model.toJSON()));
      }
    } });


View events update the Model (Update model on UI event, and Refactor to separate what's for the Model)

    var TodoView = Backbone.View.extend({
      events: {
        'change input': 'toggleStatus'
      },
      toggleStatus: function(){
       this.model.toggleStatus();
      }
    });

    var TodoItem = Backbone.Model.extend({
      toggleStatus: function(){
        if(this.get('status') === 'incomplete'){
          this.set({'status': 'complete'});
        }else{
          this.set({'status': 'incomplete'});
    ￼   } 
        this.save();  // Synch changes to the server
      }
    });


Re-render the view (Model updates change the view)

    var TodoView = Backbone.View.extend({
      events: {
        'change input': 'toggleStatus'
      },
      initialize: function(){
        this.model.on('change', this.render, this);
      },
      toggleStatus: function(){
       this.model.toggleStatus();
      },
      render: function(){
        this.$el.html(this.template(this.model.toJSON()));
    } });

Why the 3rd argument (i.e., What is ***this***)?

<img src="/assets/backboneA1_a.png"  width=""/>

<img src="/assets/backboneA1_b.png"  width=""/>




Remove View on model destroy

    var TodoView = Backbone.View.extend({
      initialize: function(){
        this.model.on('change', this.render, this);
        this.model.on('destroy', this.remove, this);
    },
      render: function(){
       this.$el.html(this.template(this.model.toJSON()));
    },
      remove: function(){
        this.$el.remove();
    } });










---



