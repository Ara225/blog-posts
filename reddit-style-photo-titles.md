## Overview
Ever noticed the little photo titles at the top of Reddit's front page? I really love the look of these, so I decided to build something similar into my site for the links to my projects. They're not quite perfect for this as they only really let you do one link but none of my projects are hosted right now (not job hunting) so this isn't a big deal for me.

I've made a couple of changes to the design to make it work with my site. I'm using Bootstrap (currently 4.5.0) on my site, so I wanted to build them them to work with that. I also wanted them to have a bit more box shadow and increase it when hovering over them to make it obvious they're clickable, and make sure they're responsive. 

If you'd rather skip the tutorial/explanation, you can find a complete example here <a href="https://gist.github.com/Ara225/7d747836348e000cd9d4178c53dc0260">here</a> You can also see the version on my website <a href="http://www.aitchisonsoft.co.uk/">here</a>

## HTML
Here's the HTML outline I came up with:
```html
<section class="container-flex" id="projects">
    <div class="row">
        <a href="#" target="_blank" class="col-md-4 col-lg-3 col-12 project-card-wrapper">
            <div class="col-12 project-card">
                <div class="project-card-content">
                    <h4>
                        <b>
                            Project Name
                        </b>
                    </h4>
                    <p>
                        Short description
                    </p>
                </div>
            </div>
        </a>
    </div>
</section>
```
It's a simple design with the tiles being columns in a row inside a full width container. This works really well as it allows you to have a completely arbitrary number of tiles with them wrapping if they need to. I've also used Bootstrap classes to enable the columns to resize responsively. I had considered using Bootstrap's cards and their card layout options, but, as Bootstrap themselves say, these layout options are not yet responsive. Each column has a col-12 column inside it containing the content, as this enables the cards to have nice regular spacing between them.

## CSS
Creating this effect without either editing the photo to add a gradient or putting up with illegible text requires layering some stuff over the image. Reddit did this using the actual image as the background for the a element and a partially transparent gradient as a background image on the ::before pseudo element and I've done the same with a few small tweaks. There's a full  breakdown of the styles below if you want to understand it better.

### Section element 
Neither of these styles are compulsory, but they help to make it look good.
```css
#projects {
    padding:5%;
    background-color: #DAE0E6;
}
```

### Outer Column
This simply ensures that there's vertical spacing between the columns when they wrap - the one thing that the row doesn't handle.
```css
.project-card-wrapper {
   margin: 3% 0%;
}
```

### Inner Column
Styling for the ::before pseudo element. It utilizes a simple gradient made up of black with varying degrees of transparency as the background.
```css
.project-card::before {
    background-image: linear-gradient(0deg, #000, rgba(0, 0, 0, .8) 25%, rgba(0, 0, 0, .6) 50%, rgba(0, 0, 0, .4) 75%, rgba(0, 0, 0, .2));
    background-position: center;
    content: "";
    border-radius: 8px;
    bottom: 0;
    left: 0;
    position: absolute;
    right: 0;
    top: 0;
}
```

Styling for the card itself. Defined a small box-shadow to make it stand out from the background. Have to define the min width and height because the card gets malformed otherwise (no background images).
```css
.project-card {
    box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
    margin-top: 2%;
    background: rgb(255, 255, 255) url(YOUR_IMAGE_URL) no-repeat scroll left center / cover; 
    min-width: 12em;
    min-height: 15em;
    border-radius: 8px;
}
```

A small increase on box shadow when hovered over to make it clear it's clickable.
```css
.project-card:hover {
    box-shadow: 0 8px 15px 0 rgba(0, 0, 0, 0.5), 0 4px 20px 0 rgba(0, 0, 0, 0.49);
}
```

### Card content
Simple styles to place the text in the right place and ensure it stands out against the background.
```css
.project-card-content {
    color: white;
    opacity: 1;
    position: absolute; 
    bottom: 0px;
}
```