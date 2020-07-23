# Building a Dynamic GitHub Timeline
## Overview 
This post is an overview of a micro project I built, and think others might find useful. It's a dynamic, responsive timeline to display GitHub projects on your personal website. It's built using vanilla HTML, CSS and JS, so it's pretty easy to understand and customize. I've got one hosted on my site <a href="http://www.aitchisonsoft.co.uk/tools/github-timeline.html">here</a> (this also works as a timeline generator if you'd prefer a static version). The full code is located <a href="https://gist.github.com/Ara225/060e3050c7c32b7b975cd9f6b6b9b81b">on GitHub here</a>.

## Getting the Data
The simplest place to get the data is direct from Github's public API. This doesn't require authentication, so we can access it safely from client side code. GitHub actually has two public APIs: V2, which is a standard REST and JSON API and V3 which is a GraphQl API, but I'm using the V2 for this as it's a bit easier to work with. Full documentation for the API endpoint can be found <a href="https://developer.github.com/v3/repos/#list-repositories-for-a-user">here in GitHub's API documentation</a>, but it's not too complicated.

The basic API integration code below gets a list of public repos owned by username, offloads the loading into the page to fillTimeline and alerts the user if there are errors (might want to handle this more gracefully in production). 
```javascript
function getUserRepos(username) {
    // Call the GitHub API
    var response = fetch("https://api.github.com/users/" + username + "/repos?sort=updated")
    // Callback for successful response
    response.then(result => {
        // Convert into JSON
        var jsonResponse = result.json()
        // Success callback
        jsonResponse.then(json => {
            if (json.message) {
                alert('Unable to find GitHub user for that username.')
                return
            }
            fillTimeline(json)
        })
        // Error callbacke
        jsonResponse.catch(error => {
            alert('An error occurred while parsing the JSON result. The error was: ' + error.toString())
        })
    })
    // Callback to catch network or other errors preventing the call
    response.catch(error => {
        alert('An error occurred while calling the GitHub API. The error was: ' + error.toString())
    })
}
```

### Displaying the Data
The function fillTimeline below handles displaying the data. This should be pretty simple; it gets an HTML element representing the 
timeline div, and iterates through the repos, adding HTML to create a box for each one, and automatically switching between left and right aligned containers. 

It generates the header text by splitting the repo name on either dashes or capital letters, and capitalizing each word in the result (so hello-world, helloWorld Hello-World and HelloWord all become Hello World). This works quite well for my repos, but if you use a different naming convention, you'll probably need to tweak it.
```javascript
function fillTimeline(json) {
    var container = document.getElementsByClassName("timeline")[0]
    container.innerHTML = ""
    var title
    var isFork
    var projectHomepage
    var className = "left"
    for (var i = 0; i < json.length; i++) {
        // Split the repo name so it makes a nice header. This deals with repo names in the format hello-world or Hello-World
        title = json[i].name.split("-")
        // If that failed, split on capital letters instead - this deals with repo names in the format HelloWord or helloWorld
        if (title.length <= 1) {
            title = json[i].name.match(/[A-Z]+[^A-Z]*|[^A-Z]+/g);
        }
        // Capitalize each word in the resulting split name
        for (var j = 0; j < title.length; j++) {
            title[j] = title[j][0].toUpperCase() + title[j].slice(1, title[j].length)
        }
        // If the GitHub repo has a link to a website, this ensures projectHomepage contains a link to it, otherwise it contains 
        // an empty string
        projectHomepage = (json[i].homepage) ? ' | <a href="' + json[i].homepage + '">Project Homepage</a>' : ''
        isFork = (json[i].fork) ? 'Forked Project | ' : 'Original Project | '
        // Add a item to the timeline with details of this repo
        container.innerHTML += '<div class="event-container-' + className + '">' +
                                    '<div class="content">' +
                                        '<h2>' + title.join(" ") + '</h2>' +
                                        '<p>' +
                                            json[i].description +
                                        '</p>' +
                                        '<br>' +
                                        isFork + json[i].language + ' | <a href="' + json[i].html_url + 
                                        '">View Code</a>' + projectHomepage +
                                    '</div>' +
                                '</div>'
        // Ensures that the next container appears on the opposite side of the timeline
        className = (className == "left") ? "right" : "left"
    }
}
```

## Design
The design was sourced from <a href="https://www.w3schools.com/howto/howto_css_timeline.asp">this W3 Schools guide</a> - they offer it as a free reusable template. The HTML is stripped down to a mere outline as the timeline is rendered by JavaScript, and I changed a few small things in the CSS to prevent the styles from conflicting with Bootstrap.

## Possible Improvements
* The current code does not handle issues with GitHub at all well; it'll alert the user - it could do with a fallback of some kind
* Currently, it simply displays all of the requested username's GitHub repos - it could easily be set up to filter these with tags or by language.
* Doesn't currently handle absence of an attribute very well - displays null for must attributes.