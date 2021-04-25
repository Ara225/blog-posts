One of the disadvantages of Bootstrap is that it depends on jQuery (at least up ) and it's own Bootstrap JS files for some functions such as modals. If all you want is a simple modal and have no other use for the JS, this can add a good few unnecessary kilobytes of JavaScript you don't actually need to your page. However, it's not too difficult to work around - in fact, it requires less than 20 lines of JS. The full code is <a href="https://github.com/Ara225/mini-frontend-projects/blob/master/2-bootstrap-modal-without-jQuery/bootstrap-modal-without-jQuery.html">here</a>

We use the same HTML as the normal Bootstrap modals, except with the extra attributes being replaced by click handlers, and an extra div (backdrop) which displays the grey background.
```html
<button type="button" class="btn btn-primary" onclick='openModal()'>
    Launch demo modal
</button>
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-modal="true"
    role="dialog">
    <div class="modal-dialog" role="document">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
                <button type="button" class="close" aria-label="Close" onclick="closeModal()">
                    <span aria-hidden="true">×</span>
                </button>
            </div>
            <div class="modal-body">
                ...
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" onclick="closeModal()">Close</button>
                <button type="button" class="btn btn-primary">Save changes</button>
            </div>
        </div>
    </div>
</div>
<div class="modal-backdrop fade show" id="backdrop" style="display: none;"></div>
```

The JavaScript simply alters the backdrop and modal display when required, as well as adding and removing the show class. The last few lines can be deleted so that it doesn't close after a click elsewhere in the page.
```javascript
function openModal() {
    document.getElementById("backdrop").style.display = "block"
    document.getElementById("exampleModal").style.display = "block"
    document.getElementById("exampleModal").classList.add("show")
}
function closeModal() {
    document.getElementById("backdrop").style.display = "none"
    document.getElementById("exampleModal").style.display = "none"
    document.getElementById("exampleModal").classList.remove("show")
}
// Get the modal
var modal = document.getElementById('exampleModal');

// When the user clicks anywhere outside of the modal, close it
window.onclick = function(event) {
  if (event.target == modal) {
    closeModal()
  }
}
```