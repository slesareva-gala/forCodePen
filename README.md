# levelGaugeXY - level gauges for x and y indices

Navigation and visual display of the current and largest values
 of the array indices [x, y]

### The reason for the creation

Need to visualize tabular data navigation.

### Development goal

Visual display of sizes and current index values array [x, y] on the minimum number of HTML + CSS elements managed by JS

### Application  

In a design in which content is formed based on formatted data, for example: json, two-dimensional array, etc.

### The essence of the idea
![alt-текст](https://github.com/slesareva-gala/gittest/blob/master/levelgaugexy_ua.png "Текст заголовка логотипа 1")



## 1. **Strips for displaying index level gauges** - this is the space between the boundaries
 of a pair of block elements (\&lt;div\&gt; tags):

**\&lt;div\&gt; \&lt;div\&gt; \&lt;/div\&gt; \&lt;/div\&gt;**

where

**base \&lt;div\&gt; (external)**

Responsible for the placement on the page and the style of the strips of the gauges,

CSS style:position: !( static); overflow: hidden;

**work \&lt;div\&gt; (internal):**

- with a width equal to or less than the width of the base;
- with a length equal to or less than the length of the base;
- coinciding with at least one side with the base;

CSS style:position: absolute;

**The dimensions of the working \&lt;div\&gt; and its positioning in the base \&lt;div\&gt;
 are basis for identification of strips**.

The **X** strip for displaying the **x** level gauge is horizontal the space between the boundaries of the base \&lt;div\&gt;and working \&lt;div\&gt;:

| **location:** | top or bottom |
| --- | --- |
| **positioning:** | horizontally equal to the left shift of the working \&lt;div\&gt; |
| **width:** | equal to the width of the working \&lt;div\&gt; |
| **height:** | equal to the height of the horizontal space between the boundaries of the base and working \&lt;div\&gt; |
| **visualization**** :** | CSS style of the base \&lt;div\&gt;:
**box-shadow: inset**....
 |

The **Y** strip for displaying the **y** level gauge is vertical the space between the boundaries of the base \&lt;div\&gt; and working \&lt;div\&gt;:

| **location:** | left or right |
| --- | --- |
| **positioning:** | vertically equal to the downward shift of the working \&lt;div\&gt; |
| **width:** | equal to the width of the vertical space between the boundaries of the base \&lt;div\&gt; and working \&lt;div\&gt; |
| **height:** | equal to the height of the worker \&lt;div\&gt; |
| **visualization**** :** | CSS style of the base \&lt;div\&gt;:
**box-shadow: inset**....
 |

## 2. **Index Indicators -** A bar chart built on strips **X** and **Y** CSS-style working \&lt;div\&gt;:
**box-shadow.**

Main characteristics of index level gauges:

| **location:** | in the respective strips **X** or **Y** |
| --- | --- |
| **coordinate origin:** | side of the square of the intersection of strips **X** and **Y** adjacent to the strip |
| **width:** | equal to the width of the corresponding strip |
| **length:** | calculation: | for x: **x \* w / x\_max** |
|
 |
 | for y: **y \* h / y\_max** |
|
 | exception: | at: x == x\_max == 0: **w** |
|
 |
 | at: y == y\_max == 0: **h** |
|
 |
 | where: |
|
 |
 | **x\_max** - the largest value of **x** , where x\_max\&gt; 0 |
|
 |
 | **y\_max** - the largest value of **y** , where y\_max\&gt; 0 |
|
 |
 | **x** - the current value of x, where 0 \&lt;= x \&lt;= x\_max-1 |
|
 |
 | **y** - the current value of y, where 0 \&lt;= y \&lt;= y\_max-1 |
|
 |
 | **w** - the length of the strip X corresponding to x\_max |
|
 |
 | **h** - the strip length Y corresponding to y\_max |
| **dependence on the index**** :** | direct (when the index increases, the level gauge increases proportionally) |
| inverse (when the index increases, the level gauge proportionally reduced) |

**IMPLEMENTATION:**

**levelGaugeXY - Level gauges for x and y indices**

HTML5 + CSS3 + JS (vanilla)

**It has the following advantages:**

- written in vanilla JavaScript, has no dependencies;
- it has a simple API;
- includes adjusting the size of the strips and level gauges when scaling;
- browser windows by pressing Ctrl - mouse wheel;
- navigation through indexes with a click of the mouse on the stripes;
- change indices by rotating the mouse wheel over the base div:

index y - mouse wheel,

index x – Shift - mouse wheel

1. **Wiring diagram**** :**

\&lt;head\&gt;

....

**\&lt;script src = &quot;levelGaugeXY.min.js&quot;\&gt; \&lt;/script\&gt;**

\&lt;/head\&gt;

\&lt;body\&gt;

....

\&lt;div id = &quot;base&quot;\&gt;

\&lt;div id = &quot;work&quot;\&gt;

**\&lt;script\&gt; &#39;use strict&#39;;**

**levelGaugeXY (&#39;base&#39;, &#39;work&#39;);**

**\&lt;/script\&gt;**

....

\&lt;/div\&gt;

\&lt;/div\&gt;

....

\&lt;/body\&gt;

1. The **levelGaugeXY()** function **adds** the **lgxy** property to the base div -
**object of class LevelGaugeXY**.

**levelGaugeXY (**idBase **,** idWork **[, {**name: value**, ...}]);**

where

**idBase**

id of the base div, e.g. &#39;base&#39;

**idWork**

id of the working div, e.g. &#39;work&#39;

**{....}**

optional installation parameters that change default property values

**name: value,**

property name and value, for example, LenY: 5000,

1. **Methods that modify properties directly on the base.lgxy object**
  1. **Return the current value of the property. Can be modified with assignment operator.**

**lenX**

The quantity values **x**.

Value is a positive number.

The default value is set to 0.

**lenY**

The quantity values **y**.

Value is a positive number.

The default value is set to 0.

**x**

Current value **x**.

Value between 0 and lenX

The default value is set to 0.

**y**

Current value **y**.

Value between 0 and lenY.

The default value is set to 0.

**stepx**

Step to change **x** -value with mouse wheel.

Value is a positive number.

The default value is set to 1.

**stepy**

Step to change **y** -value with mouse wheel.

Value is a positive number.

The default value is set to 1.

**locationX**

Strip **X** placement.

Values: **&#39;top&#39;** - at the top, **&#39;bottom&#39;** - at the bottom.

Optional: offset of the working div in the base div when &#39;top&#39; - to the down, &#39;bottom&#39; – to the up.

The default value is set to &#39;bottom&#39;.

**locationY**

Strip **Y** placement.

Values: **&#39;left&#39;** - at the left, **&#39;right&#39;** - at the right.

Optional: offset of the working div in the base div when &#39;left&#39; - to the right, &#39;right&#39; - to the left.

The default value is set to &#39;right&#39;.

**dependX**

The dependence of the level gauge **X** on **x** values.

Values:

**&#39;inverse&#39;** is the inverse relationship to index growth: with increase / decrease **x** level gauge **X** decreases / increases;

**&#39;directly&#39;** - consistent with the growth of the index: with increasing / decreasing **x** level gauge **X** increases / decreases.

The default value is set to &#39;inverse&#39;

**dependY**

The dependence of the level gauge **Y** on **y** values.

Values:

**&#39;inverse&#39;** is the inverse relationship to index growth: with increase / decrease **y** level gauge **Y** decreases / increases;

**&#39;directly&#39;** - consistent with the growth of the index: with increasing / decreasing **y** level gauge **Y** increases / decreases.

The default value is set to &#39;inverse&#39;

**colorXY**

Strips color.

Values:

**&#39;none&#39;** - disable shadows style;

**&#39;rgb (...)&#39;** - drawing a shadow with the specified rgb color;

The default value is set to &#39;rgb (200,200,200)&#39;.

**colorlevel**

Color Level Gauges.

Values:

**&#39;rgb (...)&#39;** - drawing a shadow with the specified rgb color

The default value is set to &#39;rgb (100,100,100)&#39;

  1. **Service methods on the base.lgxy object**

**visible()**

Display changes in strips and level gauges.

Installed by default.

**hidden()**

Hide (do not show) strip and level gauge changes.

It can be used in case of changing a property group.

**resize()**

Update strips and level gauges according to current base \&lt;div\&gt; sizes and working \&lt;div\&gt;.

**destroy()**

Disable levelGuageXY:

Stripes and level gauges are hidden, working div offsets are canceled, listening for events level Gauge XY (&#39;resize&#39;, &#39;wheel&#39;, &#39;click&#39;) is disabled, deleted the lgxy property the base \&lt;div\&gt;.

1. **levelgaugexy event**

It is formed on the base \&lt;div\&gt; when changing the properties x, y, lenX, lenY.

Flags: bubbles set to true, cancelable set to false.

Reading at **evevt.detail** :

**base**

id of base \&lt;div\&gt;

**work**

id of the working \&lt;div\&gt;

**x || .y || .lenX || .lenY ||**

the name of the modified property is &#39;x&#39; or &#39;y&#39; or &#39;lenX&#39; or &#39;lenY&#39;

**value**

value for changing a property value

**old**

property value before change

**new**

property value after change

**Performance tested on:**

Google Chrome, Version 83.0.4103.97

Firefox Browser, Version 77.0.1

Microsoft Edge, Version 83.0.478.45
