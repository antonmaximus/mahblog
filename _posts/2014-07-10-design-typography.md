---
layout: post
title:  "Design: Typography"
date:   2014-07-10 14:43:59
categories: skills
---

## Style Guide
Reference document for multiple instances of typography/styles that would 
be used throughout your application.  An example is shown below.  Putting this
guide beforehand will avoid any inconsistency and create a better overall 
design/user-experience.

![](http://i.imgur.com/ZLXQtur.png)


---

## Typography is both Visual and Verbal
Different classifications of type create different moods that will either
server your content well or not. The reader surveys the overall graphical 
patterns of the page, and then parse the language and read.  

History... A basic system for classifying typefaces was devised in the nineteenth century, when printers sought to identify a heritage for their own craft analogous to that of art history. *Humanist* letterforms are closely connected to calligraphy and the movement of the hand. *Transitional* and *modern* typefaces are more abstract and less organic. These three main groups correspond roughly to the Renaissance, Baroque, and Enlightenment periods in art and literature. Designers in the twentieth and twenty-first centuries have continued to create new typefaces based on historic characteristics. [Source](http://papress.com/thinkingwithtype/letter/classification.htm).

Below are the most common classification of types...


### Serif Font
Serifs are the small lines tailing from the edges of letters. Some styles of 
type in this Serif Classification are:

* Humanist -- great for historical or journalism applications
* Transitional -- traditional academia and legal
* Modern -- arts and culture
* Egyptian -- marketing and promotional

![](http://i.imgur.com/Zs5zo2S.jpg)
![](http://i.imgur.com/y2oLzyq.jpg)
![](http://i.imgur.com/vF8inNJ.jpg)
![](http://i.imgur.com/xYv4Sbh.jpg)

### Sans Serif Font

* Humanist Sans -- government or education
* Transitional Sans -- technology or transportation
* Geometric Sans -- science or architecture

![](http://i.imgur.com/ZbM9mpq.jpg)
![](http://i.imgur.com/LhxJsqI.jpg)
![](http://i.imgur.com/MzJ38Os.jpg)


### Script Font
Good for adding human element to the site, but not so great for body copy.
Comic sans is technically a script font.



## Mix & Mingle

* Don't choose two fonts from the same style (e.g., humanist serif & humanist serif)
* Don't choose two fonts from the same style (e.g., humanist serif & slan serif)
* When mixing classes find a similar trait (e.g., humanist serif & humanist sans)
  * this trait can even be largely abstract
* Strive for **Contrast over Harmony**
  * A good rule of thumb to follow is "Keep it the same or 
  change it a lot. Look for emphatic differences rather than mushy transitions."
* Single Typeface is Okay -- never feel obligated to use two.



---

## Font Size

* Humans create relationships.  It's how our brain processes. 
  * bigger items that stand out appear to be more important.
  * the headline in big/bold fonts indicate the main topic of an article.
  * a pic of red bell pepper on a site indicates food theme
  * a sea of text becomes on a site becomes troublesome for the audience 
  because it's hard to **assess** what the site is about right away.
* So create a visual hierarchy!



### When creating a style guide...

1. Set the Body Copy first -- this is usually around 16px (convert to ems or rems when necessary).
2. Headline Text -- around 200% to 300% of the body copy.
3. B-head (or sub-head) Text -- around 150% of the body copy.
4. Navigational Text -- same size as body copy.
5. Byline Text -- around 75% of the body copy.

### Leading
Leading is the amount of space between lines of text.

* Leading that is too large negatively affects readability and produces discontinuity
* Leading that is too tight creates a claustrophobic feel
* Good leading is around 120% to 150% of the body copy size.
  * setting `line-height: 1.5` provides 150% the font's size.

### Bold & Italics

* Use a "less is more" approach.
* Use natural convention -- e.g., eyes naturally expect headlines to be in bold rather than body copy


---


## Line Width

* Lines that are too short are difficult to read as the user's eyes have to jump 
quickly from line to line.
* Lines that are too long can get the user's eyes lost easily.


### Characters per Line
* Line width is measured in CPL (characters per line)
  * 50 - 70 CPL is ideal (including spaces)
* In CSS, CPL can be adjusted using `max-width`


### Save the Orphans and Widows
They impede the natural flow of reading for the user.

* A widow is a single word on a line of text
* An orphan is a single line of text on a column/page