## Website Performance Optimization portfolio project


---
###index.html

* Moved &lt;link href="//fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet&gt; tag to the buttom, outside &lt;html&gt; tag.

* Inside &lt;script src='http://www.google-analytics.com/analytics.j's &gt; tag add async.

* Inside &lt;link href="css/print.css" rel="stylesheet"&gt; add @media="print" .

* Minified style.css file

* Compressed image.

* Compressed style.css.

* Compressed perfmatters.js

* Compressed index.html
---

###main.js

#####Resize pizza
* Inside functon changePizzaSizes() avoid FSL. Move document.querySelectorAll(".randomPizzaContainer") outside for loop. Delete function determinDx() and set newwidth directly instead.
```js
    function changePizzaSizes(size) {
        var newwidth;
        switch(size) {
            case "1":
          newwidth = 25;
          break;
        case "2":
          newwidth = 33.3;
          break;
        case "3":
          newwidth = 50;
          break;
        default:
          console.log("bug in sizeSwitcher");
      }

    var pizzaCon = document.querySelectorAll(".randomPizzaContainer");
    var pizzaConLen = pizzaCon.length;
    for (var i = 0; i < pizzaConLen; i++) {
      pizzaCon[i].style.width = newwidth + "%";
    }
  }
  ```

#####Scolling

* Take document.body.scrollTop outside for loop to avoid FSL.
```js
function updatePositions() {
  frame++;
  window.performance.mark("mark_start_frame");

  var items = document.querySelectorAll('.mover');
  var topstyle = document.body.scrollTop / 1250;
  for (var i = 0; i < items.length; i++) {
    var phase = Math.sin(topstyle + (i % 5));
    items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
  }

  // User Timing API to the rescue again. Seriously, it's worth learning.
  // Super easy to create custom metrics.
  window.performance.mark("mark_end_frame");
  window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
  if (frame % 10 === 0) {
    var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
    logAverageFrame(timesToUpdatePosition);
  }
}
```

* Add will-change style to each img element. Reduce sliding pizzas number from 200 to 32, delete unnecessarily pizzas.
```js
document.addEventListener('DOMContentLoaded', function() {
  var cols = 8;
  var s = 256;
  for (var i = 0; i < 32; i++) {
    var elem = document.createElement('img');
    elem.className = 'mover';
    elem.src = "images/pizza.png";
    elem.style.height = "100px";
    elem.style.width = "73.333px";
    elem.basicLeft = (i % cols) * s;
    elem.style.top = (Math.floor(i / cols) * s) + 'px';
    elem.style.willChange = "transform";
    document.querySelector("#movingPizzas1").appendChild(elem);
  }
  updatePositions();
});
```