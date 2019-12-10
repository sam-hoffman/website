---
layout: post
date: 2019-12-10
title: Looking into responsive design
---

 I'm on a plane today, so it's the perfect time to remember that a) I actually need to Google things every two minutes while writing any kind of code, b) responsive design is functionally just a buzzword to me at the moment, and c) United Airlines sucks and I refuse to pay them for internet. 

If, in the spirit of stop-describing-your-childhood-just-give-me-the-goddamn-recipe<!--link to the author of lot being mad about that here-->, you just want to know how to do the thing, <a href="#TLDR">jump to the TLDR</a>.  

This website has about four pages and I can already feel the tech debt building, so I had planned to do clean-up and consolidation work on the plane. 
On the other hand, on a plane without internet is the perfect place to try to implement something I don't understand.

{% include image.html
img="assets/pictures/20191207/layouts.png"
title="all my layouts"
caption="Somehow default inherits from ur_default but other things do not - this is definitely on the must-fix list" %}

Giving things my best shot without internet, I switched all my widths to percentages rather than pixels. 
This resulted a sexy-looking mobile layout but a dumb-looking desktop layout with the text stretching way too far across the screen. 
Mobile first design!
{% include image.html
img="assets/pictures/20191207/mobile_layout.png"
title="Mobile layout"
caption="Mobile layout good." %}
  
{% include image.html
img="assets/pictures/20191207/desktop_layout.png"
title="Desktop layout"
caption="Desktop layout bad." %}

Clearly I need to adjust the size of my content for mobile and desktop screens - in other words, I need to make my design responsive. 
The typical way to do this is with media queries - CSS that overrides other CSS based on the size of the screen.  
According to the internet (from my layover in Denver), since I'm using SCSS, a CSS compiler, I might also need mixins and other things I've never heard of. 
Reader, I'm sorry for the fake news earlier: I'm simply not good enough to create responsive design without people on the internet telling me how to do it, and in fact I'm milking the plane as a narrative device to show you the before and after. 
Don't worry, I didn't pay United their blood money. 
I just wrote the rest of this once I got there.  

Internet restored, I found an article called [Write Better Media Queries with Sass](https://davidwalsh.name/write-media-queries-sass). 
Mixins, it turns out, allow you to define anything in your CSS other than top-level statements like imports and function definitions (very cool that there are functions in SCSS). 
Most people seem to use desktop, tablet, and phone styles, but I thought my website looked fine on tablet with the desktop defaults. 
I created two mixins: one that will allow the CSS below it to take precedence if the screen is mobile size, and one that does the same for desktop.  
Here's the SCSS code: 

```scss
@mixin desktop {
  @media screen and (min-width: 601px) {
    @content;
  }
}

@mixin mobile {
  @media screen and (max-width: 600px) {
    @content;
  }
}

.about-text {
 @include mobile {
 width: 80%
 }
 @include desktop {
 width: 50%;
 }
 margin: 0 auto;
 font-family: $sans-serif;
 p {
   display: inline-block;
 }
 a {
   font-family: $sans-serif;
   text-decoration: none;
   color: grey;
   &:hover {
      text-decoration: underline;
    }
  }
  ul {
    margin: 3px;
    li {
    margin: 5px;
    }
  }
}
```
Setting `margin: 0 auto;` is also important - without it, the mobile design won't work right. 
I wish I could say why - I'm sure I'll find out eventually. 
This responsive design is not perfect - it works most of the time, but I seem to have intermittent issues that I'm trying to fix. 
That makes me nervous to publish a blog post about responsive design, so please consider this a discussion of the things I've learned so far about responsive design. 
In order to make the media queries work, you also need to put the following HTML somewhere on the page:
{% highlight html %}
<meta content="width=device-width, initial-scale=1" name="viewport"/>
{% endhighlight %}
I put mine in my default layout, and so was finally forced to fix my inheritance structure and free myself from the tech debt I had put myself in.  

When the website is displayed in a window with a width greater than 800 pixels, the desktop mixin overrides the original width and sets the text width to 60%. 
Otherwise, the text has a width of 80%. 
This means that the text will fill your phone screen, and sit nicely in the middle on your desktop or tablet or phablet. 
Rather than put pictures of the results, I want to encourage you to resize your window/open the site on your phone and see what happens! 
This is my very first website ever, so if something looks ugly or doesn't work, please [let me know](/about.html#contact).

For my next challenge, I want to make my site more accessible for all three of my visitors. 
If someone with a screen reader wants to be my fourth visitor, I want to make it as easy as possible for them. 
I've also added some Google Analytics, so a) now I can feed my data directly to Google and b) I can find out that I actually have only two readers. 
Stay tuned for upcoming posts about these topics!


<h4 id="TLDR">Summary and TLDR</h4>
 * Responsive design good
 * Write mixins in SCSS so that you don't have to rewrite your media queries over and over:  
  {% highlight SCSS %}
  @mixin desktop {
  @media screen and (min-width: 601px) {
    @content;
   }
  }

  @mixin mobile {
  @media screen and (max-width: 600px) {
    @content;
   }
  }
{% endhighlight %}
 * Use them like: 
  {% highlight SCSS %}
  .responsive-class {width: 80%;
    margin: 0 auto;
    @include desktop {
      width: 60%;
    }
  {% endhighlight %}
 * Making sure to include the following somewhere on your page (I put it in the head of my ur_default layout to ensure it propagated to all pages)
  {% highlight html %}
  <meta content="width=device-width, initial-scale=1" name="viewport"/>
  {% endhighlight %}
 * Further instructions and explanations [here](https://davidwalsh.name/write-media-queries-sass)
 * [This article](https://superdevresources.com/image-caption-jekyll/) showed me how to add an image template that uses liquid tags to make inserting images much easier
 * Accessability also good, coming soon to a blog post near you
 * Analytics possibly good - blog post on that as soon as I have six (6) visitors
 * Despite what was advertised above, I fixed the inheritance structure 🎉
