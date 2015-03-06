**Adapting your Existing Layout into a Responsive Layout – A RWD Retrofit**

View the demo:[ http://www.lib.montana.edu/~jason/files/rwd-retrofit/](http://www.lib.montana.edu/~jason/files/rwd-retrofit/)
Download the files:[ https://github.com/jasonclark/rwd-retrofit](https://github.com/jasonclark/rwd-retrofit) or[ http://www.lib.montana.edu/~jason/files/rwd-retrofit.zip](http://www.lib.montana.edu/~jason/files/rwd-retrofit.zip)


We often work within vendor settings in our library application development. That is, we are often working to integrate "built-systems" that have been purchased to provide one of our library services. Some examples of these “built-systems” might include our Integrated Library Systems ([http://en.wikipedia.org/wiki/Integrated_library_system](http://en.wikipedia.org/wiki/Integrated_library_system)) or course guide implementations such as LibGuides from SpringShare ([http://springshare.com/libguides/](http://springshare.com/libguides/)). And it’s not just libraries, many businesses purchase systems to help workflows and enhance services. Anyone who has purchased a Content Management System (CMS) has had to work to integrate the default CMS look and feel into the brand and themes of existing websites and services. In this project, we will look at some of the challenges and solutions for adapting legacy and external systems into designs that work responsively. The goal will be to distill first principles of RWD that can be applied to “built-systems” and what to look for to make these default designs responsive. Our case study will be remaking a fixed-width digital library application into a responsive digital library application for multiple screens ([http://arc.lib.montana.edu/national-park-service-webcams/item/7](http://arc.lib.montana.edu/national-park-service-webcams/item/7)).

(Figure 5.01 – Digital Library Application on Large Screen, Item Page without RWD)

Whenever we refer to the "page" in this chapter, we are talking about this item page with three-columns, a fixed header, and a fixed footer. In this chapter, the takeaway will be a roadmap document showing the first principles of RWD, the problems they correct, and the common solutions to make them responsive. The idea is that you can use the roadmap when you have to approach integrating legacy or built-systems responsively into your website.



**/C Step 1: Set the Viewport to be Adaptable and Responsive**

Our first goal with any retrofit is to make sure our site has some instructions for controlling the width and scale of the browser’s viewport. In this case, we are looking to set an HTML tag that tells the browser how to control the page's dimensions and scaling. It’s a simple tag with incredible power. Hopefully, you will have access to the HTML source of the pages to add this tag. This is the simplest means, but you might have to get creative and figure out other ways to edit this tag. Some browsers are starting to implement a viewport declaration ([https://developer.mozilla.org/en-US/docs/Web/CSS/@viewport](https://developer.mozilla.org/en-US/docs/Web/CSS/@viewport)) that might be able to do this in the future - @viewport {width:device-width;}. There are also some possibilities around inserting the <meta> tag with Javascript ([http://www.quirksmode.org/blog/archives/2011/06/dynamically_cha.html](http://www.quirksmode.org/blog/archives/2011/06/dynamically_cha.html)). At any rate, the tag you want to add is below.

```sh
<meta name="viewport" content="width=device-width, initial-scale=1">
```

Place the meta viewport tag inside of the <head> tags at the top of the HTML document to make sure these instructions are found as the page is rendered by the browser. Each of the values inside of the content attribute introduces a specific rule. width=device-width tells the page to find and match the screen’s width. Once this match is made, it allows the page to adapt content to match different screen sizes. initial-scale=1 defines the initial scale of the page and the zoom level. Without setting this initial scale the web page will zoom out in small screen settings and appear as a tiny version of what you might see on a desktop computer. Add the <meta name="viewport" tag to all pages that you are turning into responsive pages. In our digital library application case study, the meta viewport tag is added to the head of the document and the first step of our retrofit is in place.

** **

**/C Step 2: Identify your Method for Assigning Responsive CSS Rules**

Our next goal is to figure out how we are going to get our CSS rules onto the page. Two methods are options here. First, we can try to link to new stylesheets that have media queries within the <link> tag to identify when to load and use the styles.

```sh
<link rel="stylesheet" href="original.css">
<link rel="stylesheet" href="rwd-large.css" media="(min-width:50.1em)">
<link rel="stylesheet" href="rwd-medium.css" media="(max-width:50.1em)">
<link rel="stylesheet" href="rwd-small.css" media="(max-width:30.063em)">
```

Note that these new stylesheet calls are placed in the <head> and follow a cascading order to make sure they override the styles that immediately precede them when a media query is matched to a screen or device. This method works and may be your only option for a retrofit, but it does add 3 new HTTP requests to your page which could impact performance. The second method is my preference and involves adding the media queries directly to the original.css document.

```sh
/* original stylesheet styles */
@media (min-width:50.1em) {
  /* large styles */
}
@media (max-width:50.1em) {
  /* medium styles */
}
@media (max-width:30.063em) {
  /* small styles */
}
```

I prefer this method (if you have access to the original stylesheet) because it minimizes the performance hit and keeps the styles isolated in one place. The choice is yours and will really depend on what files you can access.

** **

**/C Step 3: Set the base font size to the browser default**

With the meta viewport tag in place and having figured out how to bring in the new CSS rules, we need to set the base font size for the web browser. Many systems will set specific pixel dimensions for their base font size and when these dimensions are fixed our responsive design breakpoints have a hard time determining when to flow into the next screen setting. Look in the body and html rules of the original stylesheet to see if you can spot the font-size settings. Once you find it, reset the styles to a relative unit that will force the browser into its default font pixel size of 16 pixels. In this case, you want to set the font size to 100%. At 100%, the browser will use a size equivalency where 16px ~= 1em setting. This sizing equivalency will allow us to build our breakpoints around Em units (another relative measurement unit). This ties our breakpoints to our content size and makes for a design aligned with type size (an emerging RWD best practice -[ http://blog.cloudfour.com/the-ems-have-it-proportional-media-queries-ftw/](http://blog.cloudfour.com/the-ems-have-it-proportional-media-queries-ftw/)).  In our digital library application case study, we switch the font-size:small; to font-size:100%;



**/C Step 4: Determine and Set the Breakpoints**

           	Our goal has always been to find a way to "linearize" the content on our pages. The single-column pattern works well in medium and small screen settings which are the goal of a retrofit. We haven’t seen what will happen as we start to move to a single column layout, but the screenshot below gives us a picture of our retrofit redesign on a medium screen.

(Figure 5.02 – Digital Library Application on Medium Screen, Item Page with RWD)

Before this can happen, we have to come up with the dimensions for our breakpoints. Remember how I mentioned that we set the browser default to 16 pixels. We can use that knowledge to build our breakpoints using the relative Em units that work and scale with our typographic settings. Pixels can’t do that. So, to make our breakpoints, we have to do some division. If we wanted an 800 pixel breakpoint, we would divide 800/16 to equal 50. Similarly, for a 480 pixel breakpoint, we would divide 480/16 to get 30. You’ll see these results expressed in our two media query breakpoints below.

```sh
/*medium screen view < 801px*/
@media all and (max-width:50.1em) {
  /*medium screen styles here*/
}
/*small screen view < 481px*/
@media all and (max-width:30.063em) {
  /*small screen styles here*/
}
```

We only introduce two breakpoints - medium and small - because our legacy system already has a large screen view as its default. With these media queries in place, our work to target the problematic page elements with new CSS rules can begin.



**/C Step 5: Identify the Fixed Layout Elements and Rework into Flexible Blocks**

With the baseline RWD work done, we are ready to move on to finding the specific HTML elements on the page that don’t allow the page content to reflow into linear, stacked elements. Typical, these might be grid-like content elements including, but not limited to: any content columns, search result pages, lists of items, data tables, etc. Try to identify these items in your own site. In our case study, there is a primary grid of three items on our items page. We are going to rework those grids into a single column suitable for a medium and small screens. Our first step is to find the HTML markup and the CSS rules that forms the grid layout. You can do this by using your browser’s developer tools. Use the "View -> Developer Tools" menu in Chrome and the “Tools -> Web Developer” menu in Firefox. The screenshot below shows how you can use Google Chrome’s Developer Tools to inspect the page elements. When you think you have a match use the “magnifying glass” icon to select the HTML element and you will get a listing of all the associated CSS rules.

(Figure 5.03 – Google Chrome Developer Tool View – HTML and CSS Selection)

From this inspection, we can see that the three-column grid for the page is created using a mix of CSS table and float layout. It could be worse. Tables will flow between screens, but we can do even better. Our goal is to turn off the table and float layout and let the content blocks stretch across the screen. Our CSS media queries get some new rules here to help us accomplish that task.

```sh
/*medium screen view < 801px*/
@media all and (max-width:50.1em) {
  ul.item li {display:block;}
  ul.metadata li.object, ul.metadata li.describe, ul.metadata li.action {float:none;width:85%;}
}
/*small screen view < 481px*/
@media all and (max-width:30.063em) {
  ul.item li {display:block;}
  ul.metadata li.object, ul.metadata li.describe, ul.metadata li.action {float:none;width:85%;}
}
```

For both breakpoints, we turn off the table and float styles using the display:block; and float:none; rules. These rules create our single-column layout that you saw in Figure 5.02. These type of rules are the exactly what you would use if you were going to try to create a single-column retrofit of another site as well.



**/C Step 6: Find the Common Fixed-Width Elements and Reset as Flexible-Width**

           	Another step in the retrofit process is the incorporation of flexible media objects. This is the second component of the RWD model (fluid grid layouts, **flexible media objects**, and media queries). It is part of the model because common elements such as images and video are often set up as having a defined width. So, a generic goal here is to come up with a set of CSS rules that can redefine those widths as relative. We’ll work through an image example, but these rules can be applied other items that you might want to set up as responsive. Again, using our browser developer tools, we can see that our images are set to a fixed-width. We’ll want to change that by adding a new class to our media queries.

```sh
.img-responsive {
  display:block;
  height:auto;
  width:100%;
  max-width: 100%;
}
```

We will pair that new class with a new HTML class on any <img> tag that we want to be flexible.

```sh
<img class="img-responsive" src="…" />
```

With the CSS and HTML in place, you will see your images start to flex in any screen according to your settings.

(Figure 5.04 – Digital Library Application on Small Screen, Item Page with RWD)

And this CSS/HTML pattern will work for other media objects as well, you might try setting up a very generic .responsive class with similar CSS rules that you can attach to any item you want to adapt to screen size.



**/C Step 7: Make a Flexible Header and Footer including Global Navigation**

           	In our example, there are a few more fixed items that we will want to modify. First, it has a giant header image that will not work on medium or small screens. It also has long titles in and body text that will break the design on smaller screens. And finally, it has a global navigation and a footer that are fixed to a widescreen view. In order to make the small screen view that we saw in Figure 5.04, there are a couple more additions to make to the CSS rules. About that header: let’s remove the image, add an ellipses when the header text hits a breakpoint, and slide the global navigation together to fit on smaller screens.

```sh
div#mastHead {height:45px;}
#mastHead img {display:none;}
#mastHead h1 a {color:#fff;line-height:normal;text-decoration:none;padding-left:33px;}
h1.visuallyhidden {border:0 auto;width:100%;height:auto;margin:0;padding:0;opacity:1;clip:auto;color:#fff;width:750px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
```

Note that we set display:none; to hide the image. We also need to add a .visuallyhidden class to our <h1> tag to make it place an ellipses when text flows off the screen. We use this same set of styles in the .visuallyhidden class to make our body titles adaptable and on the The global navigation and the footer are using a float layout and we can adapt that layout to make it fit on smaller screens.

```sh
#nav ul li#searchForm {float:left;width:auto;}
#footer ul li#links {float:left;width:auto;}
```

We can push the links and search form in the header closer together by switching the float to appear next to the other items. We follow a similar pattern as we reset the CSS rules in the footer. With those final rule changes in place, the final medium screen CSS looks something like the sample below. (Note: the small screen CSS rules are almost identical.)

```sh
/*medium screen view < 801px*/
@media all and (max-width:50.1em) {
  ul.item li {display:block;}
  ul.metadata li.object, ul.metadata li.describe, ul.metadata li.action {float:none;width:85%;}
  .img-responsive {display:block;height:auto;width:100%;max-width:100%;}
  div#mastHead {height:45px;}
  #mastHead img {display:none;}
  #mastHead h1 a {color:#fff;line-height:normal;text-decoration:none;padding-left:33px;}
  h1.visuallyhidden {border:0 auto;width:100%;height:auto;margin:0;padding:0;opacity:1;clip:auto;color:#fff;width:750px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
  #nav ul li#searchForm {float:left;width:auto;}
  #footer ul li#links {float:left;width:auto;}
}
```

**/C Step 8: Thinking About the Generic Techniques of the Retrofit**

           	In the last step of the retrofit, there are a few more takeaways if you want to think about how to apply these techniques generically. First, find ways to make things disappear. We did this with display:none; and/or the opacity:1;clip:auto; for our .visuallyhidden class. Second, allow content to stretch into its container. We did this with our max-width and width:100%; settings. And finally, reuse existing layouts by changing the default layout rule. We did this by resetting the table layout to display:block and our reworking of the float layout. Retrofitting is not perfect and has its limitations, but it can also provide an improved user experience. It is worth learning some of these generic RWD retrofit techniques to give yourself some options to work more flexibly with legacy systems.

