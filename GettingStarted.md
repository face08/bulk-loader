THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

# Getting Started #

A quick how to guide to getting started with _BulkLoader_.

Using BulkLoader is simple, the workflow is very close to how you'd handle any loading operation in As3.
  1. Create a BulkLoader instance
  1. add the url(s) to be loaded
  1. add event listeners for all items together or for individual items
  1. start the loading operation
  1. retrieving content

# Creating a BulkLoader instance: #
```
// creates a BulkLoader instance with a name of "main-site", that can be used to retrieve items without having a reference to this instance
var loader : BulkLoader = new BulkLoader("main-site");
```
# Add urls to load: #
```
// simplest case:
loader.add("logo.png");
// use an "id" so the item can be retrieved later without a reference to the url
loader.add("background.jpg", {id:"bg"});
// since the url by itself can't tell us what the filetype is, use the type property to let BulkLoader know what to do:
loader.add("/some-web-services?size=Large", {type:"image"});
// add an item that should be loaded first (higher priority):
loader.add("/data/config.xml", {priority:20});
// add a max tries number (defaults to 3)
loader.add("/unreliable-web-services.xml", {maxTries:6});
// you can also use a URLRequest object , this will load from a POST request
var postRequest : URLRequest = new URLRequest("/save-prefs.php");
postRequest.method = "POST";
var postData : URLVariables = new URLVariables(myPostDataObject );
postRequest.data = postData;
loader.add(postRequest, {"id":"settings"});
// of course, all options can be combined:
loader.add("the-sound-webservices?name=maintrack", {"id":"soundtrack", type:"sound", maxTries:1, priority:100});
```
# Listening for events: #
```
// attaching events to all items:
// this will fire once all items have been loaded
loader.addEventListener(BulkLoader.COMPLETE, onAllLoaded);
// this will fire on progress for any item
// the event , BulkProgress is a subclass of ProgressEvent (with extra information)
loader.addEventListener(BulkLoader.PROGRESS, onAllProgress);
// this will fire if any item fails to load:
// the event is BulkErrorEvent and holds an array (errors) with all failed LoadingItem instances
loader.addEventListener(BulkLoader.ERROR, onAllError);                                      
// you can also listen to events in individual items               
// this will fire as soon as the item registred with the id of "bg" is done loading (even if there are other items to load)
loader.get("bg").addEventListener(Event.COMPLETE,onBackgroundLoaded)
// this will only trigged if the config.xml loading fails:
loader.get("data/config.xml").addEventListener(BulkLoader.ERROR, onXMLFailed)
```
# Starting the loading operation: #
BulkLoader will only begin loading once you call the start method:
```
loader.start();
```

# Retrieving content: #
You can retrieve an untyped object and cast it your self:
```
var theBgBitmap : Bitmap = loader.getContent("bg") as Bitmap;
// you don't need to keep a reference to the loader intance, you can get it by name:
var theBgBitmap : Bitmap = BulkLoader.getLoader("main-site").getContent("bg") as Bitmap;
// you can also use the conviniece methods to get a typed object:
var theBgBitmap : Bitmap = loader.getBitmap("bg");
// grab a BitmapData directly:
var theBgBitmap : Bitmap = loader.getBitmapData("bg");
```
Other conviniece functions: getXML, getText, getNetStream, getSound, getMovieClip, getNetStreamMetaData.

You can retrive an item using it's id, it's url as a String or as the URLRequest object (if the item was created with an URLRequest).