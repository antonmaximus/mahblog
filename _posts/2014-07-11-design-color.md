---
layout: post
title:  "Design: Color"
date:   2014-07-11 14:43:58
categories: [Web Development]
---

## Color Theory 101:
There are 2 types of color: color you can touch (subtractive color)
and color you can't touch (additive color).

* Subtractive Color -- CMYK
* Additive Color -- RGB


## HSL
* Humans process color in Hue, Saturation, Lightness.  
* Most applications refer to Lightness as Brightness where the maximum value is 
the most vibrant color.  (Lightness's maximum value is white.)

---

## Color Scheme
It's a collection of 4-5 colors built into the style guide.


<img src="/assets/webdev_color1.jpg"  width=""/>

Steps in Building a Color Scheme:

1. Start by selecting a base color -- different colors create different moods.
  * red -- heat, passion, excitement. Easily grabs a)ention and evokes speed and energy.
  * orange -- warmth, vitality.  Associated with reliability & playfulness.
  * yellow -- optimism, creativity.  Represents sunshine, cheer, and happiness.
  * green -- serenity, health.  Connotes growth, nature, freshness.
  * blue -- security, truth, stability. Implies loyalty, reliability and an open communication
  * purple -- spirituality, intelligence, wealth.  Also means royal, sentimental, and sophisticated.
  * pink -- youthful intensity.  Conveys energy, fun and excitement.
  * brown -- durability, class. Represents age, stabili$ and relaxation.
  * black -- power, drama. It’s serious, bold and strong.
  * white -- simplicity, cleanliness.  It’s message is youthful, mild and pure.
2. Keep culture in mind when choosing a base color.
3. Build Scheme around base color.


<img src="/assets/webdev_color_monochromatic.jpg"  width=""/>
<img src="/assets/webdev_color_analogous.jpg"  width=""/>
<img src="/assets/webdev_color_complementary.jpg"  width=""/>



---

## Color & type

Contrast is the difference in colors.

* Contrast of Hue
* Contrast of Saturation
* Contrast of Lightness

### Most contrasts are multi-faceted.  

It's common to combine contrasts of Hue,
Saturation, or Brightness. The left part of the image below shows a contrast of 
Hue only; the right part shows a contrast of Hue & Brightness. WCAG determines the the standard for contrasts.  It stands for Web Content 
Accessibility Guidelines.


<img src="/assets/webdev_color_contrast.jpg"  width=""/>



* High-contrast colors are the best (e.g., black and white)
* Same Hue, but high saturation & brightness contrast are second. 


Some contrasts are intrinsically difficult for humans to distinguish (and 
are plain ugly too).

<img src="/assets/webdev_color_ugly.jpg"  width=""/>



### Natural Values
Different colors have different natural values.  For example, yellow is a 
naturally light color and blue is naturally dark.  Creating contrast where
yellow is darker than the blue creates an awkward user-experience.  However,
this can be fixed by adjusting the lightness to make yellow be lighter and blue 
darker -- so as to correspond with the natural values.

<img src="/assets/webdev_color_natural.jpg"  width=""/>


### Warm & Cold Colors
Red, orange, & yellow side are warm.  Green, blue, and purple are on the cooler side.
Use warm & cold to create depth.  Warm is always closer, and cold is in the back.

<img src="/assets/webdev_color_warmcool1.jpg"  width=""/>



<img src="/assets/webdev_color_warmcool2.jpg"  width=""/>



### Visual Hierarchy
Combine colors to reinforce a hierarchy.  Use color differences to contribute 
to relationships between elements on the page.

<img src="/assets/webdev_color_hierarchy.jpg"  width=""/>


### Types and Images

* Don't be tacky -- Don't add effects (dropshadows, strokes, etc.) on text.
  * Instead, treat the image (overlay, darken, etc.) on the background to make the text stand out.
* Type in an image -- Dont do it! Professional copy doesn't belong in an image.
  * (Unless it's cat meme, or a blog post for personal use ;-) )


Below is a good example of color combination that was used on Code School's site.
A combination of analogous, complementary colors with high enough contrast for 
text colors and backgrounds.

<img src="/assets/webdev_color_combination.jpg"  width=""/>
