THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

Once you have created a BulkLoader instance and added items to it, you can fetch those items by a number of ways.

## Getting content from a loading item ##
The simplest way to get content is using the getContent method.
If you have an item added such as:
```
bulkInstance.add("background.jpg", {id:"bg"});
```
You can later retrieve it with:
```
bulkInstance.getContent("background.jpg")
```
Or you can also use the item id, if you specified one:
```
bulkInstance.getContent("bg")
```

getContent will return an untyped object. If the loading has not completed or failed for some reason it will return null. BulkLoader will trace an error message.

After using getContent you can cast and use you asset:
```
var content : * = bulkInstance.getContent("background.jpg");
var bgBitmap : Bitmap = content as Bitmap;
addChild(bgBitmap);
```

Fetching content functions (see below) all use the same signature:
get**(key :**, clearsMemory : Boolean = false);
key -> Can be an url as a string, an id as a String or a URLRequest object that was specified when adding the item
clearsMemory -> A boolean that defaults to false. If true, BulkLoader will remove all references to that asset and subsequent calls to use that item or it's content will fails. This is useful for memory management.

### Using the convenience functions for automatic casting ###
You can ask BulkLoader to return some asset in the expected format. This is just a convenience so that casting is not required.

If you have an item added such as:
```
bulkInstance.add("background.jpg", {id:"bg"});
```
You can later retrieve it with:
```
bulkInstance.getBitmap("background.jpg"); // returns a flash.display.Bitmap object
bulkInstance.getBitmapData("background.jpg"); // returns a flash.display.BitmapData object
```

If the casting fails BulkLoader will trace a debug error message and will return null;


These are the available convenience methods:
  * getBitmap -> returns a flash.display.Bitmap object. Works on items with type "image".
  * getBitmapData -> returns a flash.display.BitmapData object. Works on items with type "image".
  * getXML -> returns a XML object. Works on items with type "xml".
  * getText -> returns a String object. Works on items with type "text".
  * getMovieClip -> returns a MovieClip object. Works on items with type "swf".
  * getSound -> returns a Sound object. Works on items with type "sound".
  * getNetStream -> returns a NetStream object. Works on items with type "video".
  * getNetStreamMetaData -> returns an Object with the same properties as a NetStream metadata object. Works on items with type "video".
  * getSerializedData -> returns an Object serialized by the function encodingFunction (second parameter)
