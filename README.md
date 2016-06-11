jquery-ui-memory-game
=====================

Jquery plugin (widget) for creating "memory" card games from a list of links and images.

See it in action: http://matteosistisette.github.io/jquery-ui-memory-game


Requirements
------------

To be able to use the plugin, place the downloaded `css` and `js` folders in the webroot of your page, and paste this code into the `<head>` of your html:
```html
<!-- DEPENDENCIES: jQuery and jQuery-UI -->
<script src="https://code.jquery.com/jquery-2.2.4.js"></script>
<script src="https://code.jquery.com/ui/1.11.4/jquery-ui.js"></script>

<!-- INCLUDE THE PLUGIN (js and css) -->
<link rel="stylesheet" href="css/jquery.memory-game.css">
<script src="js/jquery.memory-game.js"></script>
```

    
Basic usage
-----------
There are two ways of building a "Memory" card game using this plugin:
- Build the list of cards through **HTML markup** (links wrapped around images), and convert it into a Memory game with one simple line of JavaScript
- or, define the list of cards as a **JavaScript array** and pass it as a parameter to the widget constructor

###Using HTML markup
See complete live example here: [http://jsbin.com/kawipa](http://jsbin.com/kawipa/edit?html,output)

HTML markup:
```html
<div id="memory-game">
  <a href="http://en.wikipedia.org/wiki/Iguana"><img width="100" height="100" src="example-images/iguana.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Panda"><img width="100" height="100" src="example-images/panda.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Lemur"><img width="100" height="100" src="example-images/lemur.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Penguin"><img width="100" height="100" src="example-images/penguins.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Polar_bear"><img width="100" height="100" src="example-images/polarbear.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Rabbit"><img width="100" height="100" src="example-images/rabbit.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Rhinoceros"><img width="100" height="100" src="example-images/rhino.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Common_seal"><img width="100" height="100" src="example-images/seal.jpg"></a>
  <a href="http://en.wikipedia.org/wiki/Zebra"><img width="100" height="100" src="example-images/zebra.jpg"></a>
</div>
```

JavaScript code:
```html
<script>
$(function(){
    $("#memory-game").memoryGame();
});
</script>
```

**NOTE:** The size of the card images in pixels must be known (and fixed) when the widget is instantiated. That's why we need the explicit `width` and `height` html attributes, at least on the first image; alternatively, CSS could be used. See below for more details and alternatives.

###Using a JavaScript array
See complete live example here: [http://jsbin.com/vonaye](http://jsbin.com/vonaye/edit?html,output)

HTML markup (just a container):
```html
<div id="memory-game"></div>
```
    
JavaScript code:
```html
    <script>
    $(function(){
      $("#memory-game").memoryGame({
        cards: [
          {
            imageUrl: 'example-images/iguana.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Iguana'
          },
          {
            imageUrl: 'example-images/panda.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Panda'
          },
          {
            imageUrl: 'example-images/lemur.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Lemur'
          },
          {
            imageUrl: 'example-images/penguins.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Penguin'
          },
          {
            imageUrl: 'example-images/polarbear.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Polar_bear'
          },
          {
            imageUrl: 'example-images/rabbit.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Rabbit'
          },
          {
            imageUrl: 'example-images/rhino.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Rhinoceros'
          },
          {
            imageUrl: 'example-images/seal.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Common_seal'
          },
          {
            imageUrl: 'example-images/zebra.jpg',
            linkUrl: 'http://en.wikipedia.org/wiki/Zebra'
          }
        ],
        cardWidth: 100,
        cardHeight: 100
      });
    });
    </script>
```

**NOTE:** if you use this method, you have to specify the card image height and width via the `cardWidth` and `cardHeight` parameters.

Styling
-------

To change the appearence of the cards, just add your own **CSS stylesheet** to override the default style rules applied by the plugin.

You can place your stylesheet inside a `<style></style>` tag, or in a css file linked via `<link rel="stylesheet">`. Just make sure to put your style **after** the link to `js/jquery.memory-game.css`.

Here's an example:
```html
<style>
.card.back, .card.front {
  border: solid 4px #272;
  border-radius: 12px
}
.card.back {
  background: url('example-images/back.jpg');
} 
.card.front {
  border-color: #472a09;
}
</style>
```

See it live here: [http://jsbin.com/cupefu](http://jsbin.com/cupefu/edit?html,output)


Handling events
---------------

See complete live example here: [http://jsbin.com/puxafos](http://jsbin.com/puxafos/edit?html,output)

You may want to make something happen every time a new pair of matching cards is succesfully disclosed. You do so by passing in a callback function as the **`onPairDisclosed`** parameter:
```html
    <script>
    $(function(){
      $("#memory-game").memoryGame({
        onPairDisclosed:function(info) {
          if (info.finished) {
            alert("Congratulations! You have finished the game in "+info.moves+" moves!\n\n"+"Now we'll start over.");
            this.reset(true, true);  // (animated, shuffle cards)
          }
          else {
            alert(
              "Congratulations! You have disclosed " +
              info.disclosedPairs +
              " of " + info.totalPairs + " matching pairs.\n" +
              "The link will now open in a new window"
            );
            window.open($(info.card).attr("href"),"popup"+info.disclosedPairs,"width=400,height=400");
          }
        }
      });
    });
    </script>
```
    
This callback function will be called every time a new pair of matching card is disclosed succesfully, and it will be passed one parameter (named `info` in the example above) which is an object containing the following properties:

- **`card`**: (`Element`) the HTML `<a>` node that wraps the card that has just been disclosed. This is also the preferred way of accessing any information associated to the card (see the section *Binding extra data to the cards* below)
- **`cardIndex`**: (`Number`) the index of the disclosed pair in the array of cards (one element per pair of cards). `this.options.cards[info.cardIndex]` is the same as `info.cardInfo` described below. Usually you shouldn't need this, unless you are storing information related to the cards in a separate array, which is not recommended (see *Binding extra data to the cards* below) or you are doing weird stuff with the array of cards.
- **`cardInfo`**: (`Object`) the object containing information about the disclosed card pair. This object has at least two properties, `imageUrl` and `linkUrl`; if you defined the array of cards yourself in JavaScript and passed it to the widget constructor, or if you bound data to the card (see *Binding extra data to the cards* below) it may contains more properties.
- **`disclosedPairs`**: (`Number`) the number of matching pairs of cards succesfully disclosed until now, including the one that triggered the event
- **`totalPairs`**: (`Number`) the total number of pairs of cards in the game
- **`moves`**: (`Number`) the number of moves (i.e. clicks on cards)
- **`finished`**: (`Boolean`) whether this was the last pair of cards in the game.

Note that, inside the code of your `onPairDisclosed` callback function, **`this`** is the widget instance.


Controlling card layout
-----------------------

### Card rotation

By default, the widget rotates each card by a **small random angle** in order to give them a "real" feel. You can adjust the maximum rotation to your taste by using the **`maxRotation`** parameter:
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    maxRotation: 25 // maximum rotation in either direction in degrees
  });
});
</script>
```
See it live here: [http://jsbin.com/xubajew](http://jsbin.com/xubajew/edit?html,output)

Of course you can set `maxRotation` to zero if you don't want any rotation at all:
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    maxRotation: 0 
  });
});
</script>
```
See it live here: [http://jsbin.com/wuxelo](http://jsbin.com/wuxelo/edit?html,output)

### Card spacing

The widget will do its best to lay out the cards in the "nicest" possible way while profiting the available space (see the next section for more details). In doing this, it will adjust the **horizontal and vertical space between adjacent cards** as needed, but always between a minimum and a maximum.

You can set these minimum and maximum value with the `minCardMargin` and `maxCardMargin` parameters: 
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    minCardMargin: 20,
    maxCardMargin: 100
  });
  // Wide min-max range:
  //   => will grow and shrink a lot depending on the available space
});
</script>
```
See it live here: [http://jsbin.com/bixonex](http://jsbin.com/bixonex)

The card *margin* is by definition half the distance between the edges of two adjacent cards. These parameters determine the minimum and maximum values for it, in pixels.

You can of course set the same values for both parameters to have a fixed distance between adjacent cards:
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    minCardMargin: 30,
    maxCardMargin: 30
  });
  // Same value for min and max:
  //   => distance between cards is fixed. The number of rows and columns may vary a bit more.
});
</script>
```
See it live here: [http://jsbin.com/canamop](http://jsbin.com/canamop)

To see the effects of the values on the layout in the examples above, open them in a big screen and try resizing the browser window to see how the layout adapts.

**NOTE:** the distance between the edges of two adjacent card is computed **as if they were not rotated**, so when deciding the `minCardMargin` you need to take into account the extra space needed to accomodate for the rotation.


### Rows:columns Aspect Ratio

By default, the widget will try to keep the ratio of nº of columns vs nº of rows as close to 1:1 as possible. However:
- it will **never exceed the available width** (determined by the width of the containing HTML node), no matter what. So, the number of cards per row will be reduced as much as needed to fit within the available width;
- it won't exceed the **height of the browser window** if doing so would leave **any empty wasted space** on the sides.

In other words, the widget will _**prefer**_ a "square" layout, as long as it doesn't exceed the available width, and as long as it doesn't _unnecessarily_ exceed the browser window height while leaving unused empty space. 

You can alter this behavior by means of the **`preferredAspectRatio`** parameter. Its default value is `1` (prefer a square aspect ratio). You can set it to a different value to aim at a different aspect ratio:
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    preferredAspectRatio: 2 // aim at a 2:1 ratio of columns:rows if possible, instead of the default 1:1
  });
});
</script>
```
See it live here: [http://jsbin.com/kefaqo](http://jsbin.com/kefaqo). In this example, the game will tend to have 3 rows of 6 cards if the window is wide and high enough.

You can **disable the preferred aspect ratio** by setting it to **zero**:
```html
<script>
$(function(){
  $("#memory-game").memoryGame({
    preferredAspectRatio: 0 // No preferred aspect ratio. Always put as as many cards per row as the available width allows.
  });
});
</script>
```
See it live here: [http://jsbin.com/bekuwe](http://jsbin.com/bekuwe).

### Overriding automatic resizing

By default, the widget automatically attaches its own listener to its HTML node's **`onresize`** event, calling the widget's public `resize` method which refreshes the layout.

Usually this means you don't have to do worry about anything, and the widget will just adapt its layout automatically every time the window is resized. This is usually the case even if you have your own listener for the window's (or other DOM nodes') `onresize` event which in turn programmatically change the widget's DOM node's width: because this will trigger the widget's listener attached to the node's event.

In some cases, however, you may want to take control yourself over the resizing of the widget. This might be the case if for some reason you need to attach your own listener(s) to the `onresize` event of the very widget's DOM node, or if you want to 
