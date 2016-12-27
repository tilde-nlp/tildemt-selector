# TildeMT system selection webcomponent
## Prerequisites
You might want to include the (webcomponents.js)[https://github.com/webcomponents/webcomponentsjs]
polyfill to support older browsers. There are no other dependencies.

## Usage
### Including the webcomponent
It should be as symple as
```HTML
<html>
<head>
    <link rel="import" href="path-to/tildemt-selector.html">
</head>
<body>
	<tildemt-selector id="tildemt" ></tildemt-selector>
</body>
</html>
```

### Attaching the event handlers
The selector component notifies it's state changes via a couple of events.
This is also one of the ways it passes out it's state which encodes the
selected systems.

#### save
The save event is used to notify that the `save` button has been pressed
indicating that the user has finished selecting her systems.
```JavaScript
document.getElementById("tildemt").addEventListener("save", function(details){
    // event details contain the selector component's state
    tildemtConfig = details.detail;
    // we'll look at this method in greater detail later
    populateSystemList(tildemtConfig);
});
```

#### cancel
The event is used to notify that the `cancel` button has been pressed
indicating that the user has aborted the system selection process
and wishes to undo the changes she's made.
```JavaScript
document.getElementById("tildemt").addEventListener("cancel", function(){
    // use the previous saved state
});
```

#### logout
The event notifies that the `logout` button has been pressed indicating that
the webcomponent's state should be dropped so that the user can authenticate
with a different Client ID.
```JavaScript
document.getElementById("tildemt").addEventListener("logout", function(){
    // drop the saved state
    tildemtConfig = undefined;
});
```

### Demo
For a full working example see [the demo](../index.html).

## Source code structure
TODO
