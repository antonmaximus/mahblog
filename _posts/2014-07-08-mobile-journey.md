---
layout: post
title:  "Mobile Journey"
date:   2014-07-08 14:42:59
categories: skills
---


### Fluid Layouts
Fluid layouts are layouts that scale.  What makes a fluid site?

* Fluid Grid
* Using relative values (i.e., percentages for widths)


### Adaptive Design
Design for controlled adaptation where context and/or device is known and
you design accordingly.

Four Design Principles

* Who is your user?
* How will they use the site?
* Context -- i.e. what device?
* Content -- how will it adapt based on the context?


### Mobile First Approach

*"...prepares you for the explosive growth and new opportunities on the mobile 
internet, forces you to focus, and enables you to innovate in ways you 
previously couldn't"*

~*Luke Wroblewski*


Mobile First Approach forces you to focus on the most important things (because
of how much real estate one has):

* Simplifying content
* Prioritizing layout
* Optimizing user experience


### Responsive Design
In responsive design, the content defines breakpoints.  We provide continuity 
between a variety of different context (e.g., screen size).



### Media Queries 
Best practice is to place them at the bottom of stylesheet


{% highlight ruby %}
/*-----------------
Media Queries
------------------*/
@media screen and (max-width: 320px) {
  body {
    font-size: 100%
  }
}
{% endhighlight%}


### Responsive Media
`img, embed, object, video { max-width: 100%; }`


### Retina Images
Retina images have 1.5x - 2x the pixel density. (Most iPhones have pixel density
of 2, while some Android devices have 1.5).  Use media queries to target
devices that have retina displays and optimize images.


{% highlight ruby %}
@media
only screen and (-webkit-min-device-pixel-ratio: 1.5),
only screen and (min-device-pixel-ratio: 1.5) {
  .logo {
    background-image: url(logo@2x.png);
    -webkit-background-size: 12px 16px;
    background-size: 12px 16px;
  } 
}
{% endhighlight %}

* iOS's naming convention for retina-images is `logo@2x.png`
* be explicit with the `size` because browsers will serve images in their 
original size


### Relative Font-sizing
Formula is `target/context = result`

