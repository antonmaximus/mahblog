---
layout: post
title:  "JavaScript Objects"
date:   2016-04-12 00:01:00 -0800
categories: craft
---


## Classes

    class Rabbit {
      // Constructor. Runs every time a new instance is created with `new` operator
      constructor(type) {
        this.type = type;
      }

      // Method
      render() {
        console.log(this.type);
      }
    }
    let whiteRabbit = new Rabbit('white');

    console.log("=========")
    console.log(Object.getPrototypeOf(whiteRabbit));
    console.log(Object.getPrototypeOf(Rabbit));

    whiteRabbit.render();
    whiteRabbit.type = 'light';
    whiteRabbit.render();

    let lightRabbit = Object.create(whiteRabbit);
    lightRabbit.render();


    console.log("=====================")
    console.log(Object.prototype.toString.call([1, 2]));
    console.log(Array.prototype.toString.call([1, 2]));


## Enumerable vs Nonenumerable properties


All properties that we create by simply assigning to them are enumerable. The standard properties in `Object.prototype` are all nonenumerable, which is why they do not show up in such a for/in loop.

    var map = {};
    function storePhi(event, phi) {
      map[event] = phi;
    }

    storePhi("pizza", 0.069);
    storePhi("touched tree", -0.081);

    Object.prototype.nonsense = "hi";
    for (var name in map)
      console.log(name);
    // → pizza
    // → touched tree
    // → nonsense
    console.log("nonsense" in map);
    // → true
    console.log("toString" in map);
    // → true

    // Delete the problematic property again
    delete Object.prototype.nonsense;


It is possible to define our own nonenumerable properties by using the `Object.defineProperty` function, which allows us to control the type of property we are creating.


    Object.defineProperty(Object.prototype, "hiddenNonsense",
                          {enumerable: false, value: "hi"});
    for (var name in map)
      console.log(name);
    // → pizza
    // → touched tree
    console.log(map.hiddenNonsense);
    // → hi

But we still have the problem with the regular in operator claiming that the `Object.prototype` properties exist in our object. For that, we can use the object’s `hasOwnProperty` method.

    for (var name in map) {
      if (map.hasOwnProperty(name)) {
        console.log(name);
      }
    }

**Prototype-less objects**.  You are allowed to pass null as the prototype to create a fresh object with no prototype.

    var map = Object.create(null);
    map["pizza"] = 0.069;
    console.log("toString" in map);
    // → false
    console.log("pizza" in map);
    // → true



## Polymorphism
------









