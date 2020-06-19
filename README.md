# levelGaugeXY - _level gauges for x and y indices_
Navigation and visual display of the current and largest values of the array indices [x, y]
  
**The reason for the creation**  
Need to visualize tabular data navigation.

**Development goal**  
Visual display of sizes and current index values array [x, y] on the minimum number of HTML + CSS elements managed by JS

**Application**  
In a design in which content is formed based on formatted data, for example: json, two-dimensional array, etc.
  
**The essence of the idea**  
![the idea of level gauges](https://github.com/slesareva-gala/gittest/blob/master/levelgaugexy_ua.png "level gauges for x and y indices")

## 1. Strips for displaying index level gauges  
This is the space between the boundaries of a pair of block elements \<div\>:  
```html 
<div id="base"><div id="work"></div></div>
```
**base**    
    Responsible for the placement on the page and for the style of the strips of the gauges,  
    _CSS style:_  
```css 
#base { 
  position: relative; /* ! static */  
  overflow: hidden;
}
```
**work**
- with a width equal to or less than the width of the base;
- with a length equal to or less than the length of the base;
- coinciding with at least one side with the base;

_CSS style:_
```css 
#work { 
  position: absolute;
}
```
**The dimensions of the working \<div\> and its positioning in the base \<div\> are basis for identification of strips**.

The **X** strip for displaying the **x** level gauge is horizontal the space between the boundaries of the base \<div\> and working \<div\>:
|  |  |
| --- | --- |
| **location:** | top or bottom |
| **positioning:** | horizontally equal to the left shift of the working \<div\> |
| **width:** | equal to the width of the working \<div\> |
| **height:** | equal to the height of the horizontal space between the boundaries of the base and working \<div\>|
| **visualization:** | CSS style of the base \<div\>: **_box-shadow: inset_**.... |

The **Y** strip for displaying the **y** level gauge is vertical the space between the boundaries of the base \<div\> and working \<div\>:
|  |  |
| --- | --- |
| **location:** | left or right |
| **positioning:** | vertically equal to the downward shift of the working \<div\> |
| **width:** | equal to the width of the vertical space between the boundaries of the base \<div\> and working \<div\> |
| **height:** | equal to the height of the worker \<div\> |
| **visualization:** | CSS style of the base \<div\>: **_box-shadow: inset_**.... |

## 2. Index Indicators  
A bar chart built on strips X and Y CSS-style working \<div\>: **_box-shadow_**.

Main characteristics of index level gauges:
|  |  |
| --- | --- |
| **location:** | in the respective strips **X** or **Y** |
| **coordinate origin:** | side of the square of the intersection of strips **X** and **Y** adjacent to the strip |
| **width:** | equal to the width of the corresponding strip |
| **length:** | **calculation:**  |
| |  _for x:_   **x\*w/xMax** |
| |  _for y:_   **y\*h/yMax** |  
| |  **exception:** |
| |  _at x==xMax==0:_ **w** |
| |  _at y==yMax==0:_ **h** |
| |  where: |
| |  **xMax** - the largest value of **x** , _at_ xMax \> 0 |
| |  **yMax** - the largest value of **y** , _at_ yMax \> 0 |
| |  **x** - the current value of x, _at_ 0\<=x\>=xMax-1 |
| |  **y** - the current value of y, _at_ 0\<=y\>=yMax-1 |
| |  **w** - the length of the strip X corresponding to xMax |
| |  **h** - the strip length Y corresponding to yMax |
| **dependence on the index:** | - **direct** - when the index increases, the level gauge increases proportionally|  
| |  - **inverse** - when the index increases, the level gauge proportionally reduced |
  
***
# IMPLEMENTATION: levelGaugeXY
**Level gauges for x and y indices:** HTML5 + CSS3 + JS (vanilla)

**It has the following advantages:**  
- written in vanilla JavaScript, has no dependencies;
- it has a simple API;
- includes adjusting the size of the strips and level gauges when scaling;
- browser windows by pressing _Ctrl-wheel_;
- navigation through indexes with a _click_ of the mouse on the stripes;
- change indices by rotating the mouse wheel over the base div:  
  - index y - _wheel_,  
  - index x – _Shift-wheel_.

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
