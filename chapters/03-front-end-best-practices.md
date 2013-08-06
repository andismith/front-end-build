# Front End Best Practices

## Feature Detection

The web is evolving faster than ever before. And with browsers such as Google Chrome and Mozilla Firefox releasing new updates every six weeks we find that new features which used to take years to go from inception to delivery now take just a few weeks.

The problem for web developers is that often we still need to support more than just the latest browsers. Older browsers such as Internet Explorer 6 – 8 still count for a large amount of visitors’ browsers; and with a wide variety of mobile devices on the market, the mobile browser market is dispersed.

But rather than wait until every browser supports a particular feature, we can use feature detection on our web site to use these features for browsers that support them.

### Feature Detection vs. Browser Detection

To be able to provide these features to supported browsers, we need to be able to detect whether they are available.

In the past, developers have offered features by detecting the browser’s user agent (or user agent sniffing). However this technique is considered bad practice for a number of reasons.

* It can be unreliable, as the user agent string does not necessarily change when the rendering engine does.
* The user can manually adjust or update their user agent string.
*	The user may be able to turn on and off certain features within the browser.
*	There is a maintainability cost when new browsers are released. Every time there is a change, your user agent detection script will need updating.

// Picture of site that sniffs for a user agent

Nowadays, feature detection is preferred over user agent sniffing. Feature detection is a more reliable technique as tests are performed at the point where we know whether a particular feature has been implemented.

[Modernizr](http://modernizr.com) is an open source JavaScript library that acts as a toolkit for feature detection, making it easier for developers to detect features on their sites. 

When we include the Modernizr script in our web page, Modernizr detects what features are available and we can use the CSS classes that Modernizr generates or a JavaScript call to one of Modernizr’s methods or properties to detect whether the browser the user is viewing our site in has implemented the feature natively. If it has, we can offer the user the preferred experience. If it hasn’t, we can offer another solution – this could be a fallback to a simpler experience or a polyfill.

### Installing Modernizr

Installing Modernizr on your website is simple. You can download the latest version from the Modernizr website at http://modernizr.com/download/. For development purposes, you can include the full Modernizr feature set but for your live release you may wish to create a custom build.

// Picture of Modernizr Build Page

Next, we need to include the Modernizr JavaScript file in our website code.

Commonly the recommended place to include external JavaScript files is at the bottom of the web page to ensure that they do not block loading of the page and the user has the least amount of time to wait to see the page. But as Modernizr will be manipulating our CSS classes and may change what we choose to show, it is recommended to place the Modernizr script file include directly after our CSS includes; and not at the bottom of our HTML document. This will stop a flash of changing/un-styled content on load.

    <!DOCTYPE html>
    <html>
      <head>

        <meta charset="utf-8">
        <title>My Website</title>

        <link href="/css/common.css" rel="stylesheet" />

        <script src="/js/modernizr.min.js"></script>

      </head>
    <body>

When you refresh your page with Modernizr included, you will not notice any visual differences to your page. Modernizr sits silently in the background, ready for us to use where and when we need.

Let’s check Modernizr is loaded correctly on our page.

Open your browser’s developer tools, using F12 on Windows or Cmd, Alt (Opt) and J on Mac.

In the console prompt type:

    Modernizr;

Hit the enter key and this should return an Object.

If Modernizr is not installed, check that you have given the correct path and file name to the Modernizr JavaScript file. 

The final thing we want to do is a add a class of “no-js” to our HTML element like so:

    <html lang="en" dir="ltr" class="no-js">

As Modernizr is a JavaScript library, it requires JavaScript to be enabled to be able to run. When Modernizr initializes, it will replace the “no-js” class with “js” for users with JavaScript enabled. Should JavaScript be disabled, you can target styles with the “.no-js” selector to provide different styles where required.

### Feature Detection

Now we have the Modernizr library included on our web page, we can begin to use its features to provide our users with a better experience. In this chapter, we will take a look at two different methods of feature detection – via CSS and via JavaScript.

It’s important to note that this chapter is not a case of CSS vs. JavaScript detection. Both are useful in different ways and as we explore this chapter the use cases for both methods should become clear.
CSS Feature Detection

If you have included the default build for Modernizr, your page source will already have been modified to include the CSS classes for the features your browser supports. Let’s take a look.

*	Open up your webpage with Modernizr included, and open your browser developer tools.
*	Go to the HTML / Elements tab.
*	Find the HTML element at the top of your source.

You should see a long list of class names, similar to the example below:

    <html lang="en" dir="ltr" class="js no-touch postmessage history multiplebgs boxshadow opacity cssanimations csscolumns cssgradients csstransforms csstransitions fontface localstorage sessionstorage svg inlinesvg blobbuilder bloburls download formdata">

As you’d expect, the list of class names will vary depending on the capabilities of your browser.

Let’s take a look at the CSS property box-shadow as an example.  If you are unsure what box-shadow does, it allows us to add a shadow to block level elements.

Here’s an example of what our site would look like in browsers that do and do not support box shadow without using Modernizr’s features, and afterwards we will look at how we can enhance this with Modernizr.

    .my-box {
	    -moz-box-shadow: grey 3px 3px 1px; 
	    -webkit-box-shadow: grey 3px 3px 1px;
	    box-shadow: grey 3px 3px 1px;
    }

This is what our site will look like in browsers that do support box shadow:

// image

And this is what our site will look like in browsers that do not support box shadow without using Modernizr:

// image

Modernizr adds a CSS class to our HTML element called boxshadow when a browser supports this feature. By adjusting our CSS, we can provide a similiar experience for browsers that do not support this feature.


    .my-box {
	    border-bottom: 3px solid grey;
	    border-right: 3px solid grey;
    }

    .boxshadow .my-box {
	    border:none;

	    -moz-box-shadow: grey 3px 3px 1px; 
	    -webkit-box-shadow: grey 3px 3px 1px;
	    box-shadow: grey 3px 3px 1px;
    }

While our site will still look the same in browsers that do support box-shadow, our site now looks like this in browsers that don’t.
