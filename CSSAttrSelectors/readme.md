#Notes on *The Skinny on CSS Attribute Selectors* (https://css-tricks.com/attribute-selectors/)

##The basic idea

Any attribute of an HTML element may be used as the target for a CSS selector, not just `class` or `id`.

The syntax to do so is:
```
element[attr=""] { 
  property: value;
}
```
Examples of attributes other than `class` and `id` include:
* `rel=""`
* `href=""`
* `type=""`

This is very useful if you have a list of links and you want a specific style for each one; the attribute `href="linktext"` will be unique for each link, and it will not be dependent on link order if something changes down the line (sweet!).

Another scenario where attribute selectors show their value is when you need to style form inputs.  There are many types of input elements, and you can target the `type=""` attribute to style them without adding in a bunch of classes and cluttering your markup.

####Notes:
* It's best to always use quotes in your CSS as you would in the HTML, because not all browsers follow the same rules for ommiting them.
* Ids and classes are attributes, so you can use attribute selector syntax to target them - which comes in handy with the fancy stuff!
* You can use more than one attribute selector at once, and they all must match the target in order for the selector to work.

##Fancy Stuff
###Attribute Contains a Value

The basic `=` will only target elements that exactly match the value in your selector.  But if you have multiple target elements with similar attributes, you may be able to use `*=`. This will match the selector value to elements that have that value *anywhere* in the targeted attribute:

```
p[class*="font"] { color: blue; }
```
will target any paragraph that has the word *font* in its class name.  It would select the first two paragraphs below, but not the third:
```
<p class="small-font">Text</p>
<p class="medium-font">Text</p>
<p class="body-text">Text</p>
```
###Attribute Begins with a Value

Using `^=` will target elements that begin with the selector value.  Using the same paragraphs as above, we could write a selector to target only the first one, like so:
```
p[class^="small"] { font-weight: bold }

<p class="small-font">Text</p>
<p class="medium-font">Text</p>
<p class="body-text">Text</p>
```
###Attribute Ends with a Value

Using `$=` will target elements that end with the selector value. This is very useful for styling download links to match the file type, based on the file extension of the download link. If you have a downloadable photo, for example, you can target it like this:
```
a[href$=".jpg"] { whatever styles you like }
```
###Attribute is in a Spaced List

If your target element has more than one value in the attribute you are targeting (like multiple classes), and you need to target a value that is not first or last, you can use `~=`.  This will match your selector value to a target value in a space-separated list.
```
p[class~="opinion"] { font-style: italic }

<p class="small opinion window">Text</p>
```
###Attribute is the Beginning of a Dashed List

If your targeted value is at the beginning of a dashed list, you can use `|=`. 
```
p[class|="smaller"] { color: pink }

<p class="smaller-opinion-window">Text</p>
```
This is different from `^=` (which targets the starting value) because it must match the entire string that appears before the first dash, where `^=` may match the first part of the string regardless of whether or not it has reached the dash.  So, in the example below, the paragraph would be orange, not pink:
```
p[class|="small"] { color: pink }
p[class^="small"] { color: orange }

<p class="smaller-opinion-window">Text</p>
```
