# windog
Browser window alert, prompt, and confirm alternative using HTML dialog and template.

Support backdrop click to close dialog using backdrop element. See dialog setup example below.

## Dialog Setup Example
index.html
```html
<template id="tmp-dialog-alert">
  <dialog class="wg-Windog">
    <!-- ^# tmp-dialog-alert -->
    <div class="backdrop"></div>
    <div class="wrapper">
      
      <form method="dialog" data-ref="form">
        <div data-kind="message"></div>
        <div class="void void-full"></div>
        <div class="text-center">
          <button>Ok</button>
        </div>
      </form>
    </div>
  </dialog>
</template>
<template id="tmp-dialog-prompt">
  <dialog class="wg-Windog">
    <!-- ^# tmp-dialog-alert -->
    <div class="backdrop"></div>
    <div class="wrapper">
      
      <form method="dialog" data-ref="form">
        <div data-kind="message"></div>
        <div class="void void-half"></div>
        
        <input class="input-style-default" type="text" data-kind="input"/>
        <div class="void void-full"></div>
        
        <div class="d-flex justify-content-end gap-full">
          <button value="ok">Ok</button>
          <button>Cancel</button>
        </div>
      </form>
    </div>
  </dialog>
</template>
<template id="tmp-dialog-confirm">
  <dialog class="wg-Windog">
    <!-- ^# tmp-dialog-confirm -->
    <div class="backdrop"></div>
    <div class="wrapper">
    
      <form method="dialog" data-ref="form">
        <div data-kind="message"></div>
        <div class="void void-full"></div>
        <div class="d-flex justify-content-end gap-full">
          <button value="ok">OK</button>
          <button>Cancel</button>
        </div>
      </form>
    </div>
  </dialog>
</template>
```

widgets.css
```css
.wg-Windog {
  &::backdrop {
    background: rgba(0, 0, 0, 0.4);
  }
  & {
    padding: 0;
    background: transparent;
  }
  .wrapper {
    background: white;
    position: relative;
    padding: 1rem;
  }
  .backdrop {
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    left: 0;
  }
  button:focus {
    border-color: #4CAF50;
    box-shadow: 0 0 0 3px rgba(76, 175, 80, 0.3);
  }
}
```

## Usage
index.js
```js
(async function() {

    await windog.alert('A warning dialog');
    let confirmResult = await windog.confirm('Waiting user action');
    let promptResult = await windog.prompt('Waiting for user input', 'Hello Vanilla JS');

    console.log('confirm result: ', confirmResult);
    console.log('prompt result: ', promptResult);

})();
```

## Customizing
windog.js
```js
// custom dialog interface
let SELF = {
    prompt,
    // ...
};

// default option, template ID, and resolve handler/callback
let dialogOptions = {
    prompt: {
        defaultValue: null,
        templateId: 'tmp-dialog-prompt',
        onResolve: (resolve, dialogEl) => {
          // provide custom callback. always resolve to show the next dialog in queue if you await the function.
          // resolve();
          handlePromptDialog(resolve, dialogEl)
        },
    },
    // ...
};

// custom dialog function
// showDialogAsync(defaultOption, userOption)
async function prompt(message='', defaultValue='') {
    return await showDialogAsync(dialogOptions.prompt, {
        message,
        defaultValue,
    });
}

// ...
```