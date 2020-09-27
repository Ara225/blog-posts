Having to use jQuery to enable Bootstrap to do one small thing like a collapsing menu can be a bit overkill. Luckily, however this can easily be worked around.

You can use the same HTML as for any other Bootstrap collapsing navbar, except with a click handler on the navbar-toggler and an ID on the div wrapping the collapsing section.

```html
<nav class="navbar navbar-light bg-white navbar-expand-md">
    <div class="container col-12">
        <a class="navbar-brand" href="/">
            <b>TestSite</b>
        </a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbar-list"
            aria-controls="navbarNav" aria-expanded="true" aria-label="Toggle navigation"
            onclick="displayMenu(event)">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbar-list">
            <ul class="nav navbar-nav ml-auto ">
                <li class="nav-item">
                    <a class="nav-link" href="/blog">Blog</a>
                </li>
                <li class="nav-item">
                    <a class="nav-link" href="/tools/">Tools</a>
                </li>

            </ul>
        </div>
    </div>
</nav>
```

Then simply put this function in a script tag or in a fille and the navbar will work perfectly.
```js
function displayMenu(event) {
    if (document.getElementById("navbar-list").classList.contains("show")) {
        document.getElementById("navbar-list").classList.remove("show")
    }
    else {
        document.getElementById("navbar-list").classList.add("show")
    }
}
```

If you found this useful, my article <a href="https://dev.to/ara225/how-to-use-bootstrap-modals-without-jquery-3475">How to Use Bootstrap Modals Without jQuery</a> may be interesting to you also.