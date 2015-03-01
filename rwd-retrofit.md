**Adapting your Existing Layout into a Responsive Layout – A RWD Retrofit**

View the demo:[ http://www.lib.montana.edu/~jason/files/rwd-retrofit/](http://www.lib.montana.edu/~jason/files/rwd-retrofit/)

Download the files:[ https://github.com/jasonclark/rwd-retrofit](https://github.com/jasonclark/rwd-retrofit)

**Step 1: Set the viewport to be adaptable and responsive**

Our first goal with any retrofit is to make sure our site has some instructions for controlling the width and scale of the browser’s viewport. In this case, we are looking to set an HTML tag that tells the browser how to control the page's dimensions and scaling. It’s a simple tag with incredible power.

```sh
<meta name="viewport" content="width=device-width, initial-scale=1">
```

We place the meta viewport tag inside of the <head> tags at the top of the HTML document to make sure these instructions are found as the page is rendered by the browser. Each of the values inside of the content attribute introduces a specific rule. width=device-width tells the page to find and match the screen’s width. Once this match is made, it allows the page to adapt content to match different screen sizes. initial-scale=1 defines the initial scale of the page and the zoom level. Without setting this initial scale the web page will zoom out in small screen settings and appear as a tiny version of what you might see on a desktop computer. Add the <meta name="viewport" tag to all pages that you are turning into responsive pages.

**Step 2: Identify your Method for Assigning Responsive CSS Rules**

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

  /* large styles*/

}

@media (max-width:50.1em) {

  /* medium styles */

}

@media (max-width:30.063em) {

  /* smal styles */

}
```
I prefer this method (if you have access to the original stylesheet) because it minimizes the performance hit and keeps the styles isolated in one place. The choice is yours and will really depend on what files you can access.

**Step 3: Identify the fixed grid elements and rework it into linear blocks**

With the meta viewport tag in place and having figured out how to bring in our new CSS rules, we are ready to move on to finding the grid elements on the page that don’t allow the page content to reflow into linear, stacked elements. Typical, grid elements might include…

Our first step in setting up the foundation of the site begins with the <head> tag on the index.html file which identifies the main CSS styles and name for the page. Add the name that you want for the page in between the <title> tags.
```sh
<head>

<meta charset="utf-8">

<meta http-equiv="X-UA-Compatible" content="IE=edge">

<meta name="viewport" content="width=device-width, initial-scale=1">

<meta name="description" content="">

<meta name="author" content="">

<link rel="icon" href="./favicon.ico">

<title>Springfield Public Library - RWD Example</title>

<!-- Bootstrap core CSS -->

<link href="./css/bootstrap.min.css" rel="stylesheet">

<!-- Custom styles for this template -->

<link href="./css/master.css" rel="stylesheet">

<!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->

<!--[if lt IE 9]>

<script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>

<script   src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>

<![endif]-->

</head>
```
**Step 4: Find the common fixed width elements and reset as flexible-width**

Common elements such as images and video are often set up as having a defined width. This is fine if we could reasonably depend on a screen being a certain size. However,

