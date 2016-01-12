THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

BulkLoader must now what type of asset you are loading. For the most common cases it will discover from the url passed. So when loading a "background.jpg" BulkLoader knows that it must use a Loader instance internally. There are two situations when that will not work:

a) The url offers no clues about the file type:
Maybe you are using a web-service or some server script that has a non-revealing url such as:
"http://mysite.com/top-ten.php"
BulkLoader doesn't know what the php script will return, so you can specify with the "type" option.
```
// returning a simple text
bulkInstance.add("http://mysite.com/top-ten.php", {type:"text"});

// returning a xml file
bulkInstance.add("http://mysite.com/top-ten.php", {type:"xml"});

// returning a jpeg image:
bulkInstance.add("http://mysite.com/top-ten.php", {type:"image"});
```

b) You are loading a file with an unknown type (to BulkLoader)
If you are loading a file with an extension that is unknown to BulkLoader, you should specify a type, or else BulkLoader will load that item as text.

## Registering new extensions ##
You can also add new extensions, so that BulkLoader will guess the correct type.
Suppose tomorrow the flash player is able to parse wmv video files, than you can say:
```
BulkLoader.registerNewType("mwv", "WMV-video", MWVVideoItem);
```
The general form is:
```
BulkLoader.registerNewType(extension : String, loaderType : String, class : Class);
```
Where class is a subclass of LoadingType than can load that specific type.

## Available types ##
According to the type of object you are loading, bulk loader does some tricks. Video objects will retain their metadata, sound and video can be accessed as soon as loading begins and so forth.

|Property name| Extensions | Helpers| Oberservations|
|:------------|:-----------|:-------|:--------------|
|TYPE\_MOVIECLIP | swf        | helpers: getMovieClip |               |
|TYPE\_IMAGE  | jpg, jpeg, gif, png | helpers: getBitmap, getBitmapData |               |
|TYPE\_SOUND  | mp3        | helpers: getSound | Content is available as soon as the download starts |
|TYPE\_TEXT   | txt, php, xml, py, asp | helpers: getText, getSerializedContent |               |
|TYPE\_VIDEO  | flv        | helpers: getNetStream, getNetStreamMetaData | Content is available as soon as the download starts |

## Observations ##
Two things to keep in mind:
a) If BulkLoader can't guess from the url extension and no type has been passed as an option, it defaults to a text type.
b) If you specify a "type" option it will override the automatic guessing from the url