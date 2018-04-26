---
layout: component
---

# Form label test

## No form wrapper

<label for="test">Explicit label</label>
<input name="test" id="test" type="text">

<label>
  <span>Implicit label</span>
  <input name="test2" type="text">
</label>

## Form wrapper

<form action="">
  <label for="test3">Explicit label</label>
  <input name="test3" id="test3" type="text">

  <label>
    <span>Implicit label</span>
    <input name="test4" type="text">
  </label>
</form>

## Dialog with no form wrapper
<button id="opendialog1">Open dialog 1</button>
<dialog id="dialog1">
  <label for="test5">Explicit label</label>
  <input name="test5" id="test5" type="text">


  <label>
    <span>Implicit label</span>
    <input name="test6" type="text">
  </label>
  <button id="cancel1">Cancel</button>
</dialog>

## Dialog with form wrapper
<button id="opendialog2">Open dialog 2</button>
<dialog id="dialog2">
  <form action="">
    <label for="test7">Explicit label</label>
    <input name="test7" id="test5" type="text">


    <label>
      <span>Implicit label</span>
      <input name="test7" type="text">
    </label>
    <button id="cancel2">Cancel</button>
  </form>
</dialog>


<script>
  (function() {
  var openButton1 = document.getElementById('opendialog1');
  var openButton2 = document.getElementById('opendialog2');
  var cancelButton1 = document.getElementById('cancel1');
  var cancelButton2 = document.getElementById('cancel2');
  var dialog1 = document.getElementById('dialog1');
  var dialog2 = document.getElementById('dialog2');

  // Update button opens a modal dialog
  openButton1.addEventListener('click', function() {
    dialog1.showModal();
  });
  openButton2.addEventListener('click', function() {
    dialog2.showModal();
  });

  // Form cancel button closes the dialog box
  cancelButton1.addEventListener('click', function() {
    dialog1.close();
  });
  cancelButton2.addEventListener('click', function() {
    dialog2.close();
  });
  })();
</script>
