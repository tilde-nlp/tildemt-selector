# TildeMT system selection webcomponent
# Prerequisites
You might want to include the [webcomponents.js](https://github.com/webcomponents/webcomponentsjs)
polyfill to support older browsers. There are no other dependencies.

# Usage
## Including the webcomponent
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

## Attaching the event handlers
The selector component notifies it's state changes via a couple of events.
This is also one of the ways it passes out it's state which encodes the
selected systems.

### save
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

### cancel
The event is used to notify that the `cancel` button has been pressed
indicating that the user has aborted the system selection process
and wishes to undo the changes she's made.
```JavaScript
document.getElementById("tildemt").addEventListener("cancel", function(){
    // use the previous saved state
});
```

### logout
The event notifies that the `logout` button has been pressed indicating that
the webcomponent's state should be dropped so that the user can authenticate
with a different Client ID.
```JavaScript
document.getElementById("tildemt").addEventListener("logout", function(){
    // drop the saved state
    tildemtConfig = undefined;
});
```

## Handling the selected systems
The state object received via the TildeMT webcomponent's `save` event has this
general structure
```JSON
{
    "engineName": "My en-ge and lv-lt engine",
    "client-id": "u-0353d8-ufub-9tga-a59a-io8a3618kb39s",
    "systems": {
		"en-de": {
			"params": {
				"systemID": "smt-83q34k17-2ob6-622l-10hk-1lb669s6ggh0",
				"options": "client=localhost,version=1.0"}},
		"lv-lt": {
			"params": {
				"systemID": "smt-f4b008c1-3152-444b-91f7-2472e0b69b63",
				"options": "client=localhost,version=1.0,qe"}}}
}
```

### engineName
The user can input a name for her newly created system configuration to later
identify it. This is usefull if the same MT plugin can be used to configure multiple
different translation engines. A single user, for example, could be using the Tilde MT
platform for two different projects each requiring her to access translation systems
belonging to different user groups.

This field is not needed when making calls to the TildeMT API. If the above described
functionality is not needed then the `engineName` field can be safely ignored. See
also [TODO].

### client-id
Used to authenticate calls to the Tilde MT API. Each call to the API must have a
`client-id` field set in the HTTP request header.

### systems
An object with keys corresponding to the different language combinations supported.
In the above example the user has selected two translation systems -- for English
to German and Latvian to Lithuanian translations. This is indicated by two ISO 639-1
language codes separated by a dash.

Each of the configured systems contains a `params` object containing multiple
key-value pairs. These should be set as the HTTP GET query parameters for the
API requests for the respective language pair.

An example of a translation request url when translating from Latvian to Lithuanian
could look like this

```
https://letsmt.eu/ws/service.svc/json/TranslateEx?systemID=smt-f4b008c1-3152-444b-91f7-2472e0b69b63&options=client%3Dlocalhost%2Cversion%3D1.0%2Cqe&text=Hello%20world
# don't forget to set the client-id header as well
```

**Note:** The contents of the `params` object is expected to change over time as new
features might be added to the Tilde MT platform. When setting the API request
parameters one should iterate over *all* of the enclosed key-value pairs.

## Persisting the webcompoent's state
It is recommended to persist the whole webcompoent's state object as a JSON string.
This should be as simple as
```JavaScript
// tildemtConfig is acquired via the webcompoents `save` event's event details
var stateString = JSON.stringify(tildemtConfig);
saveStateJsonInDb(stateString);
```

## Restoring the webcomponent's state
After the webcomponent has initially been configured the user might want to edit
the configuration to, for example, change the selected MT system for one or more
languages, add a system for a new language pair, or remove a system altogether.
To facilitate this the webcomponent provides a method to set its state to a previous
configuration.
```JavaScript
// the previous configuration could be stored and retrieved as JSON from the backend
var stateString = getStateJsonFromDb();
tildemtConfig = stateString ? JSON.parse(stateString) : undefined;
document.getElementById("tildemt").setState(tildemtConfig);
// show the webcomponent to the user; the webcomponent will now reload the previously
// selected system list
```


## Demo
For a full working example see [the demo](../index.html).

# Source code structure
TODO
