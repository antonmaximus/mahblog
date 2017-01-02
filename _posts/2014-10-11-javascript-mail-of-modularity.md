---
layout: post
title:  "JavaScript: Mail of Modularity"
date:   2014-10-11 14:44:58
categories: [Web Development]
---


## Namespacing Basics

* The key to creating a namespace is a single global Object, commonly called the “wrapper” for the space.
* Now that the function and all the necessary variables are “encapsulated” within the namespace, we’ll need to call that namespace in order to access any of them.


{% raw %}
      var ARMORY = (function( war ){
        var weaponList = [ *list of weapon Objects* ]; 
        var armorList = [ *list of armor Objects* ]; 
        var removeWeapon = function(...){};
        var replaceWeapon = function(...){};
        var removeArmor = function(...){};
        var replaceArmor = function(...){};

          return {
            makeWeaponRequest: function(...){
              if(war) //let civilians have weaponry
             },
            makeArmorRequest: function(...){}
          };
      })(wartime);
{% endraw %}




---



