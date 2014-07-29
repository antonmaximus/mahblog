---
layout: post
title:  "Front-end Formations"
date:   2014-07-05 14:42:59
categories: skills
---

### New HTML5 elements...


`<section>` -- Acts like a 'div'.  However, a 'div' has no semantic meaning, 
but the 'section' element does.  It's used for grouping together thematically 
related content and for showing document outline.  As Jeremey Keith 
mentioned, *"ask yourself: is all of the content related?"*


`<header>` -- A group of introductory content or navigational aids or both.  
It should describe the content it is contained within.

`<footer>` -- Similar to 'header'.

`<aside>` -- Tangentially related to the content surrounding it (i.e., it is 
indirectly related to the content).  However, it's been utilized as a 
sidebar for sites too.

`<nav>` -- Intended for major navigation.

`<article>` -- It represents a self-contained composition that is independently
distributable.  It's similar to a 'section', but the question to ask is: *would 
you syndicate this in an RSS feed?*

`<main>` -- It consists of content that is directly to or expands upon the 
central topic of the document.  Two pracitices: 1. Do not include more than one 
'main' in a document, and 2. Do not make it a child of 'article', 'aside', 
'footer', 'header', or 'nav'.

`<figure>` &amp; `<figcaption>` -- A 'figure' element is self-contained and that
can be moved away from the main flow of the document without affecting the 
document's meaning.  A 'figcaption' is used as supplemental information for the 
'figure'.

`<time>` -- It's used for time representation. duh.



### New HTML5 input types...

Using the correct type provides users added usability on mobile browsers.

* Search -- This also adds an extra UI benefit of a clear icon.

* Email

* URL

* Tel

* Number

* Range -- provides a slider input.

* Date -- provides a date picker.

* Month

* Week

* Time

* Datetime

* Datetime-local -- just like Datetime but with no time zone associated with it.

* Color -- provides color picker.


### Font Face
Ability to provide online fonts for use on websites.


{% highlight ruby %}
@font-face {
  font-family: 'OpenSansRegular';
  src: url('OpenSansRegular-webfont.eot');
  font-style: normal;
  font-weight: normal;
}

h1 {
  font-family: 'OpenSansRegular', Arial, sans-serif;
}
{% endhighlight %}
