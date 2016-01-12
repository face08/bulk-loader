THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

BulkLoader events are divided into two categories:
  * Events for all assets as a group.
  * Events for individual items.

## Events for all assets as a group ##
These events listeners are always added to the BulkLoader instance and relate to items added as a group.

### Complete ###
An event of name "complete" (BulkLoader.COMPLETE or Event.COMPLETE) is fired when all items have finished loading.
The event type is a BulkProgressEvent (which subclasses ProgressEvent), so your event handler can receive a Event, ProgressEvent or a BulkProgressEvent object. Example:
```
var bulkInstance = new BulkLoader("main");
bulkInstance.add("main.swf");
bulkInstance.add("config.xml");
bulkInstance.add("background.jpg");
bulkInstance.addEventListener(BulkLoader.COMPLETE, onAllLoaded);

function onAllLoaded(evt : BulkProgressEvent):void{
    // all items have been loaded!
    trace(evt.
  trace(evt.loadedPercent) ; // percentage will be 1)
    var bg : Bitmap = bulkInstance.getBitmap("background.jpg");
    addChild(bg);
}
```

A few observations about the complete event:
  * If any item fails to load this event will never fire.
  * If you add an item to the same BulkLoader instance after the complete event has fired, bulk loader will load the new item and when it's loaded will fire the complete event again.
  * Before fireing the complete event, BulkLoader will always fire a progress event where all items are loaded.

## Progress ##
When any asset fires a progress event, BulkLoader will fire the progress (BulkLoader.PROGRESS, ProgressEvent.PROGRESS) event.
The event name is : "progress" (BulkLoader.PROGRESS, BulkLoader.PROGRESS).
The event type is a BulkProgressEvent). See the ReportingLoadingStatus page for more information.

## Error ##
If any of the assets fail to load, BulkLoader will fire an "error" event.

The event name is "error" (ErrorEvent.ERROR).
The event type is ErrorEvent for both SecurityError and IOError instances.

Error events can be listened to for each item in separate, or full the entire BulkLoader instance. From whithin your error handling method you can obtain more information about the event.

Simple example:
```
bulkInstance.addEventListener(BulkLoader.ERROR,onError);

function onError(evt :ErrorEvent ) : void{
	var item : LoadingItem = evt.target;
        trace (item.errorEvent is SecurityErrorEvent); 
        trace (item.errorEvent is IOError); 
       trace (evt); // outputs more information 
}
```

The error event is only fired after the item has the number of retries especified on the maxRetries option (AvailableOptions#maxTries)

Error events will fire as soon as they happen.

## Events for individual items ##
Besides listening for events on all items as a whole, you can listen to event for each item.

To add an event for an item, first grab a reference to that item:
```
var bulkInstance = new BulkLoader("main");
bulkInstance.add("main.swf");
bulkInstance.add("background.jpg", {id:"bg"});

// add an event for the background item by the id:
bulkInstance.get("bg").addEventListener(...);
// or by the url:
bulkInstance.get("background.jpg").addEventListener(...);
```

The folowing events are available:

## Complete ##
An event of name "complete" (BulkLoader.COMPLETE or Event.COMPLETE) is fired when an item has finished loading (and succeeded).
The event type is  Event.

This code will attach a "background" jpeg as soon as that jpeg is loaded but before all items have been loaded:
```
var bulkInstance = new BulkLoader("main");
bulkInstance.add("main.swf");
bulkInstance.add("background.jpg", {id:"bg"});

// add an event for the background item by the id:
bulkInstance.get("bg").addEventListener(...);
// or by the url:
bulkInstance.get("background.jpg").addEventListener(Event.COMPLETE, onBackgroundLoaded);

function onBackgroundLoaded(evt : Event) : void{
     // the background is loaded, you can get now attach it:
    var bgBitmap : Bitmap = bulkInstance.getBitmap("background.jpg");
    addChild(bgBitmap);
}
```

## Error ##
An event of name "error" (BulkLoader.ERROR ) is fired when an item has failed to load.
The event type is  ErrorEvent.

## Progress ##
An event of name "progress" (ProgressEvent.PROGRESS ) is fired when the player has received more content for that item.
The event type is ProgressEvent.

## HttpStatus ##
You can also listen for HTTPStatusEvent for each item. Although the player is not very reliable on informing this, it can be useful.

An event of name "httpStatus" (BulkLoader.HTTP\_STATUS  ) is fired when an item has failed to load.
The event type is  HTTPStatusEvent.

## Open ##
An event of name "open"(BulkLoader.OPEN) is fired when a connection has been opened. In streaming items (sound and video) the content can be used right away.

## CAN\_BEGIN\_PLAYING ##
An event of name "canBeginPlaying" (BulkLoader.CAN\_BEGIN\_PLAYING) is fired for video objects when the download speed is enough that it should be possible for video playback to begin without any interruptions. This event will only fire once.

## SecurityError ##
BulkLoader can handle SecurityErrorEvents. By listening to BulkLoader.SECURITY\_ERROR("securityError"), client code can gracefully handle security error events.
Listeners can be added to each LoadingItem subtype or to the entire BulkLoader object. Please note that if you add an event listener for a LoadingItem but not for BulkLoader, in case the event fires, it will bubble up and result in a unhandled security error. In order to make sure the error never goes unhandled, add a securityError handler to the BulkLoader instance.

This mechanism will catch events thrown on 

&lt;someLoader&gt;

.load operations.
Event thrown when trying to access content (such as getBitmapData) is still left to client code to handle.

When using type=image or type=movieclip (which are loaded using a Loader instance), if, when the asset is loaded, accessing it's loader.content throws an error, then BulkLoader will gracefully recover and use a simple Loader instance. This way, you can still add a Loader object to the stage, but won't be able to run code inside of it (for MovieClip instances) or load it as data (transforming to BitmapData instances).

In this case, you can get the Loader instance either using getContent or getDisplayObjecLoader .