---
layout: post
date: 2019-12-08
title: Adding some minor responsive design
---

 I'm on a plane today, so it's the perfect time to remember that a) I actually need to Google things every two minutes while writing any kind of code, b) responsive design is functionally just a buzzword to me at the moment, and c) United Airlines sucks and I refuse to pay them for internet. 

If, in the spirit of stop-describing-your-childhood-just-give-me-the-goddamn-recipe<!--link to the author of lot being mad about that here-->, you just want to know how to do the thing, <a href="#TLDR">jump to the TLDR</a>.  

This website has about four pages and I can already feel the tech debt building, so I had planned to do clean-up and consolidation work on the plane. 
On the other hand, on a plane without internet is the perfect place to try to implement something I don't understand.

{% include image.html
img="/assets/pictures/20191207/layouts.png"
title="all my layouts"
caption="Somehow default inherits from ur_default but other things do not - this is definitely on the must-fix list" %}

Giving things my best shot without internet, I switched all my widths to percentages rather than pixels. 
This resulted a sexy-looking mobile layout but a dumb-looking desktop layout with the text stretching way too far across the screen. 
Mobile first design!
{% include image.html
img="/assets/pictures/20191207/mobile_layout.png"
title="Mobile layout"
caption="Mobile layout good." %}
  
{% include image.html
img="/assets/pictures/20191207/desktop_layout.png"
title="Desktop layout"
caption="Desktop layout bad." %}

Clearly I need some media queries. 
According to the internet (from my layover in Denver), I might also need mixins and other things I've never heard of. 
Reader, I'm sorry for the fake news earlier: I'm simply not good enough to create responsive design without people on the internet telling me how to do it, and in fact I'm milking the plane as a narrative device to show you the before and after. 
Don't worry: I didn't pay United their blood money, I just wrote the rest of this once I got to Cincinnati.  

Internet restored, I found an article called [Write Better Media Queries with Sass](https://davidwalsh.name/write-media-queries-sass). 
Turns out mixins allow you to define styles or anything other than top-level statements like imports and function definitions (very cool that there are functions in SASS). 
In this case, I used a desktop width only, since I was mostly concerned that my site looked dumb on desktop. 
The mixin allows me to define some css that will override the default if a condition is met - in this case, if the screen is greater than 800 pixels wide.
Here's the SCSS code: 

```scss
$desktop-width: 800px;

@mixin desktop {
  @media (max-width: #{$desktop-width}) {
    @content;
  }
}

.blog-post-text {
  font-family: $sans-serif;
  width: 80%;
  margin: 0 auto;
  @include desktop {
    width: 60%;
  }
  sub {
  color: grey;
  }
  a {
  color:grey;
  text-decoration: none;
  &:hover {
  text-decoration: underline;
  }
 }
}
```

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
 * Responsive design not as hard as it looks with SCSS - follow the instructions [here]((https://davidwalsh.name/write-media-queries-sass))
 * [This article](https://superdevresources.com/image-caption-jekyll/) showed me how to add an image template that uses liquid tags to make inserting images much easier
 * Accessability also good, coming soon to a blog post near you
 * Analytics possibly good - blog post on that as soon as I have six (6) visitors
 * Despite what was advertised above, I fixed the inheritance structure 🎉
