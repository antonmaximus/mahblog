---
layout: post
title:  "CSS Cross Country"
date:   2014-07-07 14:42:59
categories: skills
---

### Primary DOM Selectors

* Element Selector
* Class Selector
* ID Selector


### Cascade Order Priority


* Using `!important` -- (Highest Priority)
* Inline style attribute
* `<head>` definition
* External `<link>` -- (Lowest Priority)

### Combinator Selectors
Combinator -- shows relationship

* Descendant Selector  -- elements that are descendants 
  * `div p { background-color: yellow; }`
* Child Selector -- elements that are immediate children
  * `div > p { background-color: yellow; }`
* Adjacent Sibling Selector -- 'p' that are placed immediately after 'div' in 
example below: 
  * `div + p { background-color: yellow; }`
* General Sibling Selector -- all 'p' that are siblings of 'div' in example below:
  * `div ~ p { background-color: yellow; }`


### Clearing Floats: The Clearfix
A clean technique to clear floats.  When used on the parent, the children will 
be self-cleared.
{% highlight ruby %}
.group:before, .group:after { content: ""; display: table; }
.group:after { clear: both; }
.group { zoom: 1; /* IE6&7 */ }
{% endhighlight %}

### Specificity
Priority of a Selector follows this order: **A, B, C, D, E**

* A -- any inline style? (Highest priority)
* B -- # of ID Selectors
* C -- # of Class Selectors
* D -- # of Element Selectors 
* E -- any inherited style? (Lowest priority)



### Collapsing Margins
Happens when the margins of two block elements collapse to the larger one
instead of adding the two. [Source.][w3c]

![](http://i.imgur.com/KXipDH8.png)

Collapsing margins will not occur when one or more block element has:

* padding or border
* relative or absolute positioning
* a float left or right


[w3c]: http://www.w3.org/TR/CSS2/box.html#collapsing-margins


### Image Use

* Always ask: Content or Layout?
* Resize images server-side when possible
* Use overflow hidden when necessary

{% highlight ruby %}
<h4>Rental Products</h4>
<ul class="rental">
  <li class="crop">
  <img src="snowboard.jpg" alt="Snowboard" />
  </li>
</ul>
{% endhighlight %}

{% highlight ruby %}
.crop {
  height: 300px;
  width: 400px;
  overflow: hidden;
}
.crop img {
  height: auto;
  width: 400px;
}
{% endhighlight %}



### Logo/Image Replacement

Add descriptive text to image-replaced elements for best-practice especially
on screen-readers.  Then, use `text-indent: -9999px` in CSS to hide the 
placeholder text.


### Sprites

Use sprites when swapping backgroud images to prevent issues such as:

* The need to preload an image
* Adding an extra HTTP request



### Pseudo Classes
Allow you to conditionally select an element based on state or position.  Below 
are the most useful selectors:

* :hover / :focus / :active / :visited
* :first-child / :last-child / :only-child
* :nth-child(odd/even/an+b) / :nth-of-type()  


### Pseudo Element
Allows you to modify an element's content for styling.  Below are the most 
utilized pseudo elements:

* :before / :after -- needs `content` property to be used.
* :first-letter / :first-line  -- doesn't need `content` property to be used.
￼