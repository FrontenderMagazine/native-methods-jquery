# Native equivalents of jQuery functions

* Update: many people have asked about browser compatability for the native
methods I’ve shown. Here are the links to that information:
[querySelector/querySelectorAll][1], [classList][2], [getElementsByClassName][3],
[createDocumentFragment][4].*

---

If you checked out [my last post][5] you’ll know that I have been doing lots of
JavaScript coding as of late, both inside and out of [Brackets][6]. I have also
been doing a series of performance tests ([1][7], [2][8], [3][9]) between popular jQuery
methods and their native DOM equivalents.

Yes I know what you’re thinking. Obviously native methods are faster because
jQuery has to deal with older browsers and host of other things. I completely
agree. That’s why this post is not meant at all to be anti-jQuery. But if you
are able to target modern browsers in your work, using the native C++ methods
provided by your browser will not-surprisingly give you a tremendous
performance boost in most areas.

I think there are many developers who don’t realize that most of the jQuery
methods they use have native equivalents that require the same or only a
slighter larger amount of code to use. Below are a series of code samples
showing some popular jQuery functions along with their native counterparts.

## Selectors

Easily being able to find DOM elements is at the core of what jQuery is about.
You can pass it a CSS selector string and it will retrieve all of the elements
in the DOM that match that selector. For the most part, this can all be easily
achieved natively using the same amount of code.

Native equivalents to jQuery selectors:

  //----Get all divs on page---------

    /* jQuery */
    $("div")

    /* native equivalent */
    document.getElementsByTagName("div")

  //----Get all by CSS class---------

    /* jQuery */
    $(".my-class")

    /* native equivalent */
    document.querySelectorAll(".my-class")

    /* FASTER native equivalent */
    document.getElementsByClassName("my-class")

  //----Get by CSS selector----------

    /* jQuery */
    $(".my-class li:first-child")

    /* native equivalent */
    document.querySelectorAll(".my-class li:first-child")

  //----Get first by CSS selector----

    /* jQuery */
    $(".my-class").get(0)

    /* native equivalent */
    document.querySelector(".my-class")

## DOM manipulation

Another area where jQuery is used frequently is in manipulating the DOM, by
either inserting or removing elements. To do these things properly with native
methods, you will definitely have to write some extra lines of code, but of
course you can always write your own helper functions to make things like this
easier. Below is an example of inserting a set of DOM elements into the body
of a page.

Native equivalents to jQuery DOM functions:

  //----Append HTML elements----

    /* jQuery */
    $(document.body).append("<div id='myDiv'><img src='im.gif'/></div>");

    /* CRAPPY native equivalent */
    document.body.innerHTML += "<div id='myDiv'><img src='im.gif'/></div>";

    /* MUCH BETTER native equivalent */
    var frag = document.createDocumentFragment();

    var myDiv = document.createElement("div");
    myDiv.id = "myDiv";

    var im = document.createElement("img");
    im.src = "im.gif";

    myDiv.appendChild(im);
    frag.appendChild(myDiv);

    document.body.appendChild(frag);

  //----Prepend HTML elements----

    // same as above except for last line
    document.body.insertBefore(frag, document.body.firstChild);

## CSS classes

It is very easy in jQuery to add, remove, or check for the existence of CSS
classes on DOM elements. Luckily it is just as easy to do this with native
methods. I only recently found out about these myself. Thanks you Chrome
DevTools.

Native equivalents to jQuery class functions:

  // get reference to DOM element
  var el = document.querySelector(".main-content");

  //----Adding a class------

  /* jQuery */
  $(el).addClass("someClass");

  /* native equivalent */
  el.classList.add("someClass");

  //----Removing a class-----

  /* jQuery */
  $(el).removeClass("someClass");

  /* native equivalent */
  el.classList.remove("someClass");

  //----Does it have class---

  /* jQuery */
  if($(el).hasClass("someClass"))

  /* native equivalent */
  if(el.classList.contains("someClass"))

## Modifying CSS properties

The need to programmatically set and retrieve CSS properties using JavaScript
comes up all the time. When doing this it is much faster to simply set the
individual styles one by one rather than passing them all to jQuery’s CSS
function. It really isn’t any additional code either.

Native equivalent for setting CSS properties:

  // get reference to a DOM element
  var el = document.querySelector(".main-content");

  //----Setting multiple CSS properties----

  /* jQuery */
  $(el).css({
    background: "#FF0000",
    "box-shadow": "1px 1px 5px 5px red",
    width: "100px",
    height: "100px",
    display: "block"
  });

  /* native equivalent */
  el.style.background = "#FF0000";
  el.style.width = "100px";
  el.style.height = "100px";
  el.style.display = "block";
  el.style.boxShadow = "1px 1px 5px 5px red";

Remember  jQuery is an amazing library that makes all of our lives easier.
But you should always choose to use native DOM methods if they are available
to you. This is especially true if you are using jQuery inside of loops or
timers.

Now of course, I have been hanging around game developers for a while now so
maybe I’m a tad over-sensitive about performance. In that world if your game
doesn’t run at 60 FPS, you might as well go work at Target. Anyway, hope this
post helps some folks!

[1]: http://caniuse.com/#search=queryselector
[2]: http://caniuse.com/#search=classlist
[3]: http://caniuse.com/#search=getelementsbyclassname
[4]: http://www.quirksmode.org/dom/w3c_core.html#miscellaneous
[5]: http://www.leebrimelow.com/responsive-design-with-adobe-brackets/
[6]: http://brackets.io/
[7]: http://jsperf.com/jquery-css-vs-native-dom
[8]: http://jsperf.com/jquery-vs-document-queryselector
[9]: http://jsperf.com/innertext-vs-fragment

