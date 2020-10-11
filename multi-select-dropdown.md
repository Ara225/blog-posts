HTML provides a native way to create a select box which supports selecting multiple items with the multiple attribute on select elements but the rendering of this really isn't very compact or pretty, and there's not much you can do styling wise with options in a select box either. I couldn't find any custom designs that fitted the style I was looking for so I ended up making my own.

This is built from scratch due to difficulties in changing the behavior and style of a normal select. The normally visible part of the select is a simple button. The dropdown menu is a div hidden by the Bootstrap utility class d-none, with rounding and shadowing applied via the Bootstrap utility classes shadow and rounded. The options are simple checkboxes with labels. These could be replaced with radio buttons if you wanted a matching single select box.

Here's the final HTML for the button and menu
```html
<div>
    <button onclick="dropDown(event);" class="menu-btn" type="button">
        Menu 1 &#9013;
    </button>
    <div class="d-none shadow rounded menu">
        <span class="d-block menu-option"><label><input type="checkbox">&nbsp;
                Option 1</label></span>
        <span class="d-block menu-option"><label><input type="checkbox">&nbsp;
                Option 2</label></span>
        <span class="d-block menu-option"><label><input type="checkbox">&nbsp;
                Option 3</label></span>
    </div>
</div>
```

I wanted the menu button to be rounded and quite small so I styled it accordingly (you can style any way you wish of course, doesn't affect the function at all):
```css
.menu-btn {
    border-radius: 48px;
    border: 0.5px solid lightgrey;
    font-size: 0.9em;
    padding: 2px 10px;
    background-color: white;
}
```

For the menu itself, I added a little padding so that text wasn't running up against the top of the menu and some margin so it didn't overlap with the button (both completely optional). I also added a high z-index (so that it'd display over other stuff), a background color (default is transparent which looks silly in this context), and set position to absolute so it doesn't push other stuff down the page.
```css
.menu {
    padding-top: 10px;
    z-index: 200;
    margin-top: 4px;
    background-color: white;
    position: absolute;
}
```

For the menu options, I added a little padding to separate them.
```css
.menu-option {
    padding: 6px 20px 6px;
}
```

We need a way to detect when clicks are made outside of the dropdown menu so that it can be dismissed by clicking outside of it like a normal select box. I've done this by creating a div which covers the whole screen with a lower z-index than the menu. This allows us to detect all clicks outside of the menu
```html
<div class="d-none" id="overlay" onclick="hide(event)"></div>
```
```css
#overlay {
    position: absolute;
    top: 0px;
    left: 0px;
    width: 100%;
    height: 100%;
    z-index: 100;
}
```
To make this actually work, we need JavaScript functions to make the menus and overlay appear and disappear.
This function removes the d-none classes from the menu and overlay, activating them. Rather than handling this via ID, it simply takes the second element of the target's (button's) parent and assumes that's the menu. This is the reason that the menu and button are wrapped in an otherwise empty div.
```javascript
function dropDown(event) {
    event.target.parentElement.children[1].classList.remove("d-none");
    document.getElementById("overlay").classList.remove("d-none");
}
```

This function adds the d-none class to the overlay and all elements with the class menu, hiding them.
```javascript
function hide(event) {
    var items = document.getElementsByClassName('menu');
    for (let i = 0; i < items.length; i++) {
        items[i].classList.add("d-none");
    }
    document.getElementById("overlay").classList.add("d-none");
}
```

If you found this useful, you might also like my <a href="https://dev.to/ara225/simple-search-bar-design-286a">matching search bar design</a>.
