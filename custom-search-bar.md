This post is about a search bar I've made for a project I'm working on. It's mostly vanilla HTML and CSS, though I'm using Bootstrap to define the sizing and Font Awesome for the search icon. Both can easily be dispensed with though by using the sizing classes in your framework of choice and sourcing a search icon elsewhere.

## HTML
I wanted a search bar which would expand to almost 100% of the screen on small devices and contract to about three-fourths on larger screens. I additionally wanted it to have an actual button to submit the search rather than just a icon. This is what I came up with: 
```html
<form class="p-3">
    <div class="col-12 col-md-8 container">
        <input id="search-bar">
        <button class="fas fa-search btn" id="search-btn"></button>
    </div>
</form>
```

## CSS
For the input element, I set it to a fixed height of 46px and added border radius values that turn the ends of it into neat semi-circles. I added a subtile border and a bit of padding so that the curved ends and button never conceal the text. 
```css
#search-bar {
    height: 46px;
    border-radius: 48px;
    border: 0.5px solid lightgrey;
    width: 100%;
    padding-right: 40px;
    padding-left: 10px;
}
```

For the button, as input elements don't let you embed HTML inside them, I used absolute positioning to place it over the end of the input. Bootstrap container elements always have 15px padding so it needs to be 15px away from the container edge. I also added border radius values only to the right end and a border on the left end to make it clear there was a difference. 
```css
#search-btn {
    height: 46px;
    border-top-right-radius: 48px;
    border-bottom-right-radius: 48px;
    padding: 15px;
    font-size: 1.1em;
    position: absolute;
    right: 15px;
    border-left: 0.5px solid lightgrey;
} 
```
If you want to add text to the search button, increase padding-right on the search bar to account for the longer button, remove the fas fa-search classes from the button and remove the padding styling.

