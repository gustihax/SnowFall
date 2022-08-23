# SnowFall
Falling snow animation rendered in jQuery. For animation, the TweenMax library from GreenSock is used. Snowflakes have a different blur, due to which the effect of three-dimensionality of what is happening is achieved.

# HTML
The snow itself is added to the container with an ID

```html
<div id="snow-animation-container"></div>
```

# CSS
There are no styles, the very design of snowflakes is formed by a js script. 

We only manage the container that holds all the snowflakes.

# JS
For snow to work, you need to include the jQuery and TweenMax libraries on the page.

We connected them from CDN.

```js
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/1.20.3/TweenMax.min.js"></script>
```


And after connecting the file with the script script.js

By manipulating the values ​​of variables:

+ **MAX_SNOW = 200 — maximum number of snowflakes**

+ **MAX_SNOW_SIZE = 7 — maximum snowflake size in pixels**

+ **MAX_SNOW_SPEED = 1 — snowflake acceleration**

You can increase the amount of snow, control the size and speed of falling.

The entire code is shown below:

```js
$(document).ready(documentReady);

function documentReady() {
  var MAX_SNOW = 200;
  var MAX_SNOW_SIZE = 7;
  var MAX_SNOW_SPEED = 1;

  snowStart();

  function snowStart() {
    // console.log("// Snow animation start");
    createSnows();
  }

  function createSnows() {

    var container = $("#snow-animation-container");

    for (var i = 0; i < MAX_SNOW; i++) {
      var appendItem = getRandomItem(i);
              container.append(appendItem);
      var animateItem = $(".snow" + String(i));
      var randomTime = Math.random() * MAX_SNOW_SPEED;
      goAnimate(animateItem, i, randomTime);
      goAnimate2(animateItem);
    };

    // console.log("// Create snows");
  }

  function goAnimate(item, id, randomTime) {
    TweenMax.to(item, randomTime, {
      css: {
        marginTop: "+=100"
      },
      ease: Linear.easeNone,
      onComplete: function() {
        var topPosition = item.css("margin-top").replace("px", "");
        if (topPosition > $(window).height()) {
          changePosition(item);
          randomTime = Math.random() * MAX_SNOW_SPEED;
          goAnimate(item, id, randomTime);
        } else {
          goAnimate(item, id, randomTime);
        }

      }
    });
  }

  function goAnimate2(item) {

    var directionTime = 1 + Math.floor(Math.random() * 5);
    var randomDirection = 1 + Math.floor(Math.random() * 4);
    var delayTime = 1 + Math.floor(Math.random() * 3);

    if (randomDirection == 1) {

      TweenMax.to(item, directionTime, {
        css: {
          marginLeft: "+=100"
        },
        ease: Linear.easeOut,
        onComplete: function() {

          TweenMax.to(item, directionTime, {
            css: {
              marginLeft: "-=100"
            },
            delay: delayTime,
            ease: Linear.easeOut,
            onComplete: function() {
              goAnimate2(item);
            }
          });
        }
      });
    } else if (randomDirection == 2) {

      TweenMax.to(item, directionTime, {
        css: {
          marginLeft: "-=100"
        },
        ease: Linear.easeOut,
        onComplete: function() {
          TweenMax.to(item, directionTime, {
            css: {
              marginLeft: "+=100"
            },
            delay: delayTime,
            ease: Linear.easeOut,
            onComplete: function() {

              goAnimate2(item);

            }
          });
        }
      });
    } else if (randomDirection == 3) {

      TweenMax.to(item, directionTime, {
        css: {
          marginLeft: "+=100"
        },
        ease: Linear.easeOut,
        onComplete: function() {
          goAnimate2(item);
        }
      });
    } else if (randomDirection == 4) {

      TweenMax.to(item, directionTime, {
        css: {
          marginLeft: "-=100"
        },
        ease: Linear.easeOut,
        onComplete: function() {
          goAnimate2(item);
        }
      });
    }
  }

  function changePosition(item) {
    var _width = Math.floor(Math.random() * MAX_SNOW_SIZE);
    var _height = _width;
    var _blur = Math.floor(Math.random() * 5 + 2);
    var _left = Math.floor(Math.random() * ($(window).width() - _width));
    var _top = -$(window).height() + Math.floor(Math.random() * ($(window).height() - _height));

    item.css("width", _width);
    item.css("height", _height);
    item.css("margin-left", _left);
    item.css("margin-top", _top);
    item.css("-webkit-filter", "blur(" + String(_blur) + "px)");
    item.css("-moz-filter", "blur(" + String(_blur) + "px)");
    item.css("-o-filter", "blur(" + String(_blur) + "px)");
    item.css("-ms-filter", "blur(" + String(_blur) + "px)");
    item.css("filter", "blur(" + String(_blur) + "px)");
  }

  function getRandomItem(id) {
    var _width = Math.floor(Math.random() * MAX_SNOW_SIZE);
    var _height = _width;
    var _blur = Math.floor(Math.random() * 5 + 2);
    var _left = Math.floor(Math.random() * ($(window).width() - _width));
    var _top = -$(window).height() + Math.floor(Math.random() * ($(window).height() - _height));
    var _id = id;

    return getSmallSnow(_width, _height, _blur, _left, _top, _id);
  }

  function getSmallSnow(width, height, blur, left, top, id) {
    var item = "<div class='snow" + id + "' style='position:absolute; margin-left: " + left + "px; margin-top: " + top + "px; width: " + width + "px; height: " + height + "px; border-radius: 50%; background-color: white; -webkit-filter: blur(" + blur + "px); -moz-filter: blur(" + blur + "px); -o-filter: blur(" + blur + "px); -ms-filter: blur(" + blur + "px); filter: blur(" + blur + "px);'></div>"
    return item;
  }

}
```