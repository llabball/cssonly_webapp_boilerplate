# Installation

Just upload the `index.html` and the `webapp_skeleton.css` to web space (couchdb, web server, dropbox etc.) or save them to disk.

Relax.

# Customization

In the demo a second css file (`demo_assets/css/webapp_styles.css`) is used to hold all the individual styles. Its recommended to leave the skeleton css unchanged - a update will not affect the individual work. If the css should be minified and concatenated in production- just use a tool like grunt.

In the head of the style file the main css selectors can be found. Along them nearly everything is customizable.

# Compatibility 

HTML5 Browser only! Tested on iOS6 and ubuntu/osx desktops with Chrome, Firefox, Safari 

# Documentation

The project was to find a **webapp boilerplate** with the following requirements:

* [x] no JavaScript needed
* [x] JavaScript friendly (minimal side effects)
* [x] page transitions
* [x] responsive
* [x] left-, top-, right menus (and bonus points for bottom menu)
* [x] overflowing contents and native scrolling on mobile devices
* [ ] optional touch and swipe handling (via JavaScript)

## The skeleton
<figure style="float:right;">
  <img src="https://rawgithub.com/llabball/cssonly_webapp_boilerplate/master/demo_assets/img/cssonlywebappboilerplate_initview.svg" width="200">
  <figcaption>fig. 1 the app as a user will see it initially</figcaption>
</figure>

The webapp interface is simple as it can be. A full-screen content area overlapped from a fixed navigation bar on top. Not shown on the image are three navigation bar button (left|center|right) to reach the menus.

<div style="clear:both;"></div>

### the exploded view

<figure style="float:right;">
  <img src="https://rawgithub.com/llabball/cssonly_webapp_boilerplate/master/demo_assets/img/cssonlywebappboilerplate_explosion.svg" width="300">
  <figcaption>
    fig. 2 all layers with z-index and ids<br> containers are transparent and bordered <br>with a dashed line</figcaption>
</figure>

The layers in order of their z-index from bottom to top:

* **#menurightcontent** (blue): has a size of 80x100% and is aligned (left:20%) to the right border of the screen.
* **#viewport/#viewcontent** (lightgray): The #viewcontent is a html child node of the #viewport. Both have the "same" z-index. Only the #viewcontent gets the webapp contents - the #viewport is transparent and a helper for the page transitions. Both are 100x100% sized (image is abstract at this point).
* **#menutopcontainer/#menutopcontent** (green): Same as before. The container is the hidden helper and and the #menutopcontent is visible. The container is 100x100% sized the #menutopcontent has only a max-height of 100% and will have the height of the inline contents.
* **#navbarcontainer/#navbarcontent** (darkgray): #navbarcontent holding the buttons and title. The navbar is 100%x40px sized and has a fixed positioning overlapping the view.
* **#menuleftcontent** (red): has a size of 80x100% and is moved to a invisible area (left:-100%) left aside the left of the screen.

### containers and contents

The naming convention is that all *content ids are for visible parts and *container describing transparent layers. The equally named container/content pairs have always the same relationship: *container is the parent html node - *content is the inherit child node.

The **purpose of the containers** is to handle the **scrolling bar** behavior of the browser. **Horizontal scrolling should be prevented in all situations.** Simply spoken: if the *content layer moves the *container stays still - the browser has no reason to provide scroll bars.

Some will ask: why not simply using the `overflow-x: hidden` solution? Answer: Because the native Mobile Safari scrolling will be  broken. A added `-webkit-overflow-scrolling: touch` fixes it but can't handle axis-based conditions. As the result the view is horizontal scrolling is back.

## the transitions

### #menuleft

<figure style="float:right;">
<img src="https://rawgithub.com/llabball/cssonly_webapp_boilerplate/master/demo_assets/img/cssonlywebappboilerplate_menuleft.svg" width="300">
<figcaption>
    fig. 3 #menuleft transition related layers</figcaption>
</figure>

The #menuleft click starts the most parallel transitions of all. 

The #menuleftcontent moves simply 100% to the right - remember, the layer has a 80% width and the highest z-index of all layers. It will be finally positioned `left:0;top:0` and overlapping all menu content (so all content and scrolling will be accessible).

The #navbarcontent and the #viewcontent moving simultaneity 80% to the right. The containers preventing horizontal scroll bars. 

<div style="clear:both;"></div>

### #menuright

<figure style="float:right;">
<img src="https://rawgithub.com/llabball/cssonly_webapp_boilerplate/master/demo_assets/img/cssonlywebappboilerplate_menuright.svg" width="300">
<figcaption>
    fig. 4 #menuright transition related layers</figcaption>
</figure>

Didn't found a solution to animate the right menu the same as the left menu.

The concept uses css3 transitions as trigger for the layer movements. For that kind of css-definitions the layer must have already the initial position. But there is no default hidden area on the right side of a browser (as on the left). The only way to deal with was a lower z-index. The right menu can't move but the overlapping viewcontent can.

If the left menu should look the same their is a problem. That is only possible when the menus have both max 50% of the screen width. Otherwise they overlapping and one menu will not fully displayed. To change the z-index in a transition is a very rough thing and doesn't worked 100%. 

In difference to the #menuleft transition the #menuright click starts to move the containers not the contents. The reason is the higher z-index of the containers compared to the #menurightcontent. To make the menu visible AND accessible the container moving and taking their childs with them.
 

<div style="clear:both;"></div>

### #menutop

<figure style="float:right;">
<img src="https://rawgithub.com/llabball/cssonly_webapp_boilerplate/master/demo_assets/img/cssonlywebappboilerplate_menutop.svg" width="200">
<figcaption>
    fig. 5 #menuright transition related layers</figcaption>
</figure>

This is simple. The #menutopcontainer moving 100% to the bottom. The inherit #menutopcontent is only as height as its content - the underlaying #viewcontent will be probably visible.

Because of the higher z-index the #navbarcontent will stay on top and the #menuleftcontent scrolls behind it to the bottom.

<div style="clear:both;"></div>

## the click handlers

All have began with the investigations of the `:target`-selector of css3. The initial idea was to replace some boring JavaScript code (the events and animations for a webapp page transitions) with fresh and funky css solutions.

The `:target`-selector replaces the JavaScript click-event. In the css a property of a DOM element can be changed if that DOM element is "targeted". A DOM element is a fragment and a fragment can be targeted over the url e.g. `http://host.tld#ID_OF_THE_DOM_NODE`. In many cases the browser scrolls automatically to the maybe hidden fragment. In the following example the font will turn to red when the former url is called: `#ID_OF_THE_DOM_NODE:target {color: red;}`.

